+++
title = 'Why Your Dominion Without AAA is a Sandcastle'
date = 2022-09-01T00:00:00-00:00
draft = false
+++

First, some housekeeping. I am tired of typing the word "company" when I talk about security.  It makes the assumption that this stuff only applies to a place of employment and excludes a wide audience of security-minded folks. From now on, I'm going to liberate us from this capitalistic tech-centric claptrap and instead use the word ***dominion***! When you see the word ***dominion***, know that I am talking about you, the sovereign entity that is defending its borders from unfettered chaos, internal mutiny, and draconian tyrannical rule! Bwahaha! Now, onto the good stuff.

We're not talking about batteries or an auto club. The specific fundamentals we'll be talking about today are: ***authentication***, ***authorization***, and ***accounting***, or the ***AAA model***. While every dominion has its own set of practices that are specific to their realm, I would argue that ***AAA*** applies to security in almost *any* field. These are by far not the only basics, and certainly not intended to replace a proper security framework. However, they are ones that compose a substantial amount of attack surface, and you would be well-advised to focus on getting them right. 

# The Basics

Look, I know that the basics are not sexy. No one gets into security because they want to enforce 20-character passwords and remind people why they have to install the latest updates to software. For anyone complaining, I implore you to pause and consider this: the vast majority of security incidents happen because of a failure to enforce the basics. According to the [Verizon 2022 Data Breach Investigation Report](https://www.verizon.com/business/resources/reports/dbir/), an overwhelming amount of security data breaches occurred as a result of stolen credentials. If you consider security incidents in general as well as breaches, weak authentication or authorization accounts for the vast majority of them. And that's just what we know about; security incidents are likely severely underreported because they have yet to be discovered due to a lack of good accounting practices. 

Ask yourself which one of these scenarios is worse: dealing with a breach because a criminal organization spent 3+ years performing reconnaissance and practicing to pull off an Oceans Eleven style attack? Or suffering a breach because someone in your dominion re-used a leaked password and you don't enforce multi-factor authentication? If a nation-state actor is after you, many of our security controls are viewed as hurdles and they will eventually find their way in with persistence. However, for better or worse, you are much, much likely to be attacked by less experienced actors exploiting basic weaknesses.

I am not saying that you should stop at the basics; what I'm saying is that if you don't get the basics right, the rest is not going to matter much. If the foundations are not in place, your roof is always going to dramatically collapse, and your dominion might as well be a sandcastle. If you are not protecting against common threats, then what is the strongest encryption known to the world going to do for you, or the coolest new threat detection tool? Attackers are always going to take advantage of the easiest thing for them to exploit and traverse right around all of the other defenses you put in place. Anyone with malicious intent will always go for the easiest and weakest spots first. One way or another, security incidents *are* going to happen. Knowing that, you *must* take proactive steps to reduce the blast radius and recover from them more quickly *when* they happen if you care about making your operation sustainable.

# AAA To The Rescue 

There are so many models and frameworks that help to fortify your posture. I have found that many of these are either difficult to understand, overly prescriptive or overly vague with controls, and many only apply to specific industries or domains. Don't get me wrong; they can be incredibly useful and comprehensive, though they are often heavyweight solutions and are not appropriate for everyone. In lieu of or, better yet, in supplement to using those frameworks, I believe that you can both prevent a lot of security incidents and recovery more quickly from them when they happen if you have solid answers to these questions:
* ***Authentication*** - How am I making sure that my users are who they say they are?
* ***Authorization*** - How am I ensuring that anyone, including unauthenticated users, can only take the minimum actions we need them to?
* ***Accounting*** - How am I monitoring the activity of actions taken in my system?

### Authentication

***Authentication*** is people and machines proving that they are who they say they are. If you think about high-security buildings, authentication is akin to the guard station inside, as well as the RFID readers that protect any locked doors in the building. People must authenticate themselves by proving their identity in order to access the building itself, whether it's through a badge, a code, or some sort of biometric scan. This doesn't dictate what they're allowed to do; it merely asks and verifies who they are. *Authorization* will decide whether or not they get through the door. 

How should someone prove their identity? Conventionally, there are 4 *factors* that allow for authentication:
* **Something you know.** This can be a password, a PIN, or some sort of other code that you are able to provide.
* **Something you have.** Badges, swipe cards, and software and hardware tokens can be used to authenticate based on something that you have. In the world of web applications, bearer tokens also rely on the principle that anyone has a bearer token is who they say they are.
* **Something you are.**  Biometric authentication involves fingerprint reading, facial or retinal scanning, or some other metric that is able to identify you using some attribute that is not easily changeable for you as a person.
* **Something about you right now**. Context about your authentication attempt can help to identify you. For example, where are you logging in from? Is it from a device that has been previously recognized, or at an unusual time, or in some other strange way?

On the internet, passwords are still for the primary way that people are authenticated, using *something that they know*. Passwords are also one of the biggest reasons that authentication gets compromised. Since nearly everyone on the internet has now had some of their data breached, attackers use this to their advantage by leveraging these breached credentials to try to authenticate as users. They will also use social engineering techniques such as phishing to gain their target's credentials. Since so many people re-use their passwords, it is unfortunately an extremely successful attack and can allow an attacker to pivot into many other systems from the initial compromise.

## Recommendations for Authentication
There is not one simple mitigation for every authentication problem; however, here are some controls that will go a long way to protect you:
* **Enforce MFA(multi-factor authentication) everywhere**. Do not settle for less than MFA; it is the number one most impactful thing you can do to help prevent your authentication from being broken. Demand that your vendors implement SSO as a basic feature to protect you and your data; [shame them loudly and publicly](https://sso.tax) if they make you pay more to get a very basic feature for protecting authentication. If a vendor does not offer MFA, I would recommend going with a vendor that does instead.
* **Eliminate as many passwords as possible**. Use an identity provider to allow your users to log in through SSO(single sign-on) of some kind. Password policies are not only notoriously difficult to enforce; they can also actually make it easier for an attacker to determine the correct password for a user. Have good password policies, but also migrate as much of your authentication to SSO as you can to reduce the surface for attack.
* **Enforce off-boarding procedures**. When folks leave your organization or are removed from your application, they no longer have a good reason to access almost anything that they had access to before; ***remove them promptly***. Automate this if at all possible; retained access to most systems will result in nothing but trouble in every conceivable direction.


### Authorization

***Authorization*** is enforcing the set of actions that a person or system is allowed to take. Once they are inside the building, *authorization* is where they are able to go and what they are able to do. Should everyone who authenticates to the building immediately have access to the HVAC and electrical systems? Should they have keys to every locked door or filing cabinet? Should they be able to access all of the records stored in the filing cabinets? What about logging into the network? The answers to these questions may not be universally consistent, but chances are, there are a ton of actions that you *don't* want every individual to take. Authorization takes into context whether a user is authenticated, and authenticated users can most often take a different set of actions.

A data breach is an attacker getting access to data that they should not have. If you think about it that way, every single data breach that has ever occurred is because of an authorization issue. "What about malware?" Attackers will use malware to exploit vulnerabilities to escalate privileges to do things that they are *not authorized to do*. This is sill an authorization issue. "What about social engineering attacks?" Malicious actors will use phishing, tailgating, manipulation, and all sorts of tactics, in order to gain entry into areas and systems that they are *not authorized to be in*. "But n0rtr0n, what about a developer going rogue and destroying our entire production database with no backups and no recourse for recovery?" Gulp! That is a pickle. It is also an *authorization* issue. Why would you ever give a developer the privileges to be able to do this in the first place? Privileges are powerful, and they define every single scenario that can or cannot happen.

You can think of authorization as the perimeter around your data and systems. The weakest part of the perimeter will be exploited. If you have holes in the fence, that is where an attacker will go. Ask yourself: what should people who are authenticated be able to do? What actions should unauthenticated people be able to take, and what do I want to forbid them from doing?  Whatever you do; do not stop there. You also need to evaluate how the privileges that someone can be given can be escalated to a higher level. If you leave the door the cabinet that contains all the keys to unlock rooms in the building, that is just as bad as not having locks at all. If that weakness is discovered, it can and will be abused in order to give a malicious actor the ability to do something that you do not want them to do.

## Recommendations for Authorization
 
* **Apply the principle of least privilege liberally**. What is the minimum set of actions that your users need to take in order to do their job? I realize that there may be a significant maintenance burden for managing granular permissions. Balancing coarse-grained and fine-grained permissions is as much of an art as it is a science. Just don't make everyone administrators, and limit what folks can do as much as is manageable.
* **Implement deny-first policies**. Have a valid reason for giving anyone access to do anything. Do your engineers get access to view your financial data? Does your CEO really need your API keys? Does your intern need to manage your email accounts? In the overwhelming majority of these kinds of situations, the answer should be a loud "***no***". It is much more difficult to revoke permissions once they are given later, so start with the most minimal workable allowlist up front.
* **Use defense in depth**. Once someone is authenticated to your network, what can they do and where can they go? Can they escalate their privileges further in some way, or access certain parts of the the network that you don't want them to? Are your networks segmented so that a security incident in one application won't affect another? The best approach is to assume that your perimeter has already been compromised, and to craft secondary, tertiary, or even quarternary(yes this is a word!) defenses to stop an attacker from proceeding if they are able to hop over the proverbial fence. 

### Accounting

***Accounting*** is the auditing of actions taken in the system, whether users are authenticated or not. It is logging, monitoring, alerting, and audit trails. It's similar to the cameras throughout the building, and the alarms that alert the guards to suspicious activity and allow them to see what's happening. If you don't have adequate monitoring of your system, then you do not have visibility and do not have the ability to determine what happened. 

Accounting *can* be a deterrent. Someone who wants to break into a building or system is less likely to do so the more likely they are to be seen or caught. Conversely, if an attacker knows that a system has weak accounting controls, that system becomes a more attractive target for dastardly deeds.

When you start to log every individual action that gets taken, the volume of data can become enormous. It's important to not only ingest this data, but to make it presentable so that you can action on it. It's usually a good idea to have some sort of Security Information and Event Management (SIEM) tool. Whether you use rules-based or anomaly-based detection, having some system in place to alert you is important, because life is short and mere mortals cannot possibly interpret all of the raw data in a meaningful way.

## Good Logs and Alerts

Good logs allow you to reconstruct the story of what happened. Good logs should indicate how who did what and when. Though not a comprehensive list, here are some things that you may want to monitor:
* Servers
* Databases
* User-facing applications
* Identity management systems
* E-mail
* SaaS tooling

Though also not a comprehensive list, here is what you most likely want to log:
* Authentication attempts
* Any create, read, update, or deletion of data
* IP addresses of users when actions were taken

While you want to log as much data as possible, you want to alert on the most minimum that is useful. Otherwise you risk alert fatigue, which can cause you to miss important events in your system. Good alerting is its own discipline; the shortest version is that your alerts should be clear and actionable.

## Privacy Still Matters

Keep in mind that the collection of this data can have privacy implications. GDPR and CCPA are two pieces of legislation in particular that have recently empowered users in the European Union and California, respectively, to be in control of their data. In addition to storing this data, you also need to implement the ability to purge and/or phase out data and activity for a specific user if they have the authority to action on data that you collect about them. Not doing this comes at the risk of lawyers getting involved. Consider yourself warned.

## Recommendations for Accounting
In short:
* **Enable logging, monitoring, and alerting**. Get alerted on the events that matter to your system. While logs in general are always useful for diagnostics and auditing, you should consider logs as there when you need them; otherwise focus on ingesting the right data and ensuring that your alert rules give you the signal that you need.
* **Protect your audit logs**. They must be available them in order for them to be useful, and they can be weaponized if they are accessed by the wrong people. You need to be able to trust the integrity of your log data and make sure that it cannot be altered in any way.
* **Audit access.** Do people have administrative access that should not? Are there lingering accounts from folks who are no longer with your organization? Do you know which accounts have passwords or other factors that are shared with people in your dominion? You need to be able to confidently answers these questions, and the answer to these will inevitably change over time.

# Closing Thoughts
You need to focus on getting the basics right. But how much should you do? Enough to meet your security goals. If the value of your systems and data is not very high, then design your systems controls according to that value. If you are running a pirate ship, then neglecting the basics comes at the risk of mutiny and chaos. If you are protecting a financial institution or anything related to health or human safety, then neglecting the basics can come at the cost of human lives and livelihood and be met with criminal consequences and irreparable brand damage.

Build a strategy around identity and access management that focuses on ***authentication***, ***authorization***, and ***accounting***. Get really good at these, and you will prevent a lot of security incidents and be able to recover much more quickly when they inevitably happen. Good luck to you and your dominion.

