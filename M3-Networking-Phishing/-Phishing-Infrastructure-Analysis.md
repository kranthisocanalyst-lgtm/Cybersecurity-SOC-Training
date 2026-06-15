         Phishing Infrastructure Analysis

      1. Overview
Analyzed a phishing email sample to identify infrastructure indicators, sender spoofing, and malicious payload characteristics using header analysis and attachment inspection.
      
      2. Objective
Identify the true origin of a spoofed phishing email, extract IOCs, and determine the nature of the attached payload (benign vs malicious).
       
      3. Tools Used
Email header analyzer
CyberChef (base64 decoding)
TryHackMe lab environment

      4. Methodology
Examined email headers to identify X-Originating-IP and compare against the claimed sender domain
Decoded base64-encoded PDF attachment to inspect contents
Cross-referenced sender domain against the organization it claimed to represent (Home Depot)
Defanged extracted IOCs for safe documentation

       5. Key Findings
Spoofed organization: Home Depot
Actual sender: support@teckbe.com (mismatched domain)
X-Originating-IP: 103[.]234[.]236[.]83
Attachment result: Benign PDF (flag: THM{BENIGN_PDF_ATTACHMENT})

        6. What I Learned
How to spot domain mismatches as a red flag for spoofing
Importance of defanging IOCs before sharing/documenting
Not every suspicious attachment is malicious — verification matters before escalation

         7. Conclusion
This exercise reinforced practical email header analysis and the discipline of verifying claims (sender vs origin) rather than assuming malicious intent based on appearance alone.


