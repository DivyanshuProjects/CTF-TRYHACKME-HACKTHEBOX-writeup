 Spookifier - HackTheBox CTF Writeup

 Introduction
I'm Divyanshu, and in this write-up, I'll walk you through my approach to cracking the "Spookifier" challenge on HackTheBox. This challenge required us to dig into the source code of a web application, uncover its vulnerabilities, and exploit a Server-Side Template Injection (SSTI) flaw in Mako to execute arbitrary commands and grab the flag.

---

 Code Review

 Analyzing `routes.py`
The web app is built using the Mako template engine and has only a single route. The `text` parameter is passed to the `spookify()` function, and its output is rendered in the template. This immediately raises suspicion—user input being rendered without sanitization is a classic setup for SSTI exploits.

To get a better understanding of how the application processes input, we examine `util.py`, where the `spookify()` function resides.

![Screenshot 2025-03-20 130742](https://github.com/user-attachments/assets/a27bf7f9-b62e-4210-85c7-c2228fdc114a)



 Analyzing `util.py`

 `generate_render()` Function
The `generate_render()` function takes the converted fonts from `spookify()` and formats them into a neat table for the user to see. While this function itself doesn’t introduce security issues, it helps us understand the app's output structure.

 `change_font()` and `spookify()` Functions
- The `change_font()` function returns a list of four modified font versions of the original text.
- The `spookify()` function calls `change_font()` on user input and passes the result to `generate_render()`, which then renders it into the template.

The biggest issue? There’s zero input validation before data is processed by Mako’s `render_template()`, leaving the door wide open for SSTI attacks.

![Screenshot 2025-03-20 130832](https://github.com/user-attachments/assets/4921cdbe-511d-450d-9ad1-1c3043b65bb2)


---

 Exploitation

 Mako Server-Side Template Injection (SSTI)
To confirm the SSTI vulnerability, we test with a simple arithmetic expression in Mako syntax:

```
${42-20}
```

If the app returns `22`, we know Mako is executing our input as code. Now, let's take this further.

 Bypassing Character Restrictions

Trying to execute system commands using Mako’s Python block syntax:

```
<% import os; test=os.popen(‘whoami’).read() %> ${test}
```

We notice that certain characters (`<` and `%`) don’t get processed properly. This happens because the font dictionaries in `util.py` don’t define replacements for these characters, causing them to be filtered out before reaching `render_template()`.

 Remote Code Execution via Template Namespace
A clever way around this restriction is leveraging Mako's template namespace, which provides access to Python modules. Using this method, we can directly invoke system commands:

```
${self.imports['os'].popen('cat /flag.txt').read()}
```

![Screenshot 2025-03-20 125338](https://github.com/user-attachments/assets/a52f8b1f-fd55-4624-af72-1c27ba524589)


Boom! This command fetches the contents of `/flag.txt`, giving us the flag.

---

 Alternative Exploitation - Using Burp Suite
Another method is using Burp Suite’s Intruder tool to automate our attack. By sending multiple payloads and analyzing responses, we can efficiently test various payloads to bypass restrictions and gain command execution.

---

 Conclusion
This challenge highlights the risks of insecure template rendering and the power of SSTI exploitation. By abusing Mako’s rendering process, we achieved remote code execution and successfully captured the flag. The key takeaway? Never trust user input—always validate and sanitize it before passing it to a template engine!

