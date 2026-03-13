![[Gemini_Generated_Image_6a0az76a0az76a0a.png]]



![[Gemini_Generated_Image_3goxwc3goxwc3gox 1.png]]


### **Security Architecture & Threat Model Breakdown**

The visualization integrates the functional data flow with the strategic threat scenarios identified during the audit:

- **External Entry Points:**
    
    - **App User & Bug Bounty Hunter:** Both entities interact with the system via a central **API Gateway**, which serves as the reverse proxy and authentication boundary.
        
    - **LocalStorage (Untrusted):** Highlighted on the client side as a high-risk area for **XSS** and **Insecure Storage** vulnerabilities, where sensitive auth tokens and PII were found to be persisted.
        
- **Microservice Clusters (Secure Boundary):**
    
    - **User & Profile Cluster:** Manages the core identity. The **Concierge** and **Profile** services are primary targets for **IDOR** attacks, potentially allowing unauthorized access to private conversations and user details.
        
    - **Financial Services Cluster:** Houses the **Wallet**, **Payments**, and **Liabilities** services. This is a critical attack surface for **IDOR** (e.g., card/account removal) and **Price Tampering** logic flaws.
        
    - **Lifestyle & Commerce Cluster:** Contains high-volume services like **Fitness** and **Restaurants**. Vulnerabilities here often involve business logic abuse and IDOR in booking/reservation systems.
        
- **Data Persistence & Storage:**
    
    - **Central Database:** The final destination for validated data.
        
    - **GCS (Signed Upload Storage):** A critical path for service request attachments, where **SSRF** and **LFI/XSS** through malicious file uploads are identified risks.
        
- **Primary Threat Scenarios:**
    
    - **Scenario A (Wallet Fraud):** Utilizing IDOR on the Wallet API to manipulate bank accounts or payment methods.
        
    - **Scenario B (Concierge Data Breach):** Unauthorized access to concierge message history via IDOR or exfiltrating data through insecure draft storage.
        
    - **Scenario C (PII Exfiltration):** Leveraging XSS to steal authentication tokens and PII (Name, DoB, Email) directly from LocalStorage.
        
    - **Scenario D (Internal Scanning):** Exploiting SSRF in external validation logic (e.g., SVG fetching or URL validation) to probe internal network resources.