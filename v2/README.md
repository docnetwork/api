# DocNetwork APIv2 Documentation

## Chapters

1. [Onboarding](./chapters/onboarding.md)
1. [Authorization](./chapters/oauth.md)
1. [Requests](./chapters/requests.md)
1. [Data Model](./chapters/data-model.md)
1. [Profiles](./chapters/profiles.md)
1. [Groups](./chapters/groups.md)
1. [Users](./chapters/users.md)
1. [Registrations](./chapters/registrations.md)
1. [Questions & Answers](./chapters/questions-answers.md)
1. [Tags](./chapters/tags-overview.md)

## Postman collection

[Postman](https://www.postman.com/) helps developers interact with RESTful APIs like DocNetwork's. [Collections](https://www.postman.com/collection/) in Postman help organize and annotate API requests to browse and reuse.  We've prepared a Postman Collection to help you get started with our API.  Find the Collection [here](./collections/DN_API_v2_collection.json).  We hope it's helpful!

## API Usage Policy 
This API Usage and Security Policy ("Policy") governs the access and use of the DocNetwork Application Programming Interface ("API") provided by DocNetwork, Inc. ("Company"). This Policy applies to all developers, clients, integrations, and third-party systems (collectively, "Users" or "You") accessing the API.

As a provider committed to the highest standards of data protection, this Policy is designed to ensure continuous alignment with the **Health Insurance Portability and Accountability Act (HIPAA)** and the **Five Trust Services Criteria (Security, Availability, Processing Integrity, Confidentiality, and Privacy) of SOC 2**.

## 1. Regulatory Alignment & Protected Health Information (PHI)

The DocNetwork API may facilitate the transmission of Protected Health Information ("PHI") and electronic PHI ("ePHI") as defined under 45 CFR § 160.103.

* **Business Associate Agreement (BAA) Required:** You are strictly prohibited from utilizing the API to access, transmit, create, or receive PHI unless a valid, fully executed Business Associate Agreement (BAA) or a Master Services Agreement incorporating BAA terms is active between Your organization and DocNetwork.
* **Minimum Necessary Standard:** In compliance with the HIPAA Privacy Rule, Users must restrict API queries to the absolute minimum amount of data necessary to accomplish the intended, legally permitted purpose. Requesting broad data dumps or unauthorized endpoints is a severe violation of this Policy.
* **Privacy Controls:** Users must maintain robust privacy mechanisms ensuring that any data gathered via the API honors individual user consents, opt-outs, and applicable state or global regulations (including but not limited to CCPA/CPRA and GDPR).

## 2. Technical Security Hygiene (SOC 2: Security & Confidentiality)

To safeguard system endpoints and the confidentiality of data in transit and at rest, Users must adhere to the following security protocols:

* **Credential Management:** API keys, tokens, and secrets (collectively, "Credentials") are strictly confidential.
* Credentials must **never** be hardcoded into client-side code, mobile applications, or publicly accessible repositories (e.g., GitHub).
* Credentials must be stored securely using environment variables or dedicated secret management services.
* Sharing Credentials between distinct systems or individuals is explicitly prohibited.


* **Encryption in Transit:** All calls to the DocNetwork API must utilize secure transport protocols. The API exclusively accepts traffic encrypted via **TLS 1.3** (or a minimum fallback of TLS 1.2, depending on current industry standard deprecation timelines). Unencrypted HTTP requests will be rejected.
* **Authentication & Expiration:** Users must implement programmatic token rotation and handle short-lived session tokens as dictated by the DocNetwork technical documentation.

## 3. System Availability & Processing Integrity (SOC 2: Availability & Processing Integrity)

To guarantee high availability for all clients and maintain the processing integrity of data operations, the following restrictions are enforced:

* **Rate Limiting & Throttling:** DocNetwork enforces strict programmatic rate limits on API endpoints. Users must design their applications to gracefully handle `429 Too Many Requests` HTTP status codes using an exponential backoff strategy.
* **Prohibited Abuse:** Any attempt to artificially bypass, manipulate, or stress-test rate limits, or to conduct unauthorized vulnerability scans, is a material breach of this Policy.
* **Data Input & Integrity:** Users are responsible for ensuring that all data written *to* the DocNetwork API is accurately formatted, free of malicious payloads (e.g., SQL injection, Cross-Site Scripting scripts), and strictly adheres to the schema validation rules defined in the API documentation.
* **Scraping & Reverse Engineering:** Automated data scraping, bulk harvesting of participant directories, or reverse engineering of the underlying DocNetwork software via the API is strictly prohibited.

## 4. Mandatory Incident Reporting & Breach Notification

In alignment with the HIPAA Breach Notification Rule (45 CFR §§ 164.400-414) and SOC 2 incident response requirements, immediate transparency is required in the event of a security failure.

* **Notification Window:** If a User discovers or reasonably suspects any security incident, credential compromise, or unauthorized access involving data retrieved through the DocNetwork API, the User **must notify DocNetwork via [security@docnetwork.com] within twenty-four (24) hours**.
* **Cooperation:** The User must cooperate fully with DocNetwork’s security and legal teams to investigate the incident, mitigate the risk, and satisfy any regulatory notification requirements obligated under HIPAA or state data protection acts.

## 5. Compliance Monitoring & Auditing

DocNetwork reserves the right to continuously monitor API traffic, call volumes, and payload patterns to ensure compliance with this Policy.

Upon reasonable request, Users may be required to provide DocNetwork with documentation or attestation demonstrating their adherence to baseline data protection laws, encryption standards, and secure credential handling practices.

## 6. Consequences of Non-Compliance

Failure to adhere to any provision within this Policy may result in immediate, unilateral action by DocNetwork, including but not limited to:

* Temporary throttling or IP-blocking.
* Revocation of API Credentials and suspension of account access.
* Permanent termination of contractual API rights.
* Referral to state and federal regulatory bodies in the event of an unmitigated HIPAA breach.

## 7. Policy Updates

DocNetwork reviews this policy periodically to maintain compliance with evolving SOC 2 Trust Services Criteria and HHS regulations. Users will be notified of material changes via email or system dashboards. Continued use of the API following an update constitutes acceptance of the revised Policy.
