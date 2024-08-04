---
Tags: 
Created: 2023-10-03 00:28:50
Links: "[[Secure Programming]]"
---
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

> [!question] Find the Vulnerabilities
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

> [!example] How to Escape Examples
| Language   | HTML                  | Javascript string                  | URL                 |
| C#         | `WebUtility.HtmlEncode` | `HttpUtility.JavaScriptStringEncode` | `Uri.EscapeUriString` |
| Javascript | (use DOM)             | `JSON.stringify`                     | `encodeURI`           |
| PHP        | `htmlspecialchars`      | `json_encode`                        | `url_encode`          |
| Python     | `html.escape`           | `json_dumps`                         | `urllib.parse.quote         |

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
| 0     | store 1 |         |
| 1     |         | read 1  |
| 1     |         | add 2   |
| 1     |         | store 2 |
| 2     |         |         |

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
# Memory Management

---
References:
