# Revocation of Digital Certificates: CRL, OCSP, OCSP Stapling

## Overview
This document provides an overview of three methods used to check the revocation status of digital certificates. Digital certificates are typically valid for one year, but certain conditions may necessitate early revocation, such as:
- Certificate is no longer in use
- Changes in certificate ownership
- Compromise of the certificate owner's primary key
- Certificates being stolen from the Certificate Authority (CA)

To ensure security, the CA must promptly publish both approved and revoked certificates. The following methods are used for certificate revocation checks:

## 1. Certificate Revocation List (CRL)

### Process
1. The CA maintains an online CRL database.
2. When a client makes a secure connection, it downloads the CRL from the CA.
3. The CRL contains a list of revoked certificates.
4. The client checks whether the presented certificate is on the revoked list.

### Limitations
- If a client cannot download the CRL, it may trust the certificate by default, negating the revocation check.
- High-traffic sites generate excessive requests to the CA, causing scalability issues.
- CRLs can become large over time, increasing download time and overhead.

**Diagram:**
```
Client → Requests CRL from CA → Downloads CRL → Checks Certificate Revocation Status
```

## 2. Online Certificate Status Protocol (OCSP)

### Process
1. The web server sends its certificate to the client.
2. The client queries the CA's OCSP responder to verify if the certificate is revoked.
3. The OCSP responder checks the certificate’s serial number and returns one of three statuses:
   - **Good**: The certificate is valid.
   - **Revoked**: The certificate has been revoked.
   - **Unknown**: The responder does not recognize the certificate.

### Advantages
- Reduces overhead compared to CRL since only a single certificate’s status is checked rather than downloading the entire CRL.
- Provides real-time revocation status.

### Limitations
- The client must contact the CA for every verification, leading to increased traffic to the OCSP responder.
- If the OCSP request fails, the client must choose between trusting the certificate (potentially insecure) or terminating the connection.

**Diagram:**
```
Client → Requests certificate status from OCSP Responder → OCSP Responder checks and replies with status
```

## 3. OCSP Stapling

### Process
1. The web server periodically queries the OCSP responder for certificate status.
2. The responder returns a time-stamped, digitally signed OCSP response.
3. When a client connects, the web server sends the stapled OCSP response along with the certificate during the SSL handshake.
4. The client verifies the stapled OCSP response without directly contacting the CA.

### Advantages
- Offloads the verification burden from the client to the web server.
- Reduces the number of requests to the OCSP responder.
- Improves connection speed and efficiency.
- Prevents privacy leakage by avoiding client-to-CA communications.

**Diagram:**
```
Web Server → Periodically requests OCSP response → Stores and Staples Response
Client → Receives Stapled OCSP Response → Verifies Status without contacting CA
```

## Conclusion
Among the three methods, **OCSP Stapling** offers the most efficient and secure approach by shifting the burden from clients to web servers while maintaining security and performance. This method ensures that all clients receive up-to-date certificate revocation information without requiring direct communication with the CA.

---

For further details, refer to the original video: [Revocation of Digital Certificates: CRL, OCSP, OCSP Stapling](https://www.youtube.com/watch/WXNKQ_otO_g).

