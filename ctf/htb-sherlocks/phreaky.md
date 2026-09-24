# HTB Sherlock — Phreaky

**Evidence:** PCAP of SMTP traffic (data exfiltration).

## Approach
- Extracted the emails with **scapy** + Python's `email` module: 15 messages in total.
- Each message carried a base64 **AES-encrypted ZIP fragment**, with the password in the email body.
- Decrypted with **pyzipper**, concatenated `part1..part15` → a PDF containing the attacker's plan.
- Identified the insider's mailbox and the external recipient from the headers.

## Lesson
Automate the extraction. 15 manual decrypts is error-prone; a 30-line script isn't.
