# The Planet's Prestige

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

<img width="1019" height="278" alt="image" src="https://github.com/user-attachments/assets/0fe8988c-76da-4cf2-8fe6-03e70c3bb0a0" />


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


<img width="1883" height="892" alt="image" src="https://github.com/user-attachments/assets/c36b44e1-0e69-456e-b719-6444d9c18eeb" />



The second file was identified as a **PDF**, which contained the following message:


<img width="846" height="548" alt="image" src="https://github.com/user-attachments/assets/fbf90bf5-0eff-481a-9d40-419d181c6a09" />



The third file was confirmed to be an **Excel workbook** after verifying its file signature.

---

### 5. Hidden Data Discovery

Opening the workbook initially revealed a message claiming the ransom demand was fake and that a war with the CoCanDians had begun.

<img width="851" height="660" alt="image" src="https://github.com/user-attachments/assets/5cc77d29-5b0d-430e-8c61-8faf525c61c2" />


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
| **SPF** | Failed |

---

## Tools Used

- CyberChef
- HxD
- OnlyOffice
- Email Header Analysis (NotePad++)

---

## Recommendations

Based on my findings, I would recommend:

## Recommendations

Based on what I found during this investigation, I would recommend:

- Investigating any email that fails SPF authentication, especially when it's combined with other indicators such as mismatched sender information.
- Always comparing the **From** and **Reply-To** addresses, as this investigation showed how they can be used to redirect replies to a different mailbox.
- Verifying the true file type of attachments by checking their file signatures instead of relying on the filename or MIME type, as the attachment in this case was disguised as a PDF.
- Opening suspicious attachments in an isolated environment to reduce the risk of executing malicious content.
- Searching the environment for similar emails using the sender address, subject, sending IP or Message-ID to identify whether additional users may have been targeted.
- Continuing user awareness training so users are better equipped to recognise spoofed emails and suspicious attachments.
