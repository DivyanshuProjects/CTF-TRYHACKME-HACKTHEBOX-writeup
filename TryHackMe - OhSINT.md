 TryHackMe - OhSINT (OSINT CTF Write-up)  

 Challenge Overview  
- Challenge Name: OhSINT  
- Platform: TryHackMe  
- Category: OSINT (Open-Source Intelligence)  
- Difficulty: Easy  
- Objective: Extract useful information from a single image using OSINT techniques.  

---

 Approach & Solution  

 Step 1: Analyzing the Image File  
The challenge provided an image file as the only clue. The first step was to analyze its metadata using `exiftool`:  

```bash
exiftool image.jpg
```  
This revealed useful EXIF metadata, including a username OWoodflint and location details.  

---

 Step 2: Investigating the Username  
Using `OWoodflint` as a starting point, I searched across various platforms, leading to:  
- GitHub Profile – Contained an email address: `OWoodflint@gmail.com`.  
- Social Media Accounts – Traced online presence and shared posts.  

---

 Step 3: Extracting WiFi Information Using WiGLE  
The EXIF metadata also contained a WiFi SSID: UnileverWiFi. To identify the approximate location, I used [WiGLE](https://wigle.net/), a database that maps SSIDs to geographic locations.  

Steps to search Wi-Fi SSID on WiGLE:  
1. Go to [https://wigle.net/](https://wigle.net/)  
2. Navigate to the Search tab.  
3. Enter `UnileverWiFi` in the SSID field and click Search.  
4. If the SSID exists in WiGLE’s database, it will provide an approximate location.  

This method helps track Wi-Fi networks that have been logged in the past, revealing potential locations of the target.  

---

 Step 4: Discovering Travel Details  
A deeper dive into social media posts showed references to a trip to New York.  

---

 Step 5: Cracking the Password  
A previously used password, `pennYDr0pper.!`, was found associated with the email. This underscores the risks of password reuse.  

---

 Findings Summary  
| Data Type  | Extracted Information |  
|---------------|--------------------------|  
| Username  | OWoodflint |  
| City  | London |  
| WiFi SSID | UnileverWiFi |  
| WiFi Source | Tracked using WiGLE |  
| Email | OWoodflint@gmail.com |  
| Email Source | GitHub |  
| Holiday Destination | New York |  
| Password Found | pennYDr0pper.! |  

---

 Lessons Learned  
- Metadata can reveal a lot! Be mindful of the data stored in images before sharing them publicly.  
- WiFi SSID tracking via WiGLE can expose approximate locations.  
- Usernames are unique identifiers and can be traced across multiple platforms.  
- Password hygiene is critical—reusing passwords makes accounts vulnerable to attacks.  
- OSINT is powerful, and attackers can use minimal information to build a complete profile.  

---

 Conclusion  
This challenge was a great introduction to OSINT, demonstrating how small data points can be pieced together for a bigger picture. Cybersecurity awareness is essential to protect personal and organizational data.  

🔗 TryHackMe OhSINT Room: [https://tryhackme.com/room/ohsint](https://tryhackme.com/room/ohsint)  

🚀 Happy Hacking!  

---
