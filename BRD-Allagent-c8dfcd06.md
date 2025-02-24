# Project Requirements

To integrate compliance and regulatory standards into the **Business Requirements Document (BRD)** and ensure the database schema aligns with these requirements, the following updates and additions are proposed:

---

## **Compliance and Regulatory Standards**

### **Applicable Standards**
Based on the business requirements and the nature of the system, the following compliance standards are applicable:
1. **General Data Protection Regulation (GDPR)**: For handling personal data of users, especially if the system operates in the EU or processes data of EU citizens.
2. **Health Insurance Portability and Accountability Act (HIPAA)**: If the system processes or stores Protected Health Information (PHI).
3. **Payment Card Industry Data Security Standard (PCI-DSS)**: If the system processes or stores payment card information.
4. **California Consumer Privacy Act (CCPA)**: For handling personal data of California residents.
5. **ISO/IEC 27001**: For establishing an information security management system (ISMS).

---

### **Compliance Requirements and Implementation**

#### **1. GDPR**
- **Key Requirements**:
  - Data minimization: Only collect and store data necessary for the system's purpose.
  - Right to access, rectify, and delete data: Users must be able to access, update, or delete their personal data.
  - Data breach notification: Notify users and authorities within 72 hours of a data breach.
  - Data encryption: Encrypt personal data both in transit and at rest.
  - Data processing agreements: Ensure third-party integrations comply with GDPR.

- **Implementation**:
  - Add a `data_retention_policy` field in the `Users` table to track data retention periods.
  - Implement audit logs for all data access and modifications.
  - Encrypt sensitive fields like `email` and `password_hash` in the `Users` table.
  - Provide APIs for users to access, update, or delete their data.

#### **2. HIPAA**
- **Key Requirements**:
  - Ensure confidentiality, integrity, and availability of PHI.
  - Implement access controls and audit trails.
  - Encrypt PHI both in transit and at rest.
  - Conduct regular risk assessments.

- **Implementation**:
  - Add a `phi` field in the `Data Entities` table to flag records containing PHI.
  - Restrict access to PHI based on user roles (`role` field in the `Users` table).
  - Log all access to PHI in the `Audit Logs` table.
  - Encrypt PHI fields in the database.

#### **3. PCI-DSS**
- **Key Requirements**:
  - Encrypt payment card data.
  - Restrict access to cardholder data to authorized personnel.
  - Maintain a secure network and regularly test security systems.

- **Implementation**:
  - Add a `payment_info` table to store encrypted payment card data.
  - Restrict access to the `payment_info` table based on user roles.
  - Conduct regular vulnerability scans and penetration tests.

#### **4. CCPA**
- **Key Requirements**:
  - Provide users with the right to know what personal data is collected and how it is used.
  - Allow users to opt out of data selling.
  - Delete user data upon request.

- **Implementation**:
  - Add a `data_usage_policy` field in the `Users` table to track user consent.
  - Provide APIs for users to opt out of data sharing and request data deletion.

#### **5. ISO/IEC 27001**
- **Key Requirements**:
  - Establish an ISMS to manage information security risks.
  - Conduct regular audits and risk assessments.
  - Implement access controls and incident response plans.

- **Implementation**:
  - Define and document security policies and procedures.
  - Conduct regular security training for employees.
  - Implement multi-factor authentication (MFA) for user access.

---

### **Security Measures**
1. **Encryption**:
   - Use AES-256 for encrypting sensitive data (e.g., `email`, `password_hash`, `phi`).
   - Use TLS 1.2 or higher for encrypting data in transit.

2. **Access Controls**:
   - Implement Role-Based Access Control (RBAC) using the `role` field in the `Users` table.
   - Restrict access to sensitive tables (e.g., `payment_info`, `phi`) based on roles.

3. **Audit Logs**:
   - Log all access and modifications to sensitive data in an `Audit Logs` table.
   - Include fields like `user_id`, `action`, `timestamp`, and `affected_table`.

4. **Data Retention**:
   - Define data retention policies for each table.
   - Automatically delete or anonymize data after the retention period.

5. **Incident Response**:
   - Define an incident response plan for data breaches.
   - Notify affected users and authorities within the required timeframe.

---

### **Data Privacy Rules**
1. **Consent Management**:
   - Track user consent for data collection and processing in the `Users` table.
   - Provide APIs for users to update their consent preferences.

2. **Data Anonymization**:
   - Anonymize personal data in reports and analytics.
   - Use pseudonymization techniques for data processing.

3. **Data Access Requests**:
   - Provide APIs for users to request access to their data.
   - Log all data access requests in the `Audit Logs` table.

---

### **Validation of Compliance**
1. **During Implementation**:
   - Conduct regular code reviews to ensure compliance with security and privacy standards.
   - Perform penetration testing and vulnerability scans.
   - Validate encryption and access controls through automated tests.

2. **Post-Implementation**:
   - Conduct regular audits to ensure ongoing compliance.
   - Monitor access logs for unauthorized access.
   - Update security policies and procedures based on new threats and regulations.

---

### **Updates to the Database Schema**
To support compliance requirements, the following updates are proposed:

#### **1. Users Table**
- Add `data_retention_policy` (VARCHAR, 255): Defines the data retention period for the user.
- Add `data_usage_policy` (TEXT): Tracks user consent for data usage.

#### **2. Data Entities Table**
- Add `phi` (BOOLEAN): Flags whether the record contains PHI.

#### **3. Audit Logs Table**
- **Purpose**: To log all access and modifications to sensitive data.
- **Fields**:
  - `id` (Primary Key, UUID): Unique identifier for each log entry.
  - `user_id` (Foreign Key, UUID): References `users.id` to track the user.
  - `action` (VARCHAR, 50): Action performed (e.g., `INSERT`, `UPDATE`, `DELETE`).
  - `affected_table` (VARCHAR, 50): Name of the affected table.
  - `timestamp` (TIMESTAMP): Timestamp of the action.

#### **4. Payment Info Table**
- **Purpose**: To store encrypted payment card data.
- **Fields**:
  - `id` (Primary Key, UUID): Unique identifier for each payment record.
  - `user_id` (Foreign Key, UUID): References `users.id` to track the user.
  - `card_number` (VARCHAR, 255): Encrypted payment card number.
  - `expiry_date` (DATE): Expiry date of the card.
  - `cvv` (VARCHAR, 255): Encrypted CVV.
  - `created_at` (TIMESTAMP): Timestamp of when the payment info was added.

---

### **Updates to the ERD**
```
[Users] ---< [Roles]
   |
   |---< [Data Entities]
   |       |
   |       |---< [Audit Logs]
   |
   |---< [Reports]
   |
   |---< [Notifications]
   |
   |---< [Third-Party Integrations]
   |
   |---< [Payment Info]
```

---

These updates ensure that the system complies with all relevant regulatory and compliance standards while maintaining scalability, security, and usability.