CTF Walkthrough: Dimensional Escape Quest (Misc, Medium)

 Introduction

Hey everyone! I'm Divyanshu Kumar, a cybersecurity adventurer, always on the lookout for hidden exploits and secret backdoors. Today, I'm diving into an exciting CTF challenge—Dimensional Escape Quest—a puzzle-filled journey where magic meets cyber trickery. 

This challenge required a mix of puzzle-solving techniques, cryptographic decoding, and digital forensics, all wrapped in an enchanting, otherworldly narrative. Let’s unravel this mystery step by step!

---

 Challenge Description
You wake up in a surreal, enchanted forest—where reality twists and logic bends. The trees whisper secrets, the ground shifts beneath your feet, and mischievous creatures lurk in the shadows. Singing squirrels hum riddles, nymphs weave cryptic clues, and grumpy wizards guard knowledge that could lead you to freedom—or eternal confusion. 

Your mission? Escape this labyrinth of magic by decoding its hidden messages, uncovering forbidden commands, and unlocking the secrets of its digital underbelly. The journey is filled with unexpected surprises, but only the sharpest minds will emerge victorious. 

---

 Step 1: Inspecting the Digital Footprint
Like any good hacker-adventurer, the first step is to analyze the game's communication with the server. We fire up Developer Tools in our browser, navigate to the Network tab, and reload the page.

And there it is—an anomaly. A mysterious JSON file appears in the network logs, something that isn’t even listed in the game’s source folder. Suspicious? Definitely.

The JSON file is located at:
```
http://<ip>/api/options
```

Opening it, we find a list of all in-game commands, but one stands out from the rest:
```
Unknown Command: secret 0 "Blip-blop, in a pickle with a hiccup! Shmiggity-shmack"
```

A hidden command? Now things are getting interesting.

---

 Step 2: Unleashing the Secret Spell
Armed with this newfound knowledge, we return to the game and input the forbidden command. 

We type it in. Hit enter. And then—

🎉 FLAG CAPTURED! 🎉

The game responds with the coveted flag, rewarding our curiosity and persistence. 

---

 Conclusion
This challenge was a perfect blend of web exploitation, hidden API discovery, and creative problem-solving. By carefully inspecting the game's backend mechanics, we uncovered a secret passageway leading straight to victory.

Lessons learned?
- Always check the Network tab for hidden files.
- API endpoints can reveal unexpected secrets.
- Sometimes, the quirkiest messages hide the biggest clues.

Hope you enjoyed this adventure! Until next time—keep hacking, keep exploring, and never stop questioning reality. 🚀

