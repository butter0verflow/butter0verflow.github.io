---
title: OSCP/PWK Review
categories: [oscp]
tags: [oscp, Offensive Security]
---

Another obligatory OSCP review? Why not!

I recently took at a shot at OSCP and passed it with flying colors (got 5/5 systems in first attempt!). Other than academics and an internship, I had no prior work experience in pentesting. In this blog post, I will try to summarize my PWK/OSCP experience with the hope of helping others who are in similar situation or want to take a step in this direction. This is my first post and I will try to keep it as much concise as possible while also trying to be useful.  

### Background

While I have a background in the infosec field, it's a relatively short one. I got into the field in late 2018 with the start of my masters degree in cybersecurity. I was already familiar with basic concepts of programming, networking etc. thanks to my undergrad degree. In the summer of 2019, I worked as a security intern at a consulting firm. Before starting PWK, I had also worked on a few Hack the Box machines, which proved to be very useful for OSCP.

### Preparation

In the summer of 19, I was introduced to (Hack the Box)[https://www.hackthebox.eu/]. I decided to work on HTB, learn the basics, then start PWK in following September, go through the labs for 60 days and attempt the exam mid-November. With no prior background in pentesting, I spent around 8-10 hrs a week on Hack the Box for three months during summer. I focused more on hte retired systems, especially the "OSCP like HTB machines" from this (playlist)[https://www.youtube.com/playlist?list=PLidcsTyj9JXK-fnabFLVEvHinQ14Jy5tf].
(IppSec's)[https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA] youtube videos were immensely helpful and I literally treated this channel as my Netflix for the summer. Using his walkthroughs as reference, I went through about 30 retired HTB systems. This helped me understand the basic process and I got a good amount of practice on the methodology.


### Lab

I got Offsec's email for PWK lab access on 7th September. Alongside VPN creds, there was also a \~300 pages long PDF lab manual. I was able to skim through this PDF the very same day as I was now already familiar with most of these concepts (thanks to HTB). I also decided to skip the exercises and directly jump into the lab, since my goal was to pass the exam by getting 4/5 systems and not rely on the mercy of 5 exercise points (moreover, the exercises would have taken about a week, which I wanted to spend on hacking more boxes). On the second day, I setup Kali VM according to my preferences. I also setup CherryTree for notes+writeups and git for the backup. I then ran some basic enum scripts on the network to find all hosts, map hostnames with ip and few nmap scans. 

Since this was pretty early in the semester, I could afford to ignore classes and dedicate 12+ hours a day to the lab. By the end of first week, I was having a lot of fun and had solved around 10 boxes. I was making exponential progress, taking lesser time for every new box. By the end of 3rd week, I was done with almost the whole public subnet (\~40 boxes), including the big four (pain, sufferance, ghost and humble). With the arrival of mid-sems, now was the time to take a break from PWK and work on academic backlog.

Fast forward two weeks, I was now fairly confident and scheduled the first attempt for 24th October. I still had about 10 more days and decided to work on the things not covered in PWK. Since I was very weak at Windows Privilege Escalation, I spent a week on it with the help of lpeworkshop (I will soon be posting a writeup on this) and spent another day practicing buffer overflow. The remaining two days before exam were spent on going outdoors (it was fall!) and relaxing.

### Exam

OSCP exam is well known for its difficulty and it's not the exam systems but the 24-hours time limit which make it challenging. On the exam day, I setup OBS for screen recording and made sure of the backup VMs/network connection etc. I had scheduled the exam to be started at 11AM - received offsec's email 15 mins beforehand and was done with the setup process smoothly. By the end of first hour, I had completed buffer overflow. By the end of second hour, I was done with another easy system and was at 35 points mark, which is half of the 70 required for passing. I was pretty happy and decided to take a lunch break while running (autorecon)[https://github.com/Tib3rius/AutoRecon] on the remaining three systems in the background. 

Still high on confidence, I looked at the autorecon scan results of third machine (20 pts). I was pretty familiar with the running services and was able to pwn it within two hours. At the 5 hours mark, I had 55 points (25+10+20) just 15 shy of passing but poor me had absolutely no clue of of the massive uphill battle ahead! Now looking at the scan results of remaining two systems, I decided to work on both simultaneously (WHY?!?!). The intial enumeration was easy, but then I arrived at a dead end on both the systems. I spent 6 more hours (!) looking at the same things over and over again and going down the rabbitholes. It was now 10pm and I was still at 55 points. I had made no progress in the last 6-7 hours, was exhausted and started thinking about the next attempt.

Before giving up, I went to get food and decided to give it a last go. I deleted old enumeration notes and started over again focusing solely on the fourth system (20 pts). Unsurprisingly, the then overconfident me had missed a basic enum step and I now had found a very interesting lead. After working in that direction for another hour, I got user on the fourth system. I was at 65 points and regretting not completing the lab report (I had lab writeups but skipped exercises!) which would have given me the necessary 5 more points. Anyway, it was past midnight and I was living off the alternate coffee+chai boosts. In an hour more at around 3AM, I managed to get system access on fourth system and had enough points (75) to pass. Now relaxed and happy, I enumerated the fifth system (25 pts) from scratch, realized what I was doing wrong and got it within an hour!

Now, it was 4AM and I had 100/100 points. I was exhausted both mentally and physically yet decided to retrace my steps. I then spent about three hours recreating every exploit, taking missing screenshots (I already took screenshots after completing each box) - writing down the steps in notes and also making sure of the submitted flags in exam control panel. It was a chilly yet nice autumn morning, I had spent the last 20 hours on the exam and decided to turn off the music, vpn and laptop.

### Post Exam 

I woke up at about 10AM. Even though a short one, it was a good 3-hours nap. Since I had the whole writeup in notes, all I had to do was to copy-paste everything into a template for the exam report. For this, I used (whoisflynn's)[https://github.com/whoisflynn/OSCP-Exam-Report-Template] template. I submitted the report at 12PM and now was my turn to wait for Offsec's response. Surprisingly, the wait was very short (30 hours) and I received the passing email at 6PM on 26th Oct. This was the ultimate prize of a very challenging yet rewarding 5 month journey!

### My take on OSCP

As a newbie to pentesting, it took me about 5 months of prepartion to get to this point. Directly or indirectly, I learned a lot from this course and had a lot of fun. While the PWK labs are definitely outdated and also fall short on important topics such as Windows privesc and client-side web explotation, they are still worth every penny and time. The OSCP cert is still one of the more important things to have on your resume for an entry level pentesting job and you'd not regret it.

On the flipside, PWK labs are very outdated. Almost all the boxes \*nix boxes can be privesc'ed with a kernel exploit and many of the windows boxes directly give you system shell. There's so exposure to newer OS versions, AV and HIDS, Active Directory and Powershell etc. which is a big part of real-world pentesting. In this aspect, Hack the Box and VHL are way ahead. Honestly, I learned more from Ippsec's youtube videos and HTB than the PWK/OSCP. But as I said, this was a very challenging yet fun thing that required a newbie like me to put a lot of efforts and it was worth it. I can't speak for someone with a good amount of pentesting experience but I'd recommend it to anyone trying to break into this field!         

### Takeaways and Resources

During this course, I came across an amazing community ("InfoSec Prep")[https://discord.gg/RRgKaep] on discord. As if the course wasn't fun already, I found a bunch of people there who were also working towards OSCP in the same timeline, which made it more fun. It was really nice to have others to discuss technical issues, tools and resources with.

While most of these takeaways are listed on every other blog, I will just try to list the ones that were mostly helpful:

+ (Autorecon)[https://github.com/Tib3rius/AutoRecon]
This gem of a tool can be used to automate the initial enumeration phase. While I did not use it in labs, I decided to give it a go just four days before the exam, configured it according to my preferences and used it in the exam. This saved me a lot of time by automating all that enumeration. All I had to was to look at the huge output and figure out which low hanging fruit to go after. For something like OSCP exam which is time bound, autorecon is definitely very useful.

+ (OSCP-HTB Walkthrough Playlist by TJNull and Ippsec)[https://www.youtube.com/playlist?list=PLidcsTyj9JXK-fnabFLVEvHinQ14Jy5tf]
This channel and playlist deserve a huge amount of credits for helping me learn and pass the OSCP.

+ (lpeworkshop)[https://github.com/sagishahar/lpeworkshop]
Best rosurce available out there for Windows Privilge Escalation, I would not have passed the exam without this! (I'm planning on writing a series of posts on this, keep an eye out!)

+ (Lin.security)[https://www.vulnhub.com/entry/linsecurity-1,244/]
Amazing resource for learning common Linux Privilege Escalation techniques. They also have a well written walkthrough (here)[https://in.security/lin-security-walkthrough/]

+ (Linux Privilege Escalation Guide)[https://www.udemy.com/course/linux-privilege-escalation/]
Another amazing resource from Tib3rius, the author of Autorecon. While I did not take this, I have heard really good reviews about this for OSCP. 

+ (PortSwigger's Web Security Academy)[https://portswigger.net/web-security]
This is a very good guide on fundamentals of Web app vulns.

+ (dostackbufferoverflowgood)[https://github.com/justinsteven/dostackbufferoverflowgood]
I used this to practice bufer overflows before the exam. They have a well written writeup too!

#### Advice

+ The OSCP exam is all about enumertion. If you hit a brick wall, you probably missed something important in the enumeration. Don't spend too long working in the same direction, just reset and enumerate again. Following this could have saved me more than 6 hours in exam and a lot of frustration.  
+ Don't exploit the machines just to get flags. Learn what service it is, what it does and how it is supposed to work under normal conditions. This will help you understand what's different in your situation, ultimately making it easier for you to exploit it. 
+ Don't rely solely on the PWK labs. They are outdated and don't cover many of the necessary techniques.
+ Try to work on other resources such as VHL, HTB or above mentioned resources. They are really helpful from the course and exam's perpective.
+ Try to develope your own methodology. For example, I'm more comfortable with web services than network. So, I'd always enumerate the web ports first and then go after other services.
+ For windows, learn how to upgrade to powershell on the victim system using (Nishang)[https://github.com/samratashok/nishang]. You can watch ippsec's videos for this (Writeup coming soon!). 
	- Learning how to load and run those powershell scripts directly from memory can help you evade AV and HIDS on newer windows OS!
	- Powershell allows you to use PS Modules like (PowerUp)[https://github.com/HarmJ0y/PowerUp] and (JAWS)[https://github.com/411Hall/JAWS] which makes the privilge escalation way more easier.
+ In the exam, remember to take breaks and eat/drink.


### Post-OSCP plans

It's already been more than a month since getting the OSCP. I had to spend it catching up with the semester, and right now I'm enjoying the little winter break before diving more into web apps. My current plan is to work on (PenterLab's Web Exercises)[https://pentesterlab.com/exercises] and then start looking for web app pentesting jobs. I want to start OSWE in February, but that'd depend on how the job hunt goes.


Thank you for reading this article. As the (about)[/about/] section says, I suck at writing and this was my first ever blog post. I hope this proves to be helpful to someone. If you have any questions or would just like to discuss anything, feel free to get in touch with me!