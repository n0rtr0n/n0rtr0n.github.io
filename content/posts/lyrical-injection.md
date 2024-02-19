+++
title = 'Lyrical Injection: How I Exploited Defcon 30 and John Lennon'
date = 2022-08-16T00:00:00-00:00
draft = false
+++

I just returned from my very first [Defcon](https://defcon.org/), where I gave a very unofficial talk. I did not submit for this talk, and my name was not on any list. I simply showed up and capitalized on the fact that if you go to a certain place at a certain time and hand someone a piece of paper with your name on it and a song title, they *will* give you a microphone for five minutes. I was able to pivot from there. Friday night, I wandered into Hacker Karaoke, wrote down my name and a song title (I'll get to this in a minute), and nervously took a deep breath and loudly exhaled as I exchanged my golden ticket (yes, that is a Kerberos reference) for my literal five minutes of fame.

# Hacking Karaoke
I have been studying and partaking in karaoke for over 20 years, and I want to make you aware of a critical vulnerability in the karaoke protocol. This weakness is not new, but it is gaining attention in certain circles of miscreants who like to break things and not follow the norms when it comes to the authenticity of musical performance. I had first demonstrated this attack to a small crowd at the [Neurolux in Boise, Idaho in 2016](https://www.facebook.com/wade.ronsse/videos/10155212437358465). Since then, I have been perfecting it in the privacy of my home so that I would not totally blow it when presenting it to a less dubious audience.

I call this attack "Lyrical Injection". Lyrical injection is where an attacker chooses a song and replaces the words with the lyrics and melody of another song in attempt of denial of service to the original song when performed at karaoke. It can be considered an extremely offensive operation, depending on the song choices. In some cases, it becomes viral and spreads to most of the people who are participating in the karaoke session; they are suddenly compelled to echo the lyrical payload without questioning why they are doing so.

Many songs are vulnerable to lyrical injection. The more popular the song is, the more vulnerable it is. Patrons of a karaoke session are commonly impacted by this attack, and attackers are also commonly patrons of a karaoke session. This attack is more likely to occur when alcohol is consumed. After years of studying this attack, I have determined that the attacker's primary motive is: lulz.

I first became aware of this attack when Neil Cicierga demonstrated it in 2014, though not in a live format. His version of this attack, on which my exploit is based, was superimposing the lyrical track to Smash Mouth's 1999 critically-acclaimed hit "All Star" onto the John Lennon's seminal 1971 work, "Imagine". A static version of this can be experienced here:  [Neil Cicierga - Imagine All Star People](https://www.youtube.com/watch?v=LMwOvPQtUUw). He has since released multiple albums including variations on this concept, and is canonically considered the pioneer of research in this field.

# The Demo

For my demonstration of Lyrical Injection, I chose the same target and lyrics for the song: Smash Mouth's "All Star" lyrics over John Lennon's "Imagine". I handed in a piece of paper with "Nate" as the name and "John Lennon - Imagine" as the song title. My operational security was weak here since I gave my real name, but since I desired attribution, it felt like an okay compromise. After a few hours, my name was called, I grabbed the microphone, briefly explained the vulnerability to the audience, and performed the demo live. 

*"Somebody once told me the world was gonna roll me"*: was met with confusion.
*"I ain't the sharpest tool in the shed"*: I could see some eyes lighting up.
*"She was lookin' kinda dumb with her finger and her thumb"*: I'm not kidding you, half of the audience was -
*"With the shape of an 'L' on her forehead"*: - making the shape of an L with their fingers and their thumbs on their forehead

Instead of *"Imagine all the people"*, the crowd and I both started in with *"Well, the years start coming and they don't stop coming"*. I could see the karaoke hosts shaking their damn heads, which was a clear indication that I had bypassed security mechanisms of the karaoke protocol and had succeeded in my mission. I finished the song and scrambled offstage victoriously.

# Reproduction
No technical skills are required, but some knowledge of music theory may help with the reconnaissance and weaponization of this attack. Being able to sing along with a song well will help with the effectiveness of the offensive operation. The methodology explained here is based on [The Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html).

#### 1.Reconnaissance
Choose a target song. With that song in mind, choose another song that you would like to extract the lyrics and melody from. This can be done by brute-forcing or other types of enumeration, though songs in similar keys will help narrow down the possibility of choices. In this step, additionally find a karaoke event that you would like to exploit.
#### 2. Weaponization 
Once you have selected the lyrics and melody from the source song, practice superimposing them onto an instrumental version of the target song. This attack requires a live performance, so practice this until it is perfected.
#### 3. Delivery
Go to karaoke.
#### 4. Exploitation
Write down a name (preferably not your own) and the target song title on a karaoke slip. Hand the karaoke host the slip. In the history of karaoke, there has never been a single instance of validation at this step, so if you have made it this far, your attack will most likely succeed. 
#### 5. Installation
When your (preferably fake) name is called, walk up to the karaoke host and retrieve the microphone. Hopefully they have sanitized the input at this point (the microphone), but it doesn't matter because you are about to exploit the heck ouf of them anyway.
#### 6. Command and Control
Perform the song. Move laterally around the stage.
#### 7. Actions and Objective
Finish the song. Return the microphone. Walk off stage. Profit.

# Prevention
Typical controls include the following:
* A deterrent control of projecting the lyrics on a screen that is visible to both the singer and the audience.
* A corrective control of turning down the microphone once the attack is detected. By this point, it is often too late.

That being said, there is no surefire defense against lyrical injection, which makes it particularly egregious. If you see this exploit in the wild, the safest thing to do is join in on the chicanery and hope for the best.

# Lessons Learned
If karaoke is not safe from hackers, then I'm afraid that nothing really is. I am releasing this security research for RESEARCH PURPOSES ONLY. Use this information at your own risk. The consequences for attempting this yourself range from a horde of followers who may join in on the mischief for a few minutes, to being permanently banned from karaoke. Consider yourself warned.