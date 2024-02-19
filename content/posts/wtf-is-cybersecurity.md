+++
title = 'Wtf Is Cybersecurity'
date = 2022-07-22T00:00:00-00:00
draft = false
+++


I have been involved in software engineering and other fields adjacent to cybersecurity for over a decade, and I am currentlly a Staff Security Engineer, with a focus on cloud and infrastructure security. A question that I am often asked is, "so, what do you actually do?"[^1] Strap in, y'all[^2], and we'll get there.

Shows like Mr. Robot and frequent news reports of data breaches have recently brought a lot of attention to the world of offensive and defensive security. Everybody and their mother wants to put on a Cult of the Dead Cow[^3] hoodie[^4] in a dark room and crush Red Bull while hax0ring the mainframe all night to save the planet from alien lasers. I'm not excluding myself from that crowd. 

My goal is to give you a very broad overview of why we care about it, what cybersecurity actually is, and how to jump in. While this guide is aimed at folks who are new-ish, my hope is to form a digestible set of pillars and principals that I can refer back to in future writings.

# Why TF?

So why do we even care about this stuff? Aren't computers and data more secure now than they ever have before? The answer is a very loud ***no***. While it's true that we have come a long way from our beginnings, technology has become a lot more complex, and the problems that engineers are solving are becoming increasingly complicated due to more layers of abstraction and a deficit of security skills training and other resource constraints. You can safely assume that [everything](https://theconversation.com/what-is-log4j-a-cybersecurity-expert-explains-the-latest-internet-vulnerability-how-bad-it-is-and-whats-at-stake-173896) is vulnerable in some way, and I mean [everything](https://www.cnet.com/news/privacy/vulnerable-vibrator-security-researchers-find-flaws-in-connected-toy/). The internet is the still the wild west, and it's only getting wilder and wester.

I am (hopefully) preaching to the choir, but the main reason why we care about security is that people NEED safety; it's one of the most foundational aspects of Mazlow's hierarchy of needs. We NEED the cars that we drive to not explode or fail to break, we NEED airplanes to safely take off and land, we NEED our food supply to not be spoiled, and we NEED medical equipment to properly work during critical operations. Cybersecurity both protects these systems from attack and makes sure these systems do not fail in unexpected ways and cause harm to the people who depend on them.

# So WTF is it?

## People and Data

Cybersecurity, information security (InfoSec), security engineering, computer security, internet security; what do all these things mean? While they all have their own nuances, all of them are talking about the same thing for the purposes of this article: ***protecting people and data when computers are involved***. We will use that as our working definition for what cybersecurity is while we look at the mechanics.

## Risky business

When most people think of information security, their mind jumps to one of two scenes:
* a) a cacophony of defenders running frantically through a control room and haphazardly pushing buttons to prevent 1337 hax0rs from breaching the third layer of the firewall or
* b) a board room full of unsmiling suits who are monotonously reciting monstrous pages of bone-dry legalese at a glacial pace

I am happy to inform you that neither of these are correct; the reality is somewhere in between. When we talk about information security, we are specifically talking about ***managing risk***. I know what you're thinking: "but n0rtr0n, what about the hax0rs?" Defending against malicious actors is definitely a part of it, and so is writing policies and procedures. Ultimately, these things are both important and are both effective strategies in dealing with risks associated with having data and people to protect. As a security engineer, I build and use technical solutions to ***manage risk*** for my company by mitigating threats that could impact the ***lives of people*** or the ***confidentiality***, ***integrity***, and ***availability*** of systems and data. 

## The Triforce[^5]

Most security professionals will agree that there are three pillars that every inform every security principle[^6] : *confidentiality*, *integrity*, and *availability*. These are some times referred to as the CIA triad. If you take away nothing else, know that these three things are the absolute foundation of everything we do in InfoSec. 

## Confidentiality
*Confidentiality ensures that no unauthorized access to data occurs.*

Confidentiality is effectively privacy. It makes sure that, among other things, your medical history and text messages and personal photos are not publicly searchable on the internet. This pillar is about consent; those who should have access to the data do, and those who should not do not. Encryption is one of the most widely used tools to making sure private data stays private. Using encryption, only people and systems that have access to the encryption keys can decrypt, read, and access the encrypted data.

### Integrity
*Integrity ensures that data is protected from unauthorized changes.*

Another way to frame this pillar is a question: can I trust that this data is correct and accurate? We want to make sure that data was not modified by people and systems who did not have authority to do so, or in a way that was unauthorized. For example, if Steve(15) mails you a letter, integrity is the assurance that I can't intercept that letter and replace your name with "LoSeR" before the letter gets to you. One of the most common ways integrity is achieved is by a technique known as hashing. If any of the contents of data changes, the hash also changes. If the hash provided with the data is different when you hash the data yourself, you know that the data has been tampered with and cannot be trusted.

### Availability
*Availability ensures that data is accessible by people and systems who are authorized when it is needed.*

Availability is the accessibility of data by the people that are authorized to do so. A big part of cybersecurity is making sure that data is protected against destruction or inaccessibility. One common attack against availability is ransomware, in which an malicious actor will get unauthorized access to a target's data and encrypt it, which often results in data loss unless they are prepared to handle it. Another common attack is called denial of service, in which an attacker will try to make a resource inaccessible to authorized users. Security professionals will often implement backup and disaster recovery plans to mitigate this threat.

## How TF?
You can go down so many rabbit holes (and I encourage you to do so) about the different aspects of security: physical, digital, attack, defense, operations, engineering, application, cloud, detection, forensics, response. Whatever you do, do not get too caught up in the details; comprehension will come to you over time with practice and diligence. We could (and we will) talk about the nuts and bolts of encryption, software and infrastructure design, malware detection and analysis, email, DNS, TCP, IP, and a million other specific technologies and controls. *All of these are a means to end*; we implement these measures, spin up these tools, and set up these defenses *because* they help us to embody our principles and values in support of our pillars. Though this is by no means a comprehensive list, here are some of the most common principles that are shared by the InfoSec community:

### Human Lives and Livelihood Above All Else
I don't mean to virtue signal, but humans are literally the most important part of living a human life on this planet. We *must* prioritize the safety of people above anything else. While we also want to protect data, the data is valuable because there is human value that is attached to it. 

### Least Privilege
The principle of least privilege is that users should have access to only what they need, and no more. Typically[^7] when you invite guests into your home, you allow them to be in your space while also not accessing the contents of your underwear drawer[^8]. Enough said.

### Defense in Depth
Defense in depth is simply multiple lines of defense. Going back to the underwear drawer example: if you invite a guest into your home, and you want to enforce them not having access to that drawer, you could install a lock on the drawer in supplement to trusting that they are not an amoral creep as a measure of defense in depth.

### Security By Design
"Shift Left"[^9] is another way to say this; security by design is getting security involved up front instead of after a project. It is always much, much more expensive in time and money to fix flaws in designs, code, and process *after* the project is finished instead of before. Security by design gets ahead of costly design flaws by prioritizing resources ahead of a product release.

# Sick!  How do I get started?
The most important thing to know about getting started, is that there is not one single path you could or should follow. Every security professional I have ever met has had a unique background that led them to where they are now. For example, I started out my career as a software developer who dropped out of college[^10]. The common thread is that most folks are self-taught to some degree, and all of them are ravenously passionate about learning more every single day and breaking a lot of things in the process. The best thing you can possibly do is to find these people and get involved in the communities around them. All that being said, here are a few resources you might find helpful:

## Books
Books slap. While they do require an investment of time, there is a lot of value in a cohesive collection of knowledge about a topic, especially one that can be as dense as cybersecurity.  

* Foundations of Information Security by Jason Andress: https://www.amazon.com/Foundations-Information-Security-Straightforward-Introduction/dp/1718500041
* CyberSecurity Career Guide by Alyssa Miller: https://www.amazon.com/Cyber-Security-Career-Alyssa-Miller/dp/1617298204
* Sandworm by Andy Greenberg: https://www.amazon.com/Sandworm-Cyberwar-Kremlins-Dangerous-Hackers/dp/0525564632

## CTFs
CTF (Capture the Flag) programs are a great way to get hands-on experience with a lot of different tools and technologies. They put you in the hot seat of attackers and defenders by providing simulations of some real scenarios that you can use to sharpen your skills. The courses are guided, and they are welcoming to all skill levels.

* TryHackMe - https://tryhackme.com/
* HackTheBox - https://www.hackthebox.com/
* PicoCTF - https://www.picoctf.org/

## YouTube
If you are a visual learner, it is really difficult to beat YouTube. There are so many good YouTube channels that demonstrate hacks and defense and give you optics into so many topics relevant to security, computers, and the internet. The content recommendations can also help you easily get more information about something specific. 

* John Hammond:  https://www.youtube.com/c/JohnHammond010  
* Marcus Hutchins: https://www.youtube.com/channel/UCLDnEn-TxejaDB8qm2AUhHQ
* David Bombal: https://www.youtube.com/c/DavidBombal


# Wrap It Up!
There are so many places to go from here. I will be exploring a ton of different aspects of security, privacy, and engineering in the coming posts. Good luck on your cybersecurity journey, and let me know what topics you would like me to cover next!


[^1]:  I sometimes wonder the same thing myself
[^2]: You can take me out of Texas but you can't take the Texas out of me
[^3]: Stop what you're doing (after you finish this article) and go read [Cult of the Dead Cow by Joseph Menn](https://www.amazon.com/Cult-Dead-Cow-Original-Supergroup-ebook/dp/B07J54F9KR) for an epic chronicle of How We Got Here™
[^4]: These hoodies *are* pretty sick
[5^]: Yeah
[^6]:  Other models exist, like the [Parkerian Hexad](https://en.wikipedia.org/wiki/Parkerian_Hexad)  we don't want to get too deep into these. Focus on the basics!
[^7]: https://youtu.be/FArZxLj6DLk?t=56 
[^8]: I am making a lot of assumptions here
[^9]: Once again, I am making a lot of assumptions here
[^10]: Queue eye rolls
[^11]:  No regrets