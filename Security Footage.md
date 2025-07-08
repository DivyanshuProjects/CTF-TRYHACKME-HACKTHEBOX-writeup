---

A Tale of Packets and Pixels: Recovering Security Footage from Thin Air ( TryHackMe )
"They destroyed the hard drives. But they forgot about the wire."

---

💥 The Incident
Our office was broken into. Clean job. Except for one thing: they destroyed the hard drives that contained all the security footage. No DVR. No cloud. Nothing.
Except a .pcap file.
Someone had the foresight to capture the network traffic from the surveillance subnet. That was all I had to work with.
A single file:
security-footage-1648933966395.pcap
And a single shot at the truth

---

🔍 Step One: Dive Into the Packets
Like any digital forensics geek, I started with Wireshark. Loaded the .pcap, filtered for HTTP, and scrolled through the payloads.
Nothing too juicy at first glance. But then I saw it.
Content-Type: image/jpeg
JPEGs… over HTTP? That's when it clicked.
This was likely an MJPEG stream - a common format used by IP cameras where each frame is a separate JPEG image. The camera doesn't send a video - it sends hundreds of JPEGs in rapid sequence.
So instead of looking for a video file, I needed to reconstruct the stream, image by image.

---

🧪 The (Almost) Wrong Turn
At first, I thought:
"Why not extract the HTTP payloads manually, dump the binary, and convert them into images?"
I almost started using xxd or hexdump to grab the binary payloads, one by one, and reconstruct the JPEGs manually.
That would've taken forever.
"Thank god I didn't go down that rabbit hole. I'd still be there by the time this post went live."

---

🧰 Enter: foremost
Then I remembered a tool from the digital forensics underworld: foremost.
Originally designed for law enforcement to carve deleted files from disk images, foremost works just as well on PCAPs.
🔧 What is foremost?
A file carving tool
Searches for known file headers/footers in raw binary data
Extracts files like JPEG, PNG, ZIP, DOC, and more

✅ How I Used It
foremost -i security-footage-1648933966395.pcap -o extracted/
This told foremost:
-i: Input file is the .pcap
-o: Output the recovered files into a folder

After a short wait, it carved out a treasure trove of images from the stream:
extracted/jpeg/

---

🎞️ Reconstructing the Footage
Hundreds of frames. No context. No timestamps. Just raw, silent images.
But here's a trick:
⏩ Bash Image Flipbook
for img in *.jpg; do feh "$img"; sleep 0.1; done
This flipped through each frame with a short delay. Suddenly, it felt like I was watching the footage in real-time.
But something felt off…

---

🔡 The Flag Was Fragmented
There wasn't a single frame containing the full flag. Instead, each image had a tiny fragment - a single character, sometimes two - subtly embedded.
It wasn't just a visual sequence; it was a puzzle, with each JPEG carrying part of the final message.
Even more clever? Every image had a serial code - a hint to the correct order.
It became clear: the flag had to be reassembled manually, piece by piece.
So I wrote a script to extract and sort the characters based on their serial tags - and finally, the full flag appeared:
flag{5ebf457ea66*********************}
Pixel by pixel. Image by image. The truth came back.

---

🔬 Tool Breakdown
Here's a quick rundown of everything I used:
🧠 Wireshark
Opened the .pcap
Inspected packet details
Verified presence of image/jpeg payloads

🔧 foremost
Carved JPEG images out of raw packet data
Saved them to a structured output directory

🖼️ feh
Lightweight image viewer
Used to animate JPGs like a flipbook

🐚 bash
Wrote a simple for loop to display images
Scripted frame processing and character sorting

🧵 Custom script (Python)
Parsed serial codes
Reconstructed the full flag from individual pieces

---

🎯 Final Thoughts
This wasn't just about extracting images. It was about understanding how devices communicate, how cameras stream, and how even when evidence is "destroyed," it's never really gone.
Next time someone tries to wipe the truth…
Check the packets. They always remember.

---

If you enjoyed this post, follow me for more digital forensics deep dives, CTF writeups, and packet-powered adventures.
