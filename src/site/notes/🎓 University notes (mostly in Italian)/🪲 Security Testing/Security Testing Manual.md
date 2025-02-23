---
{"dg-publish":true,"permalink":"/university-notes-mostly-in-italian/security-testing/security-testing-manual/","created":"2025-02-06T08:24:03.625+01:00","updated":"2025-02-23T23:36:50.970+01:00"}
---

# Security Testing Manual

## System Development Life Cycle (SDLC)

The **System Development Life Cycle (SDLC)** is a structured framework that outlines the processes and procedures involved in the development of software and applications. It enables development teams to systematically create, deploy, and maintain software by following a series of well-defined phases. These phases typically include:

- **Planning**: Defining the project scope, objectives, and resources.
- **Requirements Gathering**: Identifying and documenting the functional and non-functional requirements.
- **Design**: Creating architectural and detailed design specifications.
- **Implementation (Building)**: Writing the code and developing the software.
- **Testing**: Verifying that the software meets the specified requirements and is free of defects.
- **Deployment**: Releasing the software to the production environment.
- **Maintenance**: Providing ongoing support and updates post-deployment.

---

## Secure System Development Life Cycle (SSDLC)

The **Secure System Development Life Cycle (SSDLC)** is an extension of the traditional SDLC that integrates security practices at every phase of the development process. The primary goal of SSDLC is to develop software that is resilient to security threats by:

- **Reducing Security Risks**: Identifying and mitigating potential security risks early in the development process.
- **Eliminating Security Vulnerabilities**: Ensuring that the software is free from vulnerabilities that could be exploited by attackers.
- **Reducing Costs**: Addressing security issues during development is significantly less costly than fixing them after deployment.

By embedding security into each phase of the SDLC, organizations can build more secure and robust software systems.

---

## Use-After-Free

### Description
**Use-After-Free (UAF)** is a type of memory corruption vulnerability that occurs when a program continues to use a pointer after the memory it references has been freed. This can lead to undefined behavior, including crashes, data corruption, or even arbitrary code execution. The sequence of events typically involves:

1. The program allocates memory and assigns it to a pointer.
2. The memory is freed, but the pointer is not set to `NULL`.
3. The same memory is reallocated for another purpose.
4. The original pointer is used, leading to access to invalid or unintended memory.

### Prevention
To prevent Use-After-Free vulnerabilities, consider the following best practices:

- **Automatic Memory Management**: Use programming languages that provide automatic memory management (e.g., garbage collection) to avoid manual memory management errors.
- **Nullify Pointers**: Always set pointers to `NULL` after freeing the memory they reference to prevent accidental reuse.

### Detection
Use-After-Free vulnerabilities can be detected through various methods, including:

- **Code Injection**: Techniques such as CAPEC-242 can be used to identify vulnerabilities.
- **Fuzzing**: Automated testing that provides random or malformed inputs to trigger unexpected behavior.
- **Buffer Overflow Analysis**: Tools like CAPEC-100 can help identify related vulnerabilities.
- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.

### Example
```c
char* ptr = (char*)malloc(SIZE);
if (err) {
    abrt = 1;
    free(ptr);
}
...
if (abrt) {
    logError("operation aborted before commit", ptr);
}
```

In this example, the pointer `ptr` is freed when an error occurs, but it is later incorrectly used in the `logError` function, leading to a Use-After-Free vulnerability.

---

## Heap-based Buffer Overflow

A **Heap-based Buffer Overflow** occurs when a buffer allocated in the heap is overwritten due to insufficient bounds checking. This can lead to memory corruption, crashes, or arbitrary code execution. Heap overflows are particularly dangerous because they can be exploited to manipulate the memory layout of a running process.

### Prevention

To prevent heap-based buffer overflows, consider the following strategies:

- **Automatic Buffer Overflow Detection**: Use tools and languages that automatically detect buffer overflows.
    
- **Bounds Checking**: Implement strict bounds checking on all input data.
    
- **Safe Functions**: Replace dangerous functions like `gets` with safer alternatives that perform bounds checking, such as `fgets`.
    

### Detection

Heap-based buffer overflows can be detected using:

- **Fuzzing**: Automated testing that provides malformed inputs to trigger overflows.
    
- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    

### Example

```c
#define BUFSIZE 4
int main(int argc, char **argv) {
    char *buf;
    buf = (char *)malloc(sizeof(char)*BUFSIZE);
    strcpy(buf, argv[1]);
}
```

In this example, the buffer `buf` is allocated with a fixed size, but there is no guarantee that the string in `argv[1]` will not exceed this size, leading to a heap-based buffer overflow.

---

## Out-of-bounds Write

An **Out-of-bounds Write** occurs when a program writes data outside the bounds of an allocated buffer. This can lead to memory corruption, crashes, or arbitrary code execution. Out-of-bounds writes are often the result of improper bounds checking or the use of unsafe functions.

### Prevention

To prevent out-of-bounds writes, consider the following best practices:

- **Bounds Checking**: Ensure that all buffer accesses are within the allocated bounds.
    
- **Safe Functions**: Replace unsafe functions like `strcpy` with safer alternatives like `strncpy` that include bounds checking.
    

### Detection

Out-of-bounds writes can be detected using:

- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    
- **Dynamic Analysis**: Techniques like fuzzing and fault-injection testing.
    

### Example

```c
int id_sequence[3];
id_sequence[0] = 123;
id_sequence[1] = 234;
id_sequence[2] = 345;
id_sequence[3] = 456;
```

In this example, the array `id_sequence` is allocated to hold three elements, but the code attempts to write a fourth element, resulting in an out-of-bounds write.

---

## Improper Input Validation

**Improper Input Validation** occurs when a program fails to validate or incorrectly validates input data. This can lead to security vulnerabilities such as injection attacks, data corruption, or unauthorized access. Input validation is a critical security measure that ensures only safe and expected data is processed by the application.

### Prevention

To prevent improper input validation, consider the following strategies:

- **Validation Frameworks**: Use established input validation frameworks like Struts or OWASP ESAPI.
    
- **Assume All Input is Malicious**: Treat all input as potentially harmful and validate it accordingly.
    
- **Check All Inputs**: Validate all inputs, including parameters, arguments, cookies, and data read from the network.
    

### Detection

Improper input validation can be detected using:

- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    
- **Dynamic Analysis**: Techniques like fuzzing and fault-injection testing.
    

### Example
```java
public static final double price = 20.00;
int quantity = currentUser.getAttribute("quantity");
double total = price * quantity;
chargeUser(total);
```

In this example, the code does not validate the `quantity` input, allowing a negative value to be used. This could result in an account being credited instead of debited.

---

## OS Command Injection

**OS Command Injection** occurs when an application constructs an OS command using externally-influenced input without proper validation or neutralization. This can allow an attacker to execute arbitrary commands on the system, leading to severe security breaches.

### Prevention

To prevent OS command injection, consider the following best practices:

- **Avoid External Processes**: Use library calls instead of external processes to achieve the desired functionality.
    
- **Server-Side Validation**: Ensure that any client-side security checks are duplicated on the server side.
    
- **Safe Argument Passing**: Use input files or standard input to pass arguments instead of command-line arguments.
    

### Detection

OS command injection can be detected using:

- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    
- **Dynamic Analysis**: Techniques like fuzzing and fault-injection testing.
    

### Example
```c
$userName = $_POST["user"];
$command = 'ls -l /home/' . $userName;
system($command);
```

In this example, the `$userName` input is not validated, allowing an attacker to inject arbitrary commands such as `;rm -rf /`, which could delete the entire filesystem.

---

## Deserialization of Untrusted Data

**Deserialization of Untrusted Data** occurs when an application deserializes data from an untrusted source without proper validation. This can lead to arbitrary code execution, data corruption, or other security vulnerabilities. Deserialization is often used to reconstruct objects from serialized data, but if the data is tampered with, it can lead to serious security issues.

### Prevention

To prevent deserialization vulnerabilities, consider the following best practices:

- **Populate New Objects**: When deserializing data, create new objects rather than modifying existing ones.
    
- **Integrity Checks**: Use digital signatures (e.g., HMAC) to ensure the integrity of serialized data before deserialization.
    

### Detection

Deserialization vulnerabilities can be detected using:

- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    

### Example
```java
try {
    File file = new File("object.obj");
    ObjectInputStream in = new ObjectInputStream(new FileInputStream(file));
    javax.swing.JButton button = (javax.swing.JButton) in.readObject();
    in.close();
}
```

In this example, the code deserializes an object from a file without verifying its source or contents, potentially allowing an attacker to execute arbitrary code.

---

## Type Confusion

**Type Confusion** occurs when a program accesses a resource using a type that is incompatible with its original type. This can lead to logical errors, memory corruption, or arbitrary code execution. Type confusion often arises from improper type casting or implicit type conversions.

### Prevention

To prevent type confusion, consider the following best practices:

- **Explicit Type Checking**: Always check the type of a resource before accessing it.
    
- **Avoid Implicit Conversions**: Be cautious with implicit type conversions, especially in dynamically-typed languages.
    

### Detection

Type confusion can be detected using:

- **Static Type Analysis**: Automated tools that analyze source code for potential type-related vulnerabilities.
    

### Example
```php
$value = $_GET['value'];
$sum = $value + 5;
echo "value parameter is '$value'<p>";
echo "SUM is $sum";
```

In this example, the code does not validate the type of the `value` input, allowing an attacker to supply an array, which results in a fatal error.

---

## Path Traversal

**Path Traversal** occurs when an application uses external input to construct a pathname without proper validation, allowing an attacker to access files or directories outside the intended directory. This can lead to unauthorized access to sensitive files or system resources.

### Prevention

To prevent path traversal vulnerabilities, consider the following best practices:

- **Input Validation**: Assume all input is malicious and validate it rigorously.
    
- **Secure File Storage**: Store sensitive files outside the web document root.
    
- **Mapping Inputs**: Use a mapping system to translate fixed input values (e.g., numeric IDs) to actual filenames or URLs, rejecting all other inputs.

### Detection

Path traversal vulnerabilities can be detected using:

- **Static Analysis**: Automated tools that analyze source code for potential vulnerabilities.
    
- **Dynamic Analysis**: Techniques like vulnerability scanning.
    
### Example
```java
String path = getInputPath();
if (path.startsWith("/safe_dir/")) {
    File f = new File(path);
    f.delete();
}
```
In this example, the code attempts to validate the input path but fails to account for path traversal sequences like `../`, allowing an attacker to delete files outside the intended directory.

---

## Missing Authentication for Critical Function

**Description:**  
Missing authentication for critical functions occurs when an application fails to enforce proper user identity verification for operations that require a trusted identity or consume significant resources. Without authentication, an attacker may gain unauthorized access or elevate privileges by exploiting these exposed functionalities, essentially operating at the same privilege level as the function itself.

**Prevention:**

- **Server-side Enforcement:** Ensure that all security checks performed on the client side are also duplicated and enforced on the server side.
- **Standardized Solutions:** Instead of building custom authentication routines, use established authentication libraries, frameworks, or operating system capabilities. These solutions have been rigorously tested and offer built-in mechanisms to securely manage user identities.

**Detection:**

- **Static Analysis:** Use both automatic and manual static analysis to detect functions lacking proper authentication.
- **Dynamic Analysis:** Employ vulnerability scanners and dynamic analysis tools to simulate attack scenarios that exploit missing authentication.

---

## Server-Side Request Forgery (SSRF)

**Description:**  
Server-Side Request Forgery (SSRF) vulnerabilities occur when a web application fetches a remote resource based on user-supplied input without sufficient validation. In such cases, an attacker can manipulate the URL parameter to make the server send crafted requests to unintended destinations. This exploitation may bypass traditional network security controls like firewalls or VPNs, thereby accessing internal systems or sensitive data.

**Prevention:**

- **Network Segmentation:** Isolate the remote resource access functionality into separate network segments and configure firewall policies to block non-essential traffic.
- **Input Validation:** At the application layer, rigorously sanitize and validate all user-supplied URLs and input data to ensure they adhere to expected formats and domains.

**Example Attack Scenarios:**

1. **Port Scanning:** An attacker could map internal networks by observing connection times or errors when sending SSRF payloads, thereby determining which ports are open or closed on internal servers.
2. **Sensitive Data Exposure:** Attackers may craft requests to access local files (e.g., `file:///etc/passwd`) or internal services (e.g., `http://localhost:28017/`) to retrieve sensitive information.
3. **Cloud Metadata Exposure:** Many cloud service providers expose metadata at endpoints like `http://169.254.169.254/`. An attacker can exploit SSRF to retrieve this metadata, which may contain sensitive configuration and access information.

---

## SQL Injection

**Description:**  
SQL Injection is a critical vulnerability that enables an attacker to insert (or “inject”) unauthorized SQL code into a query executed by an application. This manipulation can compromise data integrity, leak sensitive information, or even result in complete control over the underlying database.

**Example of Vulnerable SQL Query:**

```sql
SELECT * FROM users WHERE user_id = <user_id> AND password = <user_password>
```

**Correct Use Example:**  
If `user_id` is set to "Alice" and `password` to "B102", the query becomes:

```sql
SELECT * FROM users WHERE user_id = 'Alice' AND password = 'B102'
```

**Unexpected Input Example:**  
If an attacker supplies `user_id = admin'` and leaves `password` empty, the query might be transformed into:

```sql
SELECT * FROM users WHERE user_id = 'admin'--' AND password = ''
```

Here, the single quote terminates the legitimate string, and the double dash (`--`) converts the rest of the statement into a comment, bypassing authentication.

### Types of SQL Injection

- **In-band SQL Injection:**
    
    - **Classic SQL Injection:** Direct injection into query parameters.
    - **Error-based SQL Injection:** Relies on provoking database errors to reveal details about the structure, version, and operating system, and may return full query results.
    - **Union-based SQL Injection:** Combines the results of a legitimate query with the injected query, often visible in the HTTP response.
- **Inferential SQL Injection (Blind SQL Injection):**
    
    - **Boolean-based Blind:** Involves sending SQL queries that return a true or false response to deduce information character by character.
    - **Time-based Blind:** Relies on delaying responses (using time delays) to infer whether certain conditions are true.
    - **Out-of-band:** Uses alternative communication channels (like HTTP requests or DNS lookups) to retrieve query results when the main channel is not informative.

### SQL Injection Mitigation

- **Input Escaping:** Although escaping all user input offers partial protection, it must be combined with other methods for robust security.
- **Parameterized Queries:** Use prepared statements with parameterized queries to separate SQL code from data.
- **Allow-List Validation:** Validate inputs against a strict allow-list rather than relying solely on blacklisting harmful input.
- **Stored Procedures:** Use properly constructed stored procedures that encapsulate SQL logic and prevent injection.

**Tools to Mitigate SQL Injection:**  
SQLmap, jSQL Injection, OWASP ZAP (which includes FuzzDB), and other vulnerability scanning tools.

---

## Cross-Site Scripting (XSS)

**Description:**  
Cross-Site Scripting (XSS) is a type of injection attack wherein malicious scripts are injected into trusted websites. When a web application fails to properly validate or sanitize user input, it may inadvertently include harmful scripts in its output, which are then executed by the user’s browser. This can lead to unauthorized access to sensitive data such as cookies, session tokens, and other private information.

**How It Works:**  
An XSS attack occurs when the application processes user-supplied content without proper sanitization, sending it back to the browser where it is executed. The browser, not aware of the malicious nature of the script, treats it as legitimate code.

### Types and Techniques

4. **Reflected (Non-Persistent) XSS:**  
    The malicious script is immediately returned in the server’s response (e.g., via search results or error messages).  
    _Example:_ Injecting `<script>alert('XSS')</script>` into a URL parameter that is then rendered on the page.
    
5. **Stored (Persistent) XSS:**  
    The script is stored on the server (e.g., in a database or comment field) and served to users on subsequent requests, affecting every viewer.  
    _Example:_ A malicious forum post containing embedded JavaScript.
    
6. **DOM-Based XSS:**  
    The attack is executed entirely on the client side by modifying the Document Object Model (DOM). It does not involve server-side changes, yet it manipulates client-side scripts to execute malicious behavior.
    

### Example Payload

```html
<script>alert("XSS");</script>
```

When this payload is injected into a vulnerable context, such as an unsanitized form field or URL parameter, the script executes immediately.

### XSS Mitigation Strategy

A layered defense is required to mitigate XSS:

- **User Awareness & Training:** Educate developers about secure coding practices and the risks associated with user input.
- **Input Trust:** Never trust user input; always validate and sanitize data before processing.
- **Escaping and Encoding:** Utilize established libraries to escape or encode user input properly.
- **HTML Sanitization:** When HTML is required, use trusted libraries to clean and parse the content.
- **HttpOnly Cookies:** Set the HttpOnly flag on cookies to restrict access via client-side scripts.
- **Content Security Policy (CSP):** Implement CSP headers to restrict the sources from which scripts can be loaded.
- **Regular Scanning:** Use automated tools to routinely scan and identify XSS vulnerabilities.

**Typical Affected Domains:**  
Any web application that accepts and displays user input can be vulnerable to XSS. This includes forums, e-commerce sites, social platforms, and applications written in languages such as PHP, ASP, C#, Java, and Perl.

---

## Session Hijacking

**Description:**  
Session hijacking is the exploitation of a valid session key to gain unauthorized access to an application. Typically, this involves stealing a session cookie used to authenticate a user to a remote server. When an authorized user logs into a website, the application generates a session cookie that uniquely identifies the user. If an attacker can steal this session ID, they can impersonate the user and gain the same level of access, often without detection.

**Common Attack Vectors:**

- **Sniffing (Session-Side Jacking):** An attacker intercepts the session cookie using network sniffing tools. This method is less effective when encrypted communication (e.g., HTTPS) is in place.
- **Man-in-the-Middle (MITM) Attacks:** Through malware (e.g., Trojans) or compromised network nodes, an attacker can intercept and modify traffic between the user and the server.
- **Cross-Site Scripting (XSS):** A malicious script injected via XSS can steal the session ID from a user's browser and transmit it to the attacker.
- **Brute Force Attacks:** If session tokens are weak or poorly generated, an attacker may be able to guess valid session IDs.

**Mitigation Strategies:**

- **Secure Transmission:** Use strong encryption (HTTPS) to protect session cookies in transit.
- **Cookie Security:** Implement secure, HttpOnly, and SameSite cookie attributes to reduce the risk of theft.
- **Session Management:** Regenerate session IDs after login and frequently rotate session tokens to minimize exposure.
- **Intrusion Detection:** Monitor for anomalous session behavior that might indicate hijacking attempts.

---

## Cross-Site Request Forgery (CSRF)

**Description:**  
Cross-Site Request Forgery (CSRF) is an attack in which a malicious actor tricks an authenticated user’s web browser into executing an unwanted action on a trusted website. Because the user is already authenticated, the forged request is processed by the server as if it were legitimate, making it difficult to distinguish from a genuine user action.

**How It Occurs:**

7. **Preparation:** The attacker crafts a request (e.g., a fund transfer) targeted at a vulnerable website.
8. **Distribution:** The malicious request is embedded in a link, image, or other media and distributed to the victim via email, a blog, or other social engineering tactics.
9. **Exploitation:** When the authenticated user clicks the link or the browser automatically loads the malicious content, the request is sent to the website.
10. **Execution:** The website processes the request under the authenticated user's context, carrying out actions the user did not intend.

**Comparison with Cross-Site Scripting (XSS):**

- **Trust Relationship:**
    - **XSS:** The client trusts the server; the server sends data that the browser executes.
    - **CSRF:** The server trusts the client; the request is sent from an authenticated client without additional verification.
- **Attack Target:**
    - **XSS:** Directly impacts the client by executing malicious scripts in the browser.
    - **CSRF:** Impacts the server by making it process unintended actions from the client.

**Mitigation Strategies:**

- **Unique Request Identifiers:** Attach unpredictable, unique tokens (Anti-CSRF tokens) to sensitive action requests. These tokens validate that the request originates from an authenticated session.
- **ReCAPTCHA:** Use CAPTCHAs to confirm human interaction before processing sensitive actions.
- **Prompt Re-Authentication:** Require users to re-authenticate before performing sensitive operations.
- **Token Variants:** Implement patterns such as the double submit cookie pattern or synchronizer token pattern to further secure CSRF tokens.

---

## Server-Side Request Forgery (SSRF)

**Description:**  
Server-Side Request Forgery (SSRF) is a vulnerability that arises when an application allows an attacker to induce the server to initiate requests to internal or external resources without proper validation. In such attacks, the server is tricked into believing that the request originates from a trusted source. As a result, an attacker can leverage the inherent trust relationship between the server and other services to access or modify sensitive information, exfiltrate data, or even inject untrusted input into the application.

Key characteristics of SSRF include:

- **Abuse of Trusted Channels:** The attacker manipulates requests so that the target server acts as an intermediary between the attacker and internal or external resources.
- **Exploitation of URL Parameters:** SSRF commonly occurs when a user-supplied URL is used by the backend service to fetch or interact with data without proper sanitization or validation.
- **Impact on Internal Services:** Since the request appears to originate from a legitimate source, it can be used to access restricted internal endpoints or services that are not directly exposed to the attacker.

**Example Scenario:**  
Consider a server-side PHP script that retrieves and outputs an image from a given URL:

```php
<?php
if (isset($_GET['url_img'])) {
    $url_img = $_GET['url_img'];
    $image = fopen($url_img, 'rb');
    header("Content-Type: image/png");
    fpassthru($image);
}
?>
```

In this example, the parameter `url_img` is completely under the control of the user. An attacker could supply a URL such as:

```
GET /?url_img=http://localhost/server-status HTTP/1.1
```

If the web server (e.g., Apache with mod_status enabled) is configured to display detailed internal information, the attacker would receive sensitive details about the server's configuration, versions, and modules.

**Mitigation Strategies:**

- **Input Validation:** Rigorously validate and sanitize user-supplied URLs before processing.
- **Access Controls:** Implement authentication and authorization even for internal services that may not have these controls by default.
- **Whitelisting:** Restrict the range of IP addresses or DNS names that the server is allowed to contact by maintaining a strict whitelist.
- **HTTP Method Differentiation:** Differentiate among GET, POST, and other HTTP methods based on the service requirements, thus complicating the attacker’s efforts to craft a valid request.

---

## Static analysis and dynamic analysis
Dynamic and static analysis are complementary approaches for understanding the behavior of a program.

Static analysis examines a program's code without executing it, using the program itself as a guide to infer its behavior. It is **input-insensitive**, meaning it does not rely on specific inputs to analyze the program.
On the other hand, dynamic analysis observes the program's behavior during execution, using a combination of inputs and observed behavior as a guide to better understand the program's logic. This makes dynamic analysis inherently **input-sensitive**.

We can summarize this as:
- Program + Input = Behavior
- Static: Program as a guide to Behavior → input insensitive
- Dynamic: Input + Behavior as a guide to the Program → input sensitive

In terms of **completeness**, static analysis is considered complete because it attempts to explore all possible paths within the program. However, this can lead to **imprecision** due to abstractions and infeasible paths. It examines a small subset of states for all possible execution paths, which may result in over-approximations. In contrast, dynamic analysis is incomplete because it only examines the concrete execution paths taken for a given test input. While this makes it precise for the specific test set, it leaves many feasible paths unexplored.

Static analysis requires **abstraction/approximation** to ensure termination. It bounds the number and size of states to prevent infinite exploration. Static analysis tools approximate (sound approximation) the program’s reachable states with something that is easier to compute. In contrast, termination in dynamic analysis is a property of the running system rather than the analysis itself. Abstraction in dynamic analysis is used primarily to reduce runtime overhead, focusing on short paths rather than long traces.

Another key difference lies in their handling of paths. Static analysis often explores **infeasible paths**, leading to conservative conclusions—properties might be reported as not holding when they actually do. This makes static analysis **safe but imprecise**. Dynamic analysis, however, explores only feasible paths and may conclude that a property holds when it actually doesn’t, making it **precise for the test set but unsafe** overall.

The **imprecision** in each approach can be quantified. For dynamic analysis:

$$\text{Imprecision} = \frac{\text{Feasible paths} - \text{Executed paths}}{\text{Feasible paths}}$$

Precision improves as the number of executed paths approaches the set of all feasible paths, which requires systematic test generation. For static analysis, the imprecision is given by:

$$\text{Imprecision} = \frac{\text{Infeasible paths}}{\text{Infeasible paths} + \text{Feasible paths}}$$

Here, precision increases as the number of infeasible paths approaches zero, which requires effective techniques to identify and eliminate such paths.

To summarize:

| Feature                  | Static Analysis                                          | Dynamic Analysis                                         |
| ------------------------ | -------------------------------------------------------- | -------------------------------------------------------- |
| **Execution Scope**      | Exhaustively explores all executions                     | Checks some software executions                          |
| **Error Reporting**      | Reports errors as traces                                 | Can detect one or more bugs per execution                | 
| **Error Absence**        | Can establish the absence of whole classes of errors     | Cannot prove the absence of defects, only their presence |
| **Result Accuracy**      | May produce incorrect results (false positives)          | Can miss bugs (false negatives)                          |
| **Scalability**          | Scaling to whole systems increases imprecision           | Can be performed at scale on entire systems              |
| **Precision vs. Safety** | Pessimistically inaccurate – may reject correct programs | Optimistically inaccurate – may accept faulty programs   |
| **Detection Limitation** | May flag infeasible paths as errors                      | Can only confirm errors in tested paths                  |

---

## Fuzzing

Fuzzing is an automated testing technique that involves providing random, unexpected, or malformed inputs to a program in order to induce crashes, memory leaks, or other vulnerabilities. Its primary goal is to identify bugs and security vulnerabilities by overwhelming the software with a wide range of inputs.

**Characteristics:**

- **Automated Input Generation:** Fuzzers generate long and varied inputs that can stress the application’s input-handling routines.
- **Simplified Test Oracle:** Rather than verifying all aspects of program functionality, fuzzers mainly check whether the program crashes or behaves unexpectedly under test conditions.
- **Pattern-Based Testing:** Inputs can be generated randomly but often follow specific patterns, such as extremely long strings, boundary values, or unexpected integer values (e.g., minimum/maximum, zero, negatives).

**Types of Fuzzing:**

- **Dump Fuzzing:**
    - Generates inputs without considering the program’s context or state.
    - _Advanced Dump Fuzzing:_ Incorporates basic templates to drive input generation and may analyze outputs to detect issues beyond simple crashes.
- **Smart Fuzzing:**
    - Uses domain-specific knowledge and intelligence about the input format.
    - Focuses on enhancing code coverage and may utilize mutation-based or guided input generation techniques.

**Existing Tools:**  
Some notable fuzzing tools include American Fuzzy Lop (AFL), LibFuzzer, and the ZAP fuzzer, among others.

**Typical Domains of Application:**  
Fuzzing is applicable to a wide range of software domains including desktop applications, web applications, network protocols, and file format parsers.

**Limitations:**

- Fuzzers are particularly adept at finding simple bugs and crashes, but may not always detect complex vulnerabilities.
- More protocol-aware fuzzers may miss some unexpected errors due to their structured approach, whereas random fuzzing can yield unpredictable results.
- In black-box scenarios, evaluating the full impact of a discovered vulnerability can be challenging.

---

## Black-Box Testing
Black-box testing is a software testing methodology where the internal implementation of the application is unknown to the tester. Instead, test cases are derived solely from the external specifications, requirements, or design documentation. The focus is on verifying that the software behaves correctly for given inputs and produces the expected outputs.

**Key Points:**

- **Input/Output Behavior:** Testing is based on the observable functionality; if the output matches the expected result for a given input, the test passes.
- **Limited Test Case Generation:** Due to the often vast or infinite input domain, testers use techniques such as boundary value analysis and equivalence partitioning to reduce the number of test cases.
- **Testing Techniques:**
    - _Random Testing:_ Inputs are randomly sampled from the valid input domain.
    - _Grammar-Based Fuzzing:_ Inputs are generated based on a model of the expected format.
    - _Mutation-Based Fuzzing:_ Existing valid inputs are slightly altered to produce new test cases.
    - _Equivalence Partitioning:_ Input domains are divided into groups that are expected to exhibit similar behavior.

**Advantages and Limitations:**

- **Advantages:**
    - Fast and easy to perform without needing detailed knowledge of the code.
    - Lightweight and scalable for testing large systems.
- **Limitations:**
    - May result in poor code coverage since internal structures and logic are not considered.
    - It is often impossible to test all possible inputs, meaning some vulnerabilities may remain undiscovered.

---

## White-Box Testing
White-box testing, also known as structural or clear-box testing, involves an in-depth examination of the internal structure and workings of the software. Test cases are derived based on knowledge of the code, which allows testers to design inputs that thoroughly exercise specific code paths and logic.

**Key Points:**

- **Internal Knowledge:** Testing is informed by the actual implementation, which helps identify edge cases and subtle bugs.
- **Code Analysis:** Techniques such as code coverage analysis, control flow analysis, and data flow analysis are used to generate effective test cases.
- **Dynamic Symbolic Execution:** An advanced white-box technique where the program is executed with concrete inputs while simultaneously building a symbolic representation of the execution path. This symbolic representation, or _path condition_, specifies constraints that must be met to follow that path. Test inputs can then be derived by solving these constraints, ensuring that different execution paths are explored.

**Advantages and Limitations:**
- **Advantages:**
    - Provides higher coverage by testing all possible execution paths that are known from the code structure.
    - Capable of detecting errors that may not be apparent from an external perspective.
- **Limitations:**
    - Requires detailed knowledge of the code, making it resource-intensive and more complex to perform.
    - The process of dynamic symbolic execution and constraint solving can be computationally expensive.