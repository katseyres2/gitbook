# 5. Auditing fundamentals

### Introduction

* Students will describe cybersecurity.
* Students will identify common frameworks and governing regulations.
* Students will explain cyber maturity.
* Students will preform network auditing.

### Cybersecurity basics

Definition : it's the protection of computer systems and networks from information disclosure, theft, or damage to their hardware, software or electronic data as well as from the disruption or misdirection of the services they provide.

What are we securing :

* Personally identifiable information
* Healthcare information
* Financial data
* Intellectuel property
* Business secrets
* Business operations

Securing from whom ?

* Criminals
* Competitors
* Insider threats
* Malicious actors

The CIA triad :&#x20;

* Confidentality : for your eyes only
* Integrity : only authorized changes
* Availability : I have a job to do here

<figure><img src="../../.gitbook/assets/40.png" alt=""><figcaption></figcaption></figure>

### Compliance

There are several regulations (you must put these things into place) a society must have.

**PCI DSS** (Payment Card Industry Data Security Standard)

* Mandated by card brands
* Administered by the Payment Card Industry Security Standards Council
* Created to increase controls around cardholder data to reduce credit card fraud

**HIPAA** (Health Insurance Portability and Accountability Act of 1996)

* US regulations for the use and disclosure of Protected Health Information (PHI)
* The Final Rule on Security Standards was issued on February 20, 2003
* Standards and Specifications
  * Administrative safeguards
  * Physical safeguards
  * Technical safeguards

**GDPR** (General Data Protection Regulation)

* Data protection and privacy law in the European Union (EU) and the European Economic Area (EEA)
* Controllers and processors of personal data must put in place appropriate technical and orgazinational measures to implement the data protection principles

**CPPA** (California Consumer Privacy Act)

* Intended to enhance privacy rights and consumer protection for residents of California, United States.
* Companies that become victims of data theft or other data security breaches can be ordered in civil class action lawsuits to pay statutory damages.
* Liability may also apply in respect of businesses in overseas countries who ship items into California.

**SOX** (Sarbanes-Oxley Act of 2002)

* US federal law mandates certain practices in financial record keeping and reporting for corporations.
* Requires strong internal control processes over the IT infrastructure and applications that house the financial information that flows into its financial reports in order to enable them to make timely disclosures to the public if a breach were to occur.

### Framework and maturity

How do we implement the business needs ? We can use frameworks to deal with them.

**ISO/IEC 27000** (International Standardization Organization and Internal Electrotechnical Commission)

* Deliberately broad in scope.
* Covering more than just privacy, confidentiality and IT/technical/cybersecurity issues.
* Applicable to organizations of all shapes and sizes.

It's broken in parts.

* ISO/IEC 27001 : information technology - security techniques - information security management systems - requirements
* ISO/IEC 27002 : code of practice for information security controls

**COBIT** (Control Objectives for Information and Related Technologies)

* Created by ISACA for information technology (IT) management and IT governance.
* Business focused and defines a set of generic processes for the management of IT.

**NIST** (National Institute of Standards and Technology)

* Catalog of security and privacy controls for all US federal information systems except those related to national security.
* Agencies are expected to be compliant with NIST security standards and guidelines.
* NIST special publication 800-53B provides a set of baseline security controls and privacy controls for information systems and organizations.

**CIS** (Center for Internet Security)

* Set of 18 prioritized safeguards to mitigate the most prevalent cyber-attacks.
* A defense-in-depth model to help prevent nd detect malware.
* Offers a free, hosted software product called the CIS controls self assessment tool (CIS-CSAT)

**CMMC** (Cybersecurity Maturity Model Certification)

* A training, certification, and third party assessment program of cybersecurity in the US government defense industrial base.
* Requires a third party assessor to verify the cybersecurity maturation level.
* 5 levels.

<figure><img src="../../.gitbook/assets/41.png" alt=""><figcaption></figcaption></figure>

**ASD Essential 8** (Australian Cyber Security Center - Essential Eight Maturity Model)

* Help organizations protect themselves against various cyber threats.
* Designed to protect Microsoft Windows-based internet-connected networks.
* 4 maturity levels.

<figure><img src="../../.gitbook/assets/42.png" alt=""><figcaption></figcaption></figure>

### Auditing

**Compliance or due diligence ?** Start to **interview** your CISO / SysAdmin / Whoever using information. **Review paperwork** to prevent lost of data. Use tools like **Nessus** to read logs, bugs, attack, etc.

### SCAP scan and Stigviewer

* SCAP (Security Content Automation Protocol) : defense information system agency (DISA).
* STIGs (Security Technical Implementation Guides) : guides and tools for standardization across organizations.

https://www.stigviewer.com/