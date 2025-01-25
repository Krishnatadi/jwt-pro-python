![PyPI Version](https://img.shields.io/pypi/v/jwt-pro.svg)
![PyPI - Downloads](https://img.shields.io/pypi/dd/jwt-pro)
![Dependencies](https://img.shields.io/librariesio/release/pypi/jwt-pro)
![PyPI - License](https://img.shields.io/pypi/l/jwt-pro)


# JWT Pro - JWT Generation & Verification with AES Encryption

Welcome to `JWT Pro`, your go-to Python package for creating and verifying JSON Web Tokens (JWTs). With support for AES encryption and HMAC signatures, it ensures your user authentication and data transmission are as secure as possible. The package is highly customizable, letting you tweak encryption settings, headers, payloads and validation to fit your needs perfectly.

---

## Features

- **JWT Generation**: Create JSON Web Tokens with optional AES encryption.
- **HMAC Signatures**: Secure token signatures using HMAC with customizable algorithms.
- **Expiration Handling**: Automatic expiration handling with timestamp-based validation.
- **Customizable Headers & Payload**: Flexible header and payload creation.
- **Encryption Option**: AES encryption for protecting sensitive data in the payload.
- **Token Verification**: Validate tokens and verify signatures with proper error handling.

---

## Benefits

- **Security**: Ensures secure data transmission with AES encryption.
- **Ease of Use**: Simple API for token generation and verification.
- **Customization**: Flexible header and payload structures allow custom implementations.
- **Scalability**: Suitable for scalable applications with token-based authentication.
- **Reliability**: Automatic expiration checks and error handling for invalid tokens.

---

## Installation

This package is available through the [PyPI registry](__https://pypi.org/project/random-password-toolkit/__).

Before installing, ensure you have Python 3.6 or higher installed. You can download and install Python from [python.org](__https://www.python.org/downloads/__).

You can install the package using `pip`:

```bash
pip install jwt-pro

```

---

## Methods
| Method                    | Description                                                               |
|---------------------------|---------------------------------------------------------------------------|
| `generate_token()`        | Generates a JWT with a custom header, payload, and optional encryption.   |
| `verify_token()`          | Verifies a JWT token and checks its validity, expiration, and integrity.  |


---

## Encrypt Option (encrypt=True vs encrypt=False)

The `encrypt` parameter in the `generate_token()` and `verify_token()` methods controls whether the payload is encrypted using AES. Here’s how it behaves:

| encrypt Parameter | Behavior                                                         | Use Case                                                     |
|-------------------|------------------------------------------------------------------|--------------------------------------------------------------|
| `encrypt=True`    | - The payload is encrypted using AES with CBC mode.              | Use when sensitive data in the payload needs to be protected.|
|                   | - The token payload is stored in encrypted form and cannot be read directly. | Ideal for protecting data like passwords, user data, etc.  |
| `encrypt=False`   | - The payload is stored in plain text (unencrypted).             | Use when the data in the payload does not require encryption.|
|                   | - The payload can be directly read and is visible in the token.  | Suitable for non-sensitive, public data (e.g., user ID, session info). |

---

# Usage
## Importing the Package

```python
from jwt_pro import generate_token, verify_token
```

---

## Generate a JWT (Without Encryption)

```python
from jwt_pro import generate_token

# Define Header and Payload
header = {
    "alg": "HS256",  # HMAC-SHA256 algorithm
    "typ": "JWT"
}
payload = {
    "user_id": "12345",
    "name": "John Doe"
}
secret = "your-secret-key"
expiry = 3600 (default 3600)

# Generate JWT (without encryption)
token = generate_token(header, payload, secret, expiry, encrypt=False)

print(f"Generated Token: {token}")
```

---

## Verify a JWT (Without Encryption)
```python
from jwt_pro import verify_token

# Secret key used for signing
secret = "your-secret-key"

# Token to verify (use token from previous example)
token = "eyJhbGciOiAiSFMyNTYiLCAidHlwIjogIkpXVCJ9..."

try:
    verified_payload = verify_token(token, secret, encrypt=False)
    print(f"Verified Payload: {verified_payload}")
except ValueError as e:
    print(f"Verification Error: {e}")
```

---

## Generate JWT with AES Encryption
```python
from jwt_pro import generate_token

# Define Header and Payload
header = {
    "alg": "HS256",  # HMAC-SHA256 algorithm
    "typ": "JWT"
}
payload = {
    "user_id": "12345",
    "name": "John Doe"
}
secret = "your-secret-key"

# Generate JWT with AES encryption
token_encrypted = generate_token(header, payload, secret, expires_in=3600, encrypt=True)

print(f"Generated Encrypted Token: {token_encrypted}")
```

---

## Verify Encrypted JWT
```python
from jwt_pro import verify_token

# Secret key used for signing
secret = "your-secret-key"

# Encrypted token to verify (use token from previous example)
token_encrypted = "eyJhbGciOiAiSFMyNTYiLCAidHlwIjogIkpXVCJ9..."

try:
    verified_payload_encrypted = verify_token(token_encrypted, secret, encrypt=True)
    print(f"Verified Encrypted Payload: {verified_payload_encrypted}")
except ValueError as e:
    print(f"Verification Error: {e}")
```

---
## Token Expiration
The default expiration time for the token is **1 hour** (3600 seconds). If not explicitly specified during token generation, the token will automatically expire 1 hour from the time it was created.

You can change the expiration time by passing the `expiry` claim during the token generation process.

```python
from jwt_pro import generate_token
token = generate_token(payload, secret, expiry=7200)  # Token will expire in 2 hours

```

---

# Common Errors

| Error Type                           | Description                                                                 |
|--------------------------------------|-----------------------------------------------------------------------------|
| **ValueError: Token has expired.**   | Raised when the token has expired based on the `exp` field.                 |
| **ValueError: Invalid token format.**| Raised when the token format does not match the expected header.payload.signature format. |
| **ValueError: Invalid token header.**| Raised when the header is malformed or missing required fields.             |
| **ValueError: Invalid token payload.**| Raised when the payload cannot be decrypted or parsed.                     |
| **ValueError: Unsupported algorithm.**| Raised if the algorithm specified in the token header is unsupported.       |


---

# Use Cases

- **User Authentication**: Securely authenticate users in web applications by generating and verifying tokens.
- **Data Protection**: Encrypt sensitive data in the token payload and ensure its integrity during transmission.
- **Session Management**: Manage user sessions using JWTs with automatic expiration handling.
- **API Authentication**: Secure communication between microservices using JWTs for API authentication.

---
## Discussions
- **GitHub Discussions**: Share use cases, report bugs, and suggest features.

We'd love to hear from you and see how you're using **JWT PRO** in your projects!

---

## Requesting Features
If you have an idea for a new feature, please open a feature request in the Issues section with:
- A clear description of the feature
- Why it would be useful

---

## Issues and Feedback
For issues, feedback, and feature requests, please open an issue on our [GitHub Issues page](http://github.com/krishnatadi/jwt-pro-python/issues). We actively monitor and respond to community feedback.

---

## FAQ (Frequently Asked Questions)

### 1. **Why am I getting a `ValueError: Token has expired` error?**

**Answer:**  
This error occurs when the token's expiration time (the `exp` claim) has passed. Tokens have a built-in expiration time to limit their validity. If you encounter this error, try generating a new token using the `generate_token()` method, or check your server logic to ensure the token expiration time is appropriately set.

---

### 2. **What should I do if I receive a `jwt.exceptions.InvalidTokenError` during token verification?**

**Answer:**  
This error indicates that the token is malformed or invalid. Common reasons for this error include:
- The token has been tampered with.
- The wrong secret key or encryption method was used for verification.
- The token does not have the correct structure (header, payload, signature).

To resolve this:
- Ensure that the correct secret key or encryption key is used.
- Verify that the token has not been altered.

---

### 3. **How do I handle token expiration and renew the token?**

**Answer:**  
JWT tokens are typically designed to expire after a certain period (default 1hr or 3600s) . To handle token expiration, you can implement a **refresh token** mechanism. After the token expires, a valid refresh token can be used to generate a new access token. This can be done by generating a new token using the `generate_token()` function with the same user data.

---

### 4. **What is the difference between setting `encrypt=True` and `encrypt=False`?**

**Answer:**  
When setting `encrypt=True`, the JWT token is encrypted using AES encryption to ensure the payload is securely hidden. This is recommended when you want to protect sensitive data within the token. On the other hand, `encrypt=False` means the JWT is not encrypted, and only the base64-encoded payload is visible. 

| Option       | Behavior                                           |
|--------------|----------------------------------------------------|
| `encrypt=True` | Encrypts the payload using AES encryption.       |
| `encrypt=False` | No encryption; the token's payload is visible.   |

---

### 5. **How can I fix the `TypeError: 'NoneType' object is not callable` error during token verification?**

**Answer:**  
This error occurs when a required function or method is missing or improperly defined. If you're calling `verify_token()` and encountering this issue, check the following:
- Ensure that the `secret` parameter is provided and is not `None`.
- If using encryption, make sure the `encrypt=True` flag is set correctly and that the decryption function is properly implemented.

---

### 6. **What should I do if the `KeyError: 'alg'` error occurs in the token header?**

**Answer:**  
The `alg` key in the token header specifies the signing algorithm, such as HMAC, RSA, or AES. If the `alg` key is missing or invalid in the token's header, the verification process will fail.

To resolve this:
- Ensure the header of the JWT token includes the proper signing algorithm (e.g., `"alg": "HS256"` for HMAC).
- Double-check the configuration and the methods used for token generation.

---

### 7. **Can I use different encryption algorithms with JWT Pro?**

**Answer:**  
Currently, the package supports AES encryption by default when `encrypt=True`. If you'd like to use a different encryption algorithm, you can modify the source code or submit a pull request for additional algorithms. Future versions may include more encryption options.

---

### 8. **How can I handle missing or incorrect token headers?**

**Answer:**  
If the token header is missing or malformed, the token cannot be decoded or verified properly. Ensure that the token is generated correctly and that it follows the JWT structure (header, payload, signature).

To avoid this error:
- Validate the token structure before decoding it.
- Ensure that the token is correctly signed and includes a valid header.

---

### 9. **What happens if the `secret` key used during generation and verification doesn't match?**

**Answer:**  
If the `secret` key used to sign the token during generation is different from the one used for verification, the token will not be verified, and a `jwt.exceptions.InvalidSignatureError` will be raised. Always ensure that the same `secret` key is used for both token creation and verification.

---

### 10. **How can I handle unexpected `ValueError` or `TypeError` during token generation or verification?**

**Answer:**  
If you encounter unexpected errors such as `ValueError` or `TypeError`, make sure to:
- Verify that all required parameters (e.g., `payload`, `secret`, `encrypt`) are correctly provided and have the expected data types.
- Ensure that the `exp` claim is set correctly if you're using expiration times.
- Check if the `encrypt` flag is correctly set when generating or verifying encrypted tokens.

---

### 11. **Can I use JWT Pro in my production environment?**

**Answer:**  
Yes, JWT Pro is designed to be secure and efficient. However, before using it in production, ensure that:
- Your encryption and secret management practices are robust.
- You handle token expiration and renewal securely.
- You validate all tokens before using them in any sensitive operation.

---

### 12. **How do I debug JWT errors in my application?**

**Answer:**  
To debug JWT-related errors:
1. Check the token structure by manually decoding it using an online tool (e.g., [jwt.io](https://jwt.io/) or [TWDecoder](https://www.tialwizards.in/p/jwt-decoder.html)).
2. Log the generated tokens, headers, and payloads to verify their correctness.
3. Ensure that the secret key used in both generation and verification is correct.
4. If encryption is used, ensure that the encryption and decryption processes are correctly implemented.

---

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/Krishnatadi/jwt-pro-python/blob/main/LICENSE) file for details.
