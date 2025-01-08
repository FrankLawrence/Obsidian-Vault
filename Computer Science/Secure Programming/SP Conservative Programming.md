---
Tags: 
Created: 2023-10-03 00:28:50
Links: "[[Secure Programming]]"
---
(Links:: [[Secure Programming]])
# Handling User Input
User input is the main attack surface of most programs. Trying weird inputs is the first thing attackers will try. Data retrieved from files/databases previously provided by the user is equally risky. 
In such cases we can use: [[#Sanitization]], [[#Escaping]], or [[#Authorization]].
## Sanitization
Any user input should be **sanitized** as soon as possible:
- Verify that data follows the expected structure
- Verify that any values are within reasonable range

Immediately reject inputs that are not suitable:
- Do not process them any further
- Ideally, do not echo problematic input in error message. Doing so can sometimes be abused, for example in a reflected [[XSS attck]](Cross-site scripting).
- Proper sanitization gives attacker far less room to maneuver

> [!example] Examples
> - Set reasonable limits for string lengths: 
>   - Name probably does not need more than 1,000 chars
>   - Empty strings often invalid, and can cause trouble
> - Ensure strings contain no unexpected characters
>   - Name should probably not contain < or >
> - Verify whether numbers are in a reasonable range
>   - People rarely legitimately order 100 pizzas
>   - Beware of integer overflows when Checking
> - Some inputs may only take values from a specific, ex: country names
> - Veryify strings that follow a fixed format
>   - Dates, e-mail addresses, URLs, bank accounts, ZIP codes,...
> - Dates often only in past or only in future
>   - Birthdates range from 122 years ago to today
> - Enforce contraints between multiple inputs
>   - Departure date not before arrival date

> [!example] Counterexamples
> Be careful not to overdo
> - King of Netherlands' name including titles >700 chars
> - In many languages, names can include non-alphabet characters
> - Deceased people can be born far in the past
> - Country of birth could the Soviet Union or Ottoman Empire
> - Country of residence might be unrecognized
> - Different countries have different ZIP code formats
> - Some people *really* love pizza

### How to Sanitize
- Keep it simple
  - Vlunerable sanitization code defeats the purpose
  - Usually a simple check or regular expression suffices
- Parsers have large attack surface
  - Apply sanitization pass before parser
- Prefer allowlists (whiteilsts) over blocklists (blacklists)
  - Speicfy what is allowed rather than what is forbidden
  - Secure by default, less likely to overlook risks
  - However, more risk of false positives
- Consider context when sanitizing
  - Keep in mind data may be escaped
    - `&lt;script&gt;alert(&quot;all your base are belong to us&quot; &lt;/script&gt;`
    - `%3Cscript%3Ealert%28%22all%20your%20base%20 are%20belong%20to%20us%22%29%3C/script%3E`
  - Think of later processing for leading/trailing characters
    - Removing whitespace can make non-empty string Empty
    - Leading zeroes make number take more space than expected
    - Consider this when sanitizing
### Where to Sanitize
- Most websites sanitize inputs in Javascript
  - Quick response incase, for example, a value is missing or an e-mail address is incorrect
- This does nothing to protect the server
  - Javascript can be changed on the client side
  - Browser can be bypassed, sending requests straight to server
- Each application compartment performs its own sanitization
  - Even when the data was already sanitized on the other end

> [!question]- Find the Vulnerabilities
> ```c
> int user_is_allowed(sqlite3 *db, const char *name, const char *pwd){
>   int found, r;
>   char sql[256];
>   sqlite3_stmt *stmt;
>   /* build query */ 
>   sprintf(sql, "SELECT userid "
>       "FROM user WHERE name='%s' "
>       "AND pwd='%s'", name, pwd);
>   r = sqlite3_prepare_v2(db, sql, -1, &stmt, NULL);
>   if (r != SQLITE_OK) goto error;
>  /* execute query */
>   switch (sqlite3_step(stmt)) { 
>     case SQLITE_DONE: found = 0;
>                       break;
>     case SQLITE_ROW:  found = 1;
>                       break;
>     default:          goto error;
>   }
>   sqlite3_finalize(stmt);
>   return found;
> 
>   error:
>     fprintf(stderr, "db error: %s\n", sqlite3_errmsg(db));
>   if (stmt) sqlite3_finalize(stmt); return -1;
> }
> ```
> > [!tip] Solution
> > - Buffer overflow for long `name` and/or `pwd`
> > - [SQL Injection] if `name` and/or `pwd` contains single quote

## Escaping
- We often place user input in context where it is interpreted
- Some (sequences of) characters have special meaning
  - Text in HTML: <,>,and &
  - String in Javascript: " and \
  - Often also control characters, especially newline
- Escaping encodes these characters to avoid interpretation 
  - Critical defense against injection attacks

> [!example]
> - Escaping sequences depend on language
>   - Text in HTML: `&lt;`, `&gt;`, and `&amp;`
>   - String in Javascript: \" and \\
> - Escaping also context-dependent
>   - Attribute in HTML also needs " escaped
>   - URL also needs / escaped for single path component

### how to Escape
- Escaping rules are often more complex than they seem 
  - Every obscure syntax and context must be handled
- Escaped strings are typically longer than the original
  - Beware of buffer overflows while escaping
- Therefore: use standard library functions whenever possible
- Multiple layers may be needed
  - Example: a string in Javascript used to generate HTML

> [!example]- How to Escape Examples
> | Language   | HTML                    | Javascript string                    | URL                   |
> | ---------- | ----------------------- | ------------------------------------ | --------------------- |
> | C#         | `WebUtility.HtmlEncode` | `HttpUtility.JavaScriptStringEncode` | `Uri.EscapeUriString` |
> | Javascript | (use DOM)               | `JSON.stringify`                     | `encodeURI`           |
> | PHP        | `htmlspecialchars`      | `json_encode`                        | `url_encode`          |
> | Python     | `html.escape`           | `json_dumps`                         | `urllib.parse.quote   |

- SQL injection major security threat due to incorrect escaping
- Best approach: let database handle it
  - Prepared statements: provide a SQL string (fixed) containing placeholders and a list of values to be inserted at the placeholders
  - Stored procedures: create functions on the server in SQL and pass list of parameters for function to use
- Comparison
  - Both protect against SQL injection
  - Prepared statements usually more convenient
  - Stored procedures allow more control over what program can do

#### Prepared Statements in C/SQLite 
```c
int user_is_allowed(sqlite3 *db, const char *name, const char *pwd){
  int found, r;
  const char *sql;
  sqlite3_stmt *stmt;
  /* build query */
  sql = "SELECT userid "
      "FROM user WHERE name=@name "
      "AND pwd=@pwd";
  r = sqlite3_prepare_v2(db, sql, -1, &stmt, NULL);
  if (r != SQLITE_OK) goto error;
  r = sqlite3_bind_text(stmt, 1, name, -1, SQLITE_STATIC);
  if (r != SQLITE_OK) goto error;
  r = sqlite3_bind_text(stmt, 2, pwd, -1, SQLITE_STATIC);
  if (r != SQLITE_OK) goto error; /*...*/
}
```
- **Parameters set by index (some other database APIs work by name)**
#### Stored Procedures in PHP/MySQL
```SQL
DELIMITER //
CREATE PROCEDURE UserCheckPassword(
  nameparam CHAR(128),
  pwdparam CHAR(128))
BEGIN
SELECT userid FROM user WHERE name=nameparam AND pwd=pwdparam;
END // DELIMITER ;
```
```PHP
function UserCheckPassword($name, $pwd) {
  $db = mysqli_connect($mysql_host, $mysql_user, $mysql_password, $mysql_db);
  if ($db === false) die(mysqli_connect_error());

  $stmt = mysqli_prepare($db, "CALL UserCheckPassword(?, ?)");
  if ($stmt === false) die(mysqli_stmt_error($db));

  $r = mysqli_stmt_bind_param($stmt, "ss", $name, $pwd);
  if ($r === false) die(mysqli_stmt_error($stmt);

  $r = mysqli_stmt_execute($stmt);
  if ($r === false) die(mysqli_stmt_error($stmt));

  $r = mysqli_stmt_fetch($stmt);
  if ($r === false) die(mysqli_stmt_error($stmt));

  mysqli_stmt_close($stmt);
  mysqli_close($db);
  return $r
}
```
#### How to Escape Shell Commands
- Don't pass user input to the shell. It is hard to escape properly, dependent on environment and different between different shell implementations
- If you must invoke another program with user input, avoid convenience functions that use the shell
- Instead, use functions where you pass array of arguments
	- C/C++: `fork` together with `execv` or `execl` (UNIX) or `CreateProcess` (Win)
	- Python: `subprocess.call`
- You still have to consider how program interprets arguments
	- Usually `-` starts an option with special meaning (`/` for windows)
	- `. .` can be used to break out of directories
### Authorization
- Authorization: ensuring authenticated users can only access resources when allowed for them
- Never assume the requested resource is one the user is allowed to access
- Moreover, permission may be revoked between requests
- Verify user's permissions to every resource on every access
#### Intrusion Detection
- Validation may not be sufficient to find all malicious inputs
	- Spam e-mails
	- Malicious apps in app store
	- Attempts of denial of service by overloading server
	- Attempts to scan network for vulnerable systems
- We can use heuristics to identify suspicious inputs
	- How exactly is context-dependent and out of scope
	- It is important to consider what happens if we get it wrong
> [!question] Which intrusion detection is better?
> - A: rejects 100? of malicious, 1.0% of benign
> - B: rejects 50? of malicious, 0.5% of benign
> 
> > [!tip]- Solution
> > - Preference depends on circumstances
> > 	- Threat level
> > 	- How critical is service

#### False Positives and Negatives

|           | Accept         | Reject         |
| --------- | -------------- | -------------- |
| Benign    | true negative  | false positive |
| Malicious | false negative | true positive  |

#### Comparing Intrusion Detection
![[Percom 8 Classification III#Confusion Matrix]]
$$\text{F1 score }=2\times \frac{\text{precision} \times \text{ recall}}{\text{precisoin } + \text{ recall}}$$

> [!example]
> **System A**
> Precision: 9.09%
> Recall: 100%
> F1 score: 16.67%
> **System B**
> Precision: 9.09%
> Recall: 50%
> F1 score: 15.38%

### Machine Learning
- As machine learning is increasingly used in production systems, input validation risks are starting to show
- Think about a system classifying traffic signs
	- What if inputs are crafted to look like one sign to humans, but as another to machine learning algorithms (**evasion attack**)?
	- What if malicious inputs included in training set teach system to classify in the wrong way (**poisoning attack**)?
	- These issues are known as **adversarial machine learning**
#### Poisoning Attack
- Spam filters have long relied on simple machine learning
	- Match e-mails against characteristics more commonly found in known relevant e-mails or in know spam e-mails
	- Example: Viagra and Nigerian princes likely marked as spam
- Problem: people do not want to miss any relevant e-mail
	- False positives quickly convince users to disable spam filter
- Attack: send lots of spam with 'relevant' words
	- Increase false positive rate
#### Adversarial Machine Learning
- Hard problem to solve
	- Typical algorithms do not indicate *why* choice was made
	- Instead we get likelihood score for each option and pick the best
- Found a solution?
	- Attacker can train another system to find its weaknesses
- Be careful using machine learning for safety critical purposes
# Checking Assumptions
- When programming, we constantly make assumptions

> [!example] 
> - This buffer can store at least 42 more bytes
> - This string holds the path to a file we are allowed to read
> - A person's age always fits in three characters
> - This object has not been freed yet

- Assumptions like these can threaten security if violated
## It is Easy to get Assumptions wrong
- We changed the part of the code ensuring assumption holds
	- Change buffer size somewhere, forgot to update elsewhere
- We overlooked dependence on the environment
	- User can place a symlink at the path
- We did not consider that data may be incorrect
	- User mistyped data of birth as something unrealistic
- We forgot to consider different order of events
	- Object is freed if user deleted the file before asking for its details

## Check Your Assumptions!
- Make your assumptions explicit by including checks
- `assert(expression)` kills the program with an error message if `expression` is false
	Suitable to document and check assumptions
- Helps find problems early when debugging or testing
- Can be disabled with `-DNDEBUG` for better performance

> [!tip] Security advice: leave enabled even when releasing software to find unexpected conditions that indicate bug or attack

> [!example] Assert examples
> ```c
> void age2str(char *buf, int age) {
> 	assert(age >= 0);
> 	assert(age < 125);
> 	sprintf(buf, "%d", age);
> }
> ```
> ```csharp
> string age2str(int age) {
> 	System.Diagnostics.Debug.Assert(age >= 0);
> 	System.Diagnostics.Debug.Assert(age < 125);
> 	return age.ToString();
> }
> ```
> ```python
> def age2str(age):
> 	assert age >= 0
> 	assert age < 125
> 	return str(age)
> ```

### Assumptions on Untrusted Data
- Do not make **any** assumptions on untrusted data
	- Input entered by user
	- Data read from disk
	- Data received from network
- Functions to read such data must verify correctness and return error code/raise exception in case of errors
	- Unlike `assert` failure, this is expected possibility (not a bug in the program) and should give error message rather than crash
	- The checks must not be disabled in release builds
## Order of Operations
- Order of operations is a common source of bad assumptions
	- User may give commands in unexpected sequence
	- Environment may change right after check (TOCTTOU)
	- Thread interleaving can unexpected
	- Signal may get delivered at bad time (just before blocking call)
- Time between two operations can be arbitrarily long
	- OS may pre-empt thread at any time
- Even `count+=1` may be multiple operations
```
# note: %eax is a register in the CPU  
# note: the compiler decided to store variable "int count“ 3 
# at memory address -0x4(%rbp)  
mov -0x4(%rbp), %eax   # read count from memory into %eax  
add $0x1, %eax         # increment the register  
mov %eax, -0x4(%rbp)   # store %eax back into memory
```
- Something could happen between reading and writing `count`

| count | thread1 | thread2 |
| ----- | ------- | ------- |
| 0     | read 0  |         |
| 0     | add 1   |         |
| 0     |         | read 0  |
| 0     | store 1 |         |
| 1     |         | add 1   |
| 1     |         | store 1 |
| 1     |         |         |

- Always verify whether system is in expected state before executing command, and fail if it is not
	- Example: do not edit current record with no record selected
- Any interactions with the file system, network, ... might see different state than the previous one
	- Use system calls with file descriptor rather than system calls with paths (example: `open+fstat` instead of `stat`)
- Getting threads or shared memory right is very hard
	- Even when using locks

# Handling Errors
> [!example] Programming errors without bugs
> - File not found
> - File/device already in use by another program
> - Hardware I/O errors
> - Incorrect user input
> - Insufficient permissions
> - Network down
> - Ran out of memory/disk space
> - Many others...

- Almost all calls can fail, usually in multiple ways
## Error Handling Goals
- Even if an error happens we should ensure
	- Any resources (memory, open files, mutexes, network connections, temporary files, ...) are still released
	- Persistent state (files/databases) remains consistent
	- Program continues to work (if possible)
	- Program remains secure
	- Program state remains consistent
- Usually, it is also desirable to inform the user and/or log the error for later analysis or debugging
## How not to Handle (most) Errors
- Terminating immediately (common in PHP) leaves no way to clean up resources and persistent state
  ```php
  $handle = fopen('myfile.txt', 'r') or die('open failed');
  ```
- Ignoring is easy in C but means state will be inconsistent with expectations, which may threaten security
  ```C
  FILE *file = fopen("myfile.txt", "r");
  fread(buf, sizeof(buf), 1, file); // buf uninitialized
  ```
- Some languages actively encourage bad programmers
  `On Error Resume Next`
## Reporting Errors
- Functions report errors by
	- Return value (C,PHP, UNIX API, Windows API)
	- Exception (C++, C#, Java, Javascript, Python)
	- Error code in memory (many third-party libraries)
## Handling Error Return Codes
- Traditionally, return code indicates errors
	- Pointer NULL for C standard library
	- Integer -1 for UNIX API
	- Boolean FALSE for Windows API
- Handling errors is hard
	- Remember to always check for errors
	- The number of error handling paths grows quickly with number of calls that may fail
	- Each path introduces possibility of incorrect cleanup

> [!example]- Getting a Password from the Database
> ```c
> int get_pwd(const char *dpath, const char *name, char **pwd_p) { 
> 	sqlite3 *db;  
> 	const char *sql;  
> 	sqlite3_stmt *stmt;
> 	/* open database */
> 	sqlite3_open(dbpath, &db); /* prepare query */  
> 	sql = "SELECT pwd FROM user "
> 	  "WHERE name = @name";
> 	sqlite3_prepare_v2(db, sql, -1, &stmt, NULL);
> 	sqlite3_bind_text(stmt, 1, name,-1,SQLITE_STATIC); /* execute query */  
> 	switch (sqlite3_step(stmt)) { 
> 		case SQLITE_DONE: /* no data */
> 		  *pwd_p = NULL;
> 		  break;
> 		case SQLITE_ROW: /* data found */
> 			*pwd_p = (char *)
> 			sqlite3_column_text(stmt, 0);
> 			break;
> 	}
> 	/* clean up */
> 	sqlite3_finalize(stmt);
> 	sqlite3_close(stmt);
> }
> int main(int argc, char** argv) {
> 	char *pwd;
> 	get_pwd("test.db", "erik", &pwd);
> 	printf("pwd: %s\n", pwd);
> 	return 0;
> }
> ```
> Error handling needed
> - Report to user
> - Clean up
> - Abort operation

> [!warning] Handling Errors comes with drawbacks
> - 6x as many statements
> - It gets worse as more resources are allocated
> - And we still ignore finalize and close errors
> 
> > [!info] The dreaded `goto` to the rescue!
> > - Amount of code no longer grows dramatically with number of calls
> > - Initialization is now critical: Label `cleanup` can be reached through multiple paths
> > 
> > > [!example]-
> > > ```c
> > > int get_pwd(const char *dbpath,
> > > 	const char *name, char **pwd_p) {
> > > 	sqlite3 *db = NULL;
> > > 	int r = SQLITE_OK;
> > > 	sqlite3_stmt *stmt = NULL;
> > > 	/* ... */
> > > 	r = sqlite3_bind_text(stmt, 1, name, -1, SQLITE_STATIC);
> > > 	if (r != SQLITE_OK) goto cleanup;
> > > cleanup:
> > > 	if (r != SQLITE_OK)
> > > 	  fprintf(stderr, "database error: %s\n", sqlite3_errmsg(db));
> > > 	if (stmt) sqlite3_finalize(stmt);
> > > 	if (db) sqlite3_close(db);
> > > 	return (r == SQLITE_OK) ? 0 : -1;
> > > }
> > > ```

We can also separate code into smaller functions which: improves readability, and each function has less resources to worry about. Usually 0 indicates a *success* and -1 indicates an *error*.
## Reference Parameters
```c
*pwd_p = (char *)sqlite3_column_text(stmt, 0);
```
In this code, when `sqlite3_finalize` is called, `pwd_p` becomes a **dangling pointer**. Using `strdup()` the *caller* becomes the owner and must free `pwd`.
```c
*pwd_p = strdup((char *)sqlite3_column_text(stmt, 0));
```
Pass by reference is also unsafe, since the value can freed in the callee's code and freed again in the callers code.
> [!important] Always initialize a reference parameter
> ```c
> *pwd_p = NULL;
> ```

Initialization at start of function prevents uninitialized values in case function fails before assigning reference parameter. Now `main` can always safely free the pointer.
> [!example] C++ can do cleanup automatically
> ```cpp
> class Database {
> 	public:
> 		int open(const char *dbpath) {
> 		    int r = sqlite3_open(dbpath, &conn);
> 		    if (r != SQLITE_OK) {
> 			    fprintf(stderr, "open database %s failed: %s\n",
> 					dbpath, sqlite3_errmsg(conn));
> 			    return -1;
> 			}
> 		return 0;
> 		}
> 	~Database() {
> 		if (conn) sqlite3_close(conn);
> 	}
> 	sqlite3 *conn = NULL;
> };
> int get_pwd(const char *dbpath, const char *name, char **pwd_p) {
> 	Database db;
> 	if (db.open(dbpath) != 0) return -1;
> 	/* ... */
> 	return 0;
> }
> ```
> - Any return from `get_pwd` automatically calls destructor
> - This is known as *Resource Acquisition Is Initialization* (RAII)
> - Usually constructor opens, but it cannot return error code

## Exceptions
Exceptions provide a better alternative in most languages. Whenever an exception is thrown, execution continues at the exception handler of the most recent open try block.
- Code between exception throw and exception handler is skipped
- try blocks also allow to catch *specific* exceptions

> [!warning] Code can be skipped: account for cleanup!

> [!example]+
> 
> ```cpp
> class ExceptionA : public std::exception
> {};
> class ExceptionB : public std::exception
> {};
> 
> void func2() {
> 	try {
> 		std::cout << "func2-A\n"; 
> 		throw ExceptionA(); 
> 		std::cout << "func2-B\n";
> 	} catch(const ExceptionB &e) {
> 	    std::cout << "func2-C\n";
> 	}
> }
> void func1() {
>   std::cout << "func1-A\n";
>   func2();
>   std::cout << "func1-B\n";
> }
> 
> int main(int argc, char **argv) { 
> 	try {
> 	    std::cout << "main-A\n";
> 	    func1();
> 	    std::cout << "main-B\n";
> 	} catch(const std::exception &e) {
> 	    std::cout << "main-C\n";
> 	}
> }
> ```
> > [!question]- What gets printed?
> > ```
> > main-A
> > func1-A
> > func2-A
> > main-C
> > ```

Attackers love errors messages; segmentation fault, 500 internal server error,... are usually a sign that there may be a vulnerability. 
# Memory Management
Programmers are often tasked with:
1. Allocating right amounts of memory
2. Avoiding reading or writing past allocation boundaries
3. Initializing memory before reading it
4. Reading data from memory with the same type as it was written
5. Deallocating in time to avoid memory leaks
6. Ensuring memory is not used after deallocation

> [!info]+ Types of Memory
> # Global Memory
> - Used for global and static variables
> - Allocated by linker for lifetime of program
> - No manual management needed
> 
> # Stack Memory
> - Used for local non-static variables
> - Allocated by the compiler for lifetime of function
> - No manual management needed
> 
> # Heap Memory
> - Used for dynamic memory allocations
> - Allocated explicitly with `malloc` (and friends) and `new`
> - Programmer must ensure size and lifetime matches needs

> [!example]- Allocating Memory
> ```c
> struct data {
> 	char a;    // 1 byte
> 	long b;    // 8 bytes
> 	short c;   // 2 bytes
> 	int d;     // 4 bytes
> }
> sizeof(struct data) // 24 bytes
> ```
> ```
> |0|1|2|3|4|5|6|7||0|1|2|3|4|5|6|7||0|1|2|3|4|5|6|7|
> |a|             ||       b       || c |   |   d   |
> ```

The main issue is having to allocate enough and the right amount of space for data. This it is **advised** to use the stack over the heap (e.g. in c++ use `new`). **Don't** use fixed size numbers to `malloc` but use `sizeof`! Don't forget to check the return value of `malloc`.
## Respecting Object Bounds
- some functions ignore boundaries: `strcpy` copies until null terminator
- `strncpy` requires size of destination buffer: `strncpy(buf, argv[1], sizeof(buf));`
	- does not copy null terminator if there is no space
- `snprintf` does secure string formatting and always writes a null terminator -> string *shortened* if too long
- `strdup` allocates flexible amount of memory on the heap -> must be *freed*

> [!warning] Avoid functions with no boundary checks!
> Example: `strcpy`, `strcat`, `gets`, `sprintf`
> Replacements: `strncpy`, `strncat`, `fgets`, `snprintf`
## Memory Initialization

> [!question]- Find the Bug
> ```c
> void example(int authenticated, const char *auth_username) {
> 	char username[256];  
> 	if (authenticated)
> 	    strncpy(username, auth_username, sizeof(username));
> 	/* ... */  
> 	if (strlen(username) > 0)
> 	    perform_operation_for(username);
> }
> ```
> > [!info]- Solution
> > - `username` is not always initialized; program works fine if the first byte happens to be 0
> > - Attacker may be able to make the program initialize this stack location in an earlier function call, forcing `username` to a chosen value

Often variables such as `char *buf[256]` are not initialized, and if *not written to*, can be used to leak information or corrupt memory. We differentiate these types:
1. Global memory automatically initialized to zero (if not specified)
2. Stack memory not automatically initialized
3. Heap memory not initialized by `malloc`, but `calloc` does

> [!tip] Initialize everything at *allocation time*

> [!question]- Find the Bug
> ```cpp
> class ClassA { };
> class ClassB : public ClassA { public: int value; };
> class ClassC : public ClassA { public: char *value; };
> 
> void foo(ClassA *obj) {
> 	ClassC *objC = static_cast<ClassC*>(obj);
> 	const char *str = objC ? objC->value : "no string";
> 	std:cout << str << "\n";
> }
> void bar(int uservalue) {
> 	ClassB obj;
> 	obj.value = uservalue;
> 	foo(&obj);
> }
> ```
> > [!info]- Solution
> > An attacker can specify an integer that will be used as a pointer!

A memory access is **type-unsafe** if the value is read as another type than it was written (occurs often with incorrect type conversion/casting) -> **Avoid casts if possible**.
## Object Lifetime

> [!example]- Freeing memory at wrong time
> ```c
> char *str = strdup(argv[1]);
> printf("%s\n", str);
> //pointer goes out of scope
> ```
> ```c
> char *str = strdup(argv[1]);
> free(str);
> printf("%s\n", str);
> ```

These types of bugs usually cause **Memory Leaks** and [[User-after-free Vulnerability]] (UAF). They occur often in heap memory, however UAF is only applicable in stack memory, and it is not found in global memory.
- Use stack allocations (fixed size)
- Never return pointer to stack variable, and never store it in data structure that survives function
- After `free` set variable to `NULL`
### Smart Pointers in C++
- `std::unique_ptr`
	- Prevents copying pointer
	- Frees object when pointer lifetime ends  
	- Full protection against UAF and mem leaks, but not always possible
- `std::shared_ptr`
	- Keeps track of number of copies of pointer  
	- Frees object when no more copies of pointer exist 
	- Full protection against UAF  
	- Memory leaks possible with circular references
## Integer Overflows
- Can be used to bypass checks or overflow buffers
- Signed integers: result is officially undefined
	- In practice on most CPUs (including x86) it is still mod 2n, but now half of the possible numbers are negative
	- Therefore INT_MAX+1 == INT_MIN and INT_MIN–1 == INT_MAX
	- Note that INT_MAX=2n-1-1 (0x7fffffff) and INT_MIN=-2n-1 (-0x80000000)
- Mitigation:
	- Think about possible inputs
	- Choose types based on these ranges
	- Verify whether each input is in range before converting/storing it
- The right integer type must follow the following rules
	- Fits the full expected range
	- Is ideally the same between values used together in computations
	- Is ideally signed if you perform any subtractions or check before
	- May be larger than strictly required (overhead usually minimal)
- Use safe/efficient [overflow checks](https://gcc.gnu.org/onlinedocs/gcc/Integer-Overflow-Builtins.html) in C/C++


---
References:
