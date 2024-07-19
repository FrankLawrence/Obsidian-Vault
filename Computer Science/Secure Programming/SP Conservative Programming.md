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
> > [!solution]
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
# Checking Assumptions
# Handling Errors
# Memory Management

---
References:
