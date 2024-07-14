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

> [!example] 
> - Set reasonable limits for string lengths: 
>   - Name probably does not need more than 1,000 chars
>   - Empty strings often invalid, and can cause trouble
# Checking Assumptions
# Handling Errors
# Memory Management

---
References:
