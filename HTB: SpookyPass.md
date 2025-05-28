🧛‍♂️ HTB: SpookyPass – A Quick Reversing Challenge
🎃 Introduction
This was a fun little reversing challenge from Hack The Box named SpookyPass. The goal? Analyze a binary, extract the correct password, and get the flag. It might sound spooky, but it was all treat, no trick! 🕷️

In this write-up, I’ll walk through the simple steps I used to reverse the file and uncover the flag using basic Linux command-line tools.

📦 Step 1: Analyzing the Given File

The first thing I did was determine what kind of file I was dealing with. For that, I used the filetype command:

filetype -f pass
Output:

pass: elf (application/x-executable)
Great — it's an ELF binary, meaning we can analyze it like any other Linux executable.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

🔒 Step 2: Checking Binary Protections
Before diving deeper, it’s important to understand what mitigations or protections the binary has. I used the checksec utility:


checksec --file=./pass
From the output, we can see:

Partial RELRO

Stack Canary found

NX enabled

PIE enabled

These are fairly standard security features, but nothing that would prevent simple static analysis or string inspection.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


🔍 Step 3: Looking at Strings
Now, time to dig into the binary to see if it contains any hardcoded information. I used the strings command:


strings ./pass | less
And guess what? Among the usual noise, a password was printed right in the output:



s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5

It also mentioned something like:


Before we let you in, you'll need to give us the password
Looks like we’re on the right track.

![Screenshot 2025-05-28 094552](https://github.com/user-attachments/assets/871b844a-704a-43cc-aa4a-1a212431b9e8)


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


🧪 Step 4: Executing the Binary
I ran the binary to see how it behaves:


./pass
It displayed the following:


Welcome to the SPOOKIEST party of the year.
Before we let you in, you'll need to give us the password:
I entered the password I found earlier:


s3cr3t_p455_f0r_gh05t5_4nd_gh0ul5
And voilà:


Welcome inside!
HTB{un0bfu5c4t3d_5tr1ng5}
There it was — the flag!

🏁 Conclusion
This was a nice beginner-friendly reversing challenge that required no disassembler or decompiler — just classic command-line tools: filetype, checksec, strings, and bash.


![Screenshot 2025-05-28 094702](https://github.com/user-attachments/assets/52da65a7-eba6-478a-9fdc-067ab2bff045)

🔐 Key Takeaways:
Always start by identifying the file type and checking security mitigations.

strings can sometimes reveal secrets without even running the binary.

Static analysis is often faster than dynamic — try it first!

💬 Hope this walkthrough helps others trying to get comfortable with basic reversing challenges. If you enjoyed this, follow me for more write-ups!
