# Section 8: Digital Signatures Lab

## Objective
Understand and implement digital signatures for ensuring data integrity, authentication, and non-repudiation in cybersecurity applications.

## Learning Goals
- Learn how digital signatures work in public key cryptography
- Implement digital signature creation and verification
- Understand the role of hash functions in digital signatures
- Practice with real-world digital signature tools and techniques

## Lab Environment
- Operating System: Windows 10/11
- Tools: OpenSSL, GPG, PowerShell
- Test Data: Sample documents and certificates

## Step-by-Step Procedures

### Step 1: Understanding Digital Signatures
1. **Review digital signature concepts:**
   - Digital signatures use asymmetric cryptography
   - Private key signs, public key verifies
   - Provides integrity, authentication, and non-repudiation

### Step 2: Generate Key Pair
```bash
# Generate RSA key pair using OpenSSL
openssl genrsa -out private_key.pem 2048
openssl rsa -in private_key.pem -pubout -out public_key.pem
```

### Step 3: Create Digital Signature
```bash
# Create signature of a document
openssl dgst -sha256 -sign private_key.pem -out signature.bin document.txt
```

### Step 4: Verify Digital Signature
```bash
# Verify the signature
openssl dgst -sha256 -verify public_key.pem -signature signature.bin document.txt
```

### Step 5: Test Integrity
1. Modify the original document
2. Attempt to verify the signature
3. Observe verification failure

## Key Screenshots
- [Screenshot 1: Key pair generation]
- [Screenshot 2: Digital signature creation]
- [Screenshot 3: Successful signature verification]
- [Screenshot 4: Failed verification after document modification]

## Key Takeaways
- **Digital signatures provide three critical security properties:**
  - **Integrity:** Any modification to the signed data will cause verification to fail
  - **Authentication:** Confirms the identity of the signer
  - **Non-repudiation:** Signer cannot deny having signed the document

- **Hash functions are essential:** Digital signatures typically sign a hash of the data, not the data itself
- **Key management is crucial:** Private keys must be kept secure, public keys can be distributed
- **Real-world applications:** Used in code signing, email security, document authentication

## Security Implications
- Digital signatures are fundamental to PKI (Public Key Infrastructure)
- Essential for secure communications and data integrity
- Critical component in modern cybersecurity frameworks
- Used extensively in web security, email security, and software distribution

## Next Steps
- Practice with different hash algorithms (SHA-1, SHA-256, SHA-512)
- Explore certificate-based digital signatures
- Learn about timestamping and signature validity periods
- Study real-world implementations in web browsers and email clients

---
*Lab completed: September 10, 2025 | Duration: 2 hours | Status: ✅ Complete*
