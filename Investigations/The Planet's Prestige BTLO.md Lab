# Email Investigation Report

## Executive Summary

I investigated a suspicious email sent to **themajoronearth@gmail.com** that claimed to be from **Bill <billjobs@microapple.com>**. Initial header analysis identified several indicators of spoofing, including an SPF authentication failure, delivery through infrastructure unrelated to the claimed sender, and a mismatched Reply-To address.

The email contained a Base64 encoded message and an attachment presented as a PDF. Further analysis showed the attachment was actually a ZIP archive containing multiple files that hid several clues. The investigation ultimately revealed a hidden Base64 encoded message concealed within an Excel workbook, which decoded to:

> **The Martian Colony, Beside Interplanetary Spaceport.**

---

## Key Findings

- The email originated from **93.99.104.210 (emkei.cz)** rather than infrastructure associated with **microapple.com**.
- SPF authentication failed, indicating the sending server was not authorised to send emails on behalf of the claimed domain.
- The **Reply-To** address differed from the **From** address, which is a common phishing indicator.
- The attachment claimed to be a PDF but was actually a ZIP archive.
- Hidden information was recovered from files within the archive, including a Base64 encoded string concealed inside an Excel worksheet.

---

## Methodology

### 1. Email Header Analysis

I began by reviewing the email headers to determine whether the sender could be trusted.

The most significant finding was an **SPF failure**, showing that the sending IP address (**93.99.104.210**) was not authorised to send email on behalf of **microapple.com**.

I also noticed that although the email claimed to be from **billjobs@microapple.com**, replies would actually be sent to **negeja3921@pashter.com**. Combined with the sending infrastructure being **emkei.cz**, this strongly suggested the sender address had been spoofed.

---

### 2. Decoding the Email Body

The email body was Base64 encoded, so I used **CyberChef** to decode it.

The decoded message read:

> Hi TheMajorOnEarth,
>
> The abducted CoCanDians are with me including the President's daughter. Don't worry. They are safe in a secret location.
>
> Send me 1 Billion CoCanDs in cash with a spaceship and my autonomous bots will safely bring back your citizens.
>
> I heard that CoCanDians have the best brains in the Universe. Solve the puzzle I sent as an attachment for the next steps.
>
> My advice for the puzzle is:
>
> **"Don't Trust Your Eyes"**

---

### 3. Attachment Analysis

The attachment was advertised as a PDF.

After decoding the Base64 attachment, I examined the file signature using **HxD**.

The first four bytes were:

```text
50 4B 03 04
```

A legitimate PDF begins with:

```text
25 50 44 46
```

This confirmed the attachment was actually a **ZIP archive** rather than a PDF.

I saved the decoded file as **attachment.zip** before extracting its contents.

---

### 4. File Investigation

The ZIP archive contained three files.

Two of the files had no extension, so I used **HxD** to identify their true file types.

The first file was identified as a **JPEG** image showing a crown.





The second file was identified as a **PDF**, which contained the following message:

> CoCanDians are Safe.
>
> The proof is in the file named **DaughtersCrown**.
>
> Location to send **1 Billion CoCanDs** is in **Money.xlsx**.

The third file was confirmed to be an **Excel workbook** after verifying its file signature.

---

### 5. Hidden Data Discovery

Opening the workbook initially revealed a message claiming the ransom demand was fake and that a war with the CoCanDians had begun.

I noticed the workbook contained a second worksheet that appeared empty. Rather than assuming it contained no data, I highlighted the worksheet and selected **Clear → Formats**.

This revealed the following hidden Base64 string:

```text
VGhlIE1hcnRpYW4gQ29sb255LCBCZXNpZGUgSW50ZXJwbGFuZXRhcnkgU3BhY2Vwb3J0Lg==
```

I decoded the string using **CyberChef**, which revealed the final hidden location:

> **The Martian Colony, Beside Interplanetary Spaceport.**

---

## Indicators of Compromise (IOCs)

| Indicator | Value |
|-----------|-------|
| **Sender** | billjobs@microapple.com |
| **Reply-To** | negeja3921@pashter.com |
| **Recipient** | themajoronearth@gmail.com |
| **Sending IP** | 93.99.104.210 |
| **Sending Host** | emkei.cz |
| **Subject** | A Hope to CoCanDa |
| **SPF** | Failed |

---

## Tools Used

- CyberChef
- HxD
- Microsoft Excel
- Email Header Analysis
- Base64 Decoding

---

## Recommendations

Based on my findings, I would recommend:

- Investigating emails that fail SPF authentication, particularly when combined with other indicators of spoofing.
- Comparing the **From** and **Reply-To** addresses during email analysis, as mismatches are a common phishing technique.
- Verifying attachment file types by checking their file signatures rather than relying on filenames or MIME types.
- Analysing suspicious attachments in an isolated environment before opening them.
- Searching the environment for similar emails using the sender, subject, sending IP address or Message-ID to determine whether additional users received the same message.
- Continuing user awareness training to help users recognise phishing attempts involving spoofed senders and disguised attachments.
