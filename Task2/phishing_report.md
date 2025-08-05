# Phishing Email Analysis Report (Sample)

File: suspicious_eml_sample.eml
Analyst: Rajesh
Date: 2025-08-05

## 1. Visible fields
- From: support@secure-bank.com
- Reply-To: security-team@verify-now.example
- Subject: Urgent: Verify your Secure Bank Account NOW

## 2. Header auth results
- Authentication-Results: spf=fail; dkim=none; dmarc=fail

## 3. Links found (do NOT click)
- http://secure-bank.verify-account.example/login?user=12345
- http://198.51.100.23/verify?u=12345

## 4. Suspicious indicators observed
- From/Return-Path mismatch (support@secure-bank.com vs bounce@malicious-example.com)
- SPF/DKIM/DMARC indicate authentication failure
- Links use deceptive subdomain and direct IP
- Urgent, threatening language: "Your account will be suspended"
- Generic greeting "Dear Customer" (no personalization)

## 5. Verdict
- Assessment: **Phishing**
- Recommended actions: Mark as phishing, delete, do not click links or open attachments; report to security.

# Task 2 – Phishing Email Analysis

## Objective
Analyze a suspicious email to identify phishing indicators such as spoofed senders, fake links, and malicious attachments using only built-in Linux tools.

---

## commands used and their explaination:

sed -n '1,/^$/p' suspicious_eml_sample.eml > headers.txt
# Extracts everything from line 1 to the first empty line (email headers) and saves it to headers.txt.
# Email headers contain sender details, routing information, and authentication results useful for detecting spoofing.

grep -i 'Authentication-Results\|DKIM-Signature\|Received-SPF' suspicious_eml_sample.eml -n
# Searches the email for SPF, DKIM, and DMARC authentication results.
# The -i flag makes the search case-insensitive, and -n shows the line number of each match.
# Failures in these fields often indicate email spoofing.

grep -Eo '(http|https)://[^ >"]+' suspicious_eml_sample.eml | sort -u > urls_found.txt
# Finds all URLs in the email body that start with http or https.
# Uses a regular expression to match URLs, removes duplicates with sort -u, and saves them to urls_found.txt.

cat urls_found.txt
# Displays the unique URLs found in the email without opening them.
# Useful for manually inspecting suspicious links.

grep -iE 'filename=|name=' suspicious_eml_sample.eml
# Searches for attachment filenames in the email MIME headers.
# This can reveal if malicious files are attached (e.g., .zip, .exe, .doc with macros).

ripmime -i suspicious_eml_sample.eml -d attachments/
# Extracts attachments from the email into the 'attachments' folder.
# Allows safe inspection of file types without actually opening potentially harmful files.

