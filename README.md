# Detecting-and-Analyzing-a-Potential-Phishing-Attack.


In this blog post, I’ll walk you through how I detected and analyzed a potential phishing email received by an employee. This challenge was part of [TCMSecurity’s](https://academy.tcm-sec.com/) Hands-On Security Operations course, but it’s a great example of what SOC Analysts deal with in real life.


## The Scenario


![1](https://imgur.com/p7HyPZg.png)


“Imagine you’re a SOC Analyst at a corporation. An account executive notices a warning email in her inbox claiming her online access has been disabled. However, she can still access her online business platforms and inboxes. Sensing something fishy, she forwards the suspicious email to the security team’s phishing mailbox for review. How would you approach this potential security threat?”

Before we get into the nitty-gritty, let’s chat about phishing for a second:

Phishing is when bad guys pretend to be someone you trust to trick you into giving up sensitive info, like passwords or credit card numbers.

Phishing comes in different forms:

**Vishing:** This is when they try to scam you over the phone.

**Spear phishing:** When they target specific people.

**Whaling:** Going after the big fish (like CEOs)

These scammers are pretty clever. They use tricks like:

- Making you feel it’s super urgent
  
- Pretending to be an authority figure
  
- Trying to scare you
  
- Making you think you’ll miss out on something big
  
Phishing is bad news for companies. It can lead to data breaches, loss of money, and damage to the company’s reputation. That’s why it’s so important for folks like me to catch these attacks quickly, figure out what’s going on, and stop them from happening again.

The dangers of phishing to a company are significant, including data breaches, financial losses, and reputational damage. That’s why it’s crucial for SOC Analysts to detect phishing attacks promptly, analyze them thoroughly, and implement robust defense actions to mitigate future attacks from the same or similar sources.

## Let’s Look at That Email

![1](https://imgur.com/p7HyPZg.png)

Okay, so here’s what this suspicious email is saying:

- It claims to be from the Outlook Support team.
  
- States that the user’s account has been flagged for unusual activity and disabled.

- Urges the user to “re-verify” their account by clicking a button to log in to their Microsoft account.
  
- Threatens account suspension within 24 hours if ignored
  
The first red flag is the sense of URGENCY, a classic sign of a phishing attempt. The email tries to create fear to manipulate the recipient into taking hasty action.

However, as a SOC Analyst, we don’t jump to conclusions. Instead, we approach this potential security incident with the same thoroughness a detective would apply to a criminal investigation.

These were the steps taken to analyze the email:

--
## 1. Grabbing the Email Safely
First things first, I needed to get that email without risking anything:

I downloaded the email as an .eml file and saved it in a folder
Make sure not to click any links in the email
This way, I can poke around in the email’s guts without accidentally setting off any traps.

--
## 2. Analyze the Email Source Code
   
Email source code is like the DNA of an email — it contains crucial information that isn’t visible to the naked eye. Let’s break down its key components:

**Headers:** Metadata about the email (Sender, recipient, subject, return path, Email Authentication- SPF, DKIM & DMARC, etc.)
**Body:** The actual content of the email (text, HTML)
**Attachments:** Any files included with the email

To view the source code, I used Sublime Text, a versatile text editor available for multiple operating systems.

![2](https://imgur.com/RwiW9cl.png)

For a quicker analysis, I also utilized the [DidierStevensSuite’s](https://github.com/DidierStevens/DidierStevensSuite) “eioc.py” Python script to extract essential header details.

![3](https://imgur.com/rd6nkyY.png)

Here are the key details extracted from the email headers:

![4](https://imgur.com/RqIHXQH.png)

The body of the email contains both encoded content and HTML structure. I used [CyberChef](https://gchq.github.io/CyberChef/), an online tool, to decode the base64 encoded text. Additionally, I defanged the embedded URL for safe documentation:

Defanged URL: hxxps[://]0[.]232[.]205[.]92[.]host[.]secureserver[.]net/lclbluewin08812/

The email has no attachment, if there was one, either static MalDoc Analysis or Dynamic attachment Analysis and Sandboxing could have been carried out using tools like [Joesandbox](https://www.joesandbox.com/#windows) or [AnyRun](https://app.any.run/)

--
## 3. Artifact Analysis
**A. Sender Analysis**
The email claims to be from the “Outlook Support Team” but uses a suspicious domain (**cial.**lwan.edu.eg) unrelated to Microsoft’s official domains. I also went further to analyze the base domain (**cial.**lwan.edu.eg) on [VirusTotal](https://www.virustotal.com/gui/url/c414cf3f5647cfea3f9b69ecb5d7c9f7008361368eaf2920b8b92c8e1e52dd90) and it came back malicious, a vendor already reported the link as a malicious.

![5](https://imgur.com/TLc14wf.png)





**B. IP Address Lookup**
I used WHOIS to investigate the Sender’s IP (40.107.22.60).

![6](https://imgur.com/9eGTcPv.png)

The IP is owned by Microsoft Corporation, and the resolved host (***************2060.o*tb**nd.protection.outlook.com) indicates it was sent via an Office 365 account. While this might seem legitimate, it’s important to note that attackers can sometimes exploit Office 365 accounts or spoof email headers.





**C. URL Analysis**
I checked analyzed the embedded URL using VirusTotal and 3 (Fortinet, Trustwave, Kaspersky) out of 96 security vendors flagged this URL as malicious, specifically for phishing.

![7](https://imgur.com/l2AvEB7.png)

Other indicators that this is a phishing email is that, the email is not properly formatted and there are grammatical errors in the email which I believe are not usually present in a genuine or legite email.

--
## 4. Verdict
Based on this analysis, I can conclude that:

The Sender is not who they claim to be, the sender was trying to impersonate Outlook Support Team.
The URL embedded in the email is MALICIOUS
This is a PHISHING ATTACK

--
## 5. Defense Actions
To protect the organization and prevent future attacks, I recommended the following actions:

- Check for signs of compromise, such as unauthorized access to the employee’s accounts or data, and take necessary remediation steps.
  
- Immediately block the Sender’s email address in the system to prevent further emails from being delivered.
  
- Update or create email filtering rules to detect and block similar phishing attempts based on identified keywords and sender addresses.
  
- Inform all employees about this phishing attempt and provide guidance on recognizing and handling suspicious emails.
  
- As we’ve seen, phishing attacks are becoming increasingly sophisticated, but they’re not unbeatable. By following a methodical approach to email analysis, we can uncover the truth behind suspicious messages.

The key to effective cybersecurity is a combination of technical skills, critical thinking, and constant vigilance.

As a SOC Analyst, my job is to stay one step ahead of the attackers, protecting organization’s data and reputation.

But security isn’t just the responsibility of the IT department. Every employee plays a crucial role in the defense against phishing.

By staying alert, questioning unusual requests, and reporting suspicious emails, we all contribute to a stronger, more secure digital environment.

I hope this analysis has shed light on this phishing email and how to spot them. Your online security is crucial, so stay vigilant!

If you found this information helpful, kindly follow my github page for more SOC posts. And I’d love to hear from you — feel free to share in the comments any additional red flags or tips you use to identify phishing attempts.

Together, we can create a more secure digital environment for everyone. Stay safe out there!



