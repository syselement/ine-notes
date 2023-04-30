# Social Engineering

> #### ⚡ Prerequisites
>
> * Basic Network and Cybersecurity Concepts
>
> #### 📕 Learning Objectives
>
> * Perform **phishing** and **watering hole** attacks
> * Describe **Social Engineering** fundamentals
> * Identify techniques and real-life scenarios

🗒️ **Social engineering** is a type of cyber attack that exploits human psychology to gain access to sensitive information or systems. Instead of using technical exploits to hack into a system, social engineers manipulate people into giving up confidential information (*gather information*) or performing actions that compromise security (*gain access*).

The success of social engineering attacks relies on the attacker's ability to **create a convincing scenario** that triggers an *emotional response, such as curiosity, fear, or urgency, in the victim*.

Social engineering through physical security is an effective tactic to gain **physical access** to a restricted building or office area.

Here are some examples of manipulating attacks:

- Phishing, Vishing, Smishing
- Impersonation
- Pretexting
- Emotional pull, Urgency
- Quid pro quo, Blackmail
- Watering hole

> ❗ **It's important to stay vigilant against phishing attacks and be wary of any unsolicited messages asking for personal or sensitive information.**

## Phishing

🗒️ **Phishing** is the most common form of social engineering attack. Attackers send fraudulent emails that appear to be from a reputable source, such as a bank or social media site, to trick the victim into revealing their login credentials, credit card numbers, or other sensitive information.

- **`e.g.`** Emails with malicious links and/or attachments

Common types of phishing attacks are:

- `Spear Phishing` - *targeted attack where the attacker researches and personalizes the message to increase the likelihood of success*
- `Whaling` - *targets high-profile individuals*, such as executives or celebrities, who have access to sensitive information or financial resources
- `Smishing` - phishing attack that *uses SMS text messages to trick the victim into revealing sensitive information or downloading malware*
- `Vishing` - phishing attack that uses *voice communication, such as a phone call, to trick the victim into revealing sensitive information*
- `Baiting` - *leaving a physical device, such as a USB drive, in a public place for the victim to pick it up and plug it into their computer, unintentionally downloading malware, keylogger, backdoor*

Getting people's trust is a big part of a phishing attack and can be done through different approaches like redirecting web traffic maliciously, targeting a trusted site and use it against the victim (**watering hole**), compromise business email (**BEC**), spoofing and impersonation.



> 📌 [FBI IC3 - 2022 Internet Crime Report](https://www.ic3.gov/Media/PDF/AnnualReport/2022_IC3Report.pdf)
>
> ![IC3 Report - 2022](.gitbook/assets/image-20230430173841740.png)
>
> ![IC3 Report - 2022](.gitbook/assets/image-20230430173938810.png)
>
> ![IC3 Report - 2022](.gitbook/assets/image-20230430174159761.png)
>
> 📌 [Trendmicro Security 101: Business Email Compromise (BEC) Schemes](https://www.trendmicro.com/vinfo/fr/security/news/cybercrime-and-digital-threats/business-email-compromise-bec-schemes)
>
> 📌 [CEO Fraud Attacks - KnowBe4](https://www.knowbe4.com/ceo-fraud)

It is essential for organizations to take social engineering attacks seriously and implement comprehensive cybersecurity measures that include **employee training**, **security awareness programs**, and regular **security audits and controls** to identify and address vulnerabilities in their systems and processes. *By staying vigilant and educating employees on how to identify and respond to social engineering attacks, organizations can significantly **reduce the risk** of a successful cyber attack.*

## Case Studies

- [Google and Facebook fake invoicing](https://www.trendmicro.com/vinfo/fr/security/news/cybercrime-and-digital-threats/google-and-facebook-fraudster-pleads-guilty-to-100-million-scam)
- [FACC CEO fraud](https://www.trendmicro.com/vinfo/pl/security/news/cybercrime-and-digital-threats/austrian-aeronautics-company-loses-42m-to-bec-scam)
- [Robinhood Vishing](https://www.bleepingcomputer.com/news/security/robinhood-discloses-data-breach-impacting-7-million-customers/)
- [Fake Excel file - Emotet](https://unit42.paloaltonetworks.com/new-emotet-infection-method/)
- [HTML Table Windows Logo](https://www.microsoft.com/en-us/security/blog/2021/08/18/trend-spotting-email-techniques-how-modern-phishing-emails-hide-in-plain-sight/)
- [FIN7 USB in Mail](https://www.bleepingcomputer.com/news/security/fbi-hackers-use-badusb-to-target-defense-firms-with-ransomware/)

## Penetration Testing

The purpose of **social engineering in a penetration test** is to *identify weaknesses in an organization's security systems and processes and to raise awareness among employees about the risks of social engineering attacks.*

> 📌 [Social Engineering Penetration Testing: Attacks, Methods, & Steps - Purplesec.us](https://purplesec.us/social-engineering-penetration-testing/)
>
> 📌 [Social Engineering Community ad DEFCON](https://www.se.community/)

- **Information Gathering** - *OSINT, dumpster diving, shoulder surfing, pretexting*
- **External Access** to internal I.T. infrastructure - *malware, credentials attacks, reverse shells*
- **Physical Access** - *attempt to gain physical access to the organization's facilities by impersonating employees or contractors, employees badges scan, malicious USBs, work people impersonation*

## [🔬 Gophish](https://getgophish.com/)

[**`Gophish`**](https://github.com/gophish/gophish) -  *a powerful, easy-to-use, open-source phishing toolkit meant to help pentesters and businesses conduct **real-world phishing simulations**.*

> ❗ **Phishing campaigns should only be conducted for legitimate security testing purposes**, and not for malicious intent. Additionally, it's crucial to obtain **permission** and **informed consent** from the costumer involved in the campaign.

```bash
# Linux Install

cd /opt/

sudo wget https://github.com/gophish/gophish/releases/download/v0.12.1/gophish-v0.12.1-linux-64bit.zip

sudo unzip -d gophish gophish-v0.12.1-linux-64bit.zip

sudo chmod +x gophish/gophish

cd /opt/gophish && sudo ./gophish
```

- To create a demo instance with a fake campaign, you can use `docker` and this command, made available by the Gophish maintainer [Jordan Wright](https://github.com/jordan-wright)
  - [Creating the Gophish Demo: Part One](https://getgophish.com/blog/post/2019-01-04-creating-the-gophish-demo-part-one/)
  - [Creating the Gophish Demo: Part Two](https://getgophish.com/blog/post/2019-01-11-creating-the-gophish-demo-part-two/)

```bash
docker run -ti -p 3333:3333 --rm gophish/demo
```

The basics steps to create a phishing campaign with Gophish are:

- Create a `Sending Profile`

![](.gitbook/assets/image-20230430215816312.png)

- Set up a new `Landing Page` - the landing page is the website or form that the victim will be directed to after clicking on the phishing link

![image-20230430220314084](.gitbook/assets/image-20230430220314084.png)

- Customize the `Email Template`- Gophish provides a set of pre-built email templates, but it can also be create by using the HTML editor or importing an existing template. Customize the email to make it look convincing and relevant to the target audience

![](.gitbook/assets/image-20230430220437682.png)

- Create `Users & Groups`- specify targeted users' emails or bulk import users

![image-20230430220810824](.gitbook/assets/image-20230430220810824.png)

- Create a `Campaign` - give the campaign a name, select the type of phishing campaign to create (such as credential harvesting or malicious attachment), and specify the targets and email template

![](.gitbook/assets/image-20230430220159433.png)

- `Launch the campaign` - Gophish will automatically send the phishing emails to the target list and track who clicked on the link and submitted data
- Analyze the results provided by Gophish as detailed metrics on email opens, link clicks and successful credential submissions.

------

