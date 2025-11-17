# Lesson 2: Implementing Security Protocols with GitHub Copilot

## Demo Runbook (15-20 minutes)

**Target Audience:** Professional developers implementing secure authentication, encryption, and API security

**Demo Application:** PyGoat (deliberately vulnerable Python/Django application)

**Tools Required:**

- VS Code with GitHub Copilot Enterprise
- GitHub Enterprise Cloud (GHEC) with Advanced Security
- Python 3.10+ and pip
- Docker (for dependencies)
- Azure CLI (for Key Vault demo - optional)

---

## Prerequisites (Complete before demo - 5 minutes)

### 1. Clone and Setup PyGoat

```bash
git clone https://github.com/adeyosemanputra/pygoat.git
cd pygoat
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

### 2. Install Required Security Libraries

```bash
pip install pyjwt cryptography python-jose azure-identity azure-keyvault-secrets bcrypt
```

### 3. Configure Copilot for Security Patterns

Open Copilot Chat settings (Ctrl/Cmd+Shift+I) and enable security-focused completions.

---

## Demo Part 1: OAuth 2.0 with PKCE Implementation (6 minutes)

### Scenario

PyGoat uses basic authentication. We'll implement OAuth 2.0 with PKCE (Proof Key for Code Exchange) to secure the authentication flow.

### Step 1: Generate OAuth 2.0 Server with Copilot

Create `auth/oauth_server.py` and prompt Copilot:

**Copilot Prompt:**

```text
Generate a complete OAuth 2.0 authorization server implementation for Python/Django with:
1. PKCE support (code_verifier and code_challenge using SHA-256)
2. Authorization code flow with proper state parameter validation
3. Access token and refresh token issuance using JWT with RS256 signing
4. Token expiration (access: 15 minutes, refresh: 7 days)
5. Token rotation on refresh
6. Comprehensive error handling with OAuth 2.0 error codes

Include detailed inline comments explaining each security control.
Use the authlib library for OAuth primitives.
```

**Expected Outcome:**

Copilot generates 200+ lines of production-ready OAuth code including:

```python
from authlib.oauth2 import ResourceProtector, AuthorizationServer
from authlib.oauth2.rfc7636 import CodeChallenge
from werkzeug.security import gen_salt
import hashlib
import base64

class OAuth2Server:
    def __init__(self):
        # Initialize with PKCE support (RFC 7636)
        self.challenge = CodeChallenge(required=True)

    def generate_authorization_code(self, client_id, redirect_uri, scope,
                                    code_challenge, code_challenge_method):
        """
        Generate authorization code with PKCE.
        code_challenge_method must be 'S256' for security.
        """
        if code_challenge_method != 'S256':
            raise ValueError("Only S256 challenge method supported")

        # Generate cryptographically secure authorization code
        auth_code = gen_salt(48)

        # Store challenge for verification during token exchange
        # (in production: use Redis or database)
        self._store_challenge(auth_code, code_challenge, client_id)

        return auth_code

    def exchange_code_for_token(self, auth_code, code_verifier, client_id):
        """
        Exchange authorization code for access/refresh tokens.
        Validates PKCE challenge to prevent interception attacks.
        """
        # Retrieve stored challenge
        stored_challenge = self._get_stored_challenge(auth_code)

        # Compute challenge from provided verifier
        computed_challenge = base64.urlsafe_b64encode(
            hashlib.sha256(code_verifier.encode()).digest()
        ).decode().rstrip('=')

        # Constant-time comparison to prevent timing attacks
        if not secrets.compare_digest(stored_challenge, computed_challenge):
            raise ValueError("Invalid code verifier")

        # Generate JWT tokens with proper claims
        access_token = self._generate_jwt(
            client_id,
            exp_minutes=15,
            token_type='access'
        )

        refresh_token = self._generate_jwt(
            client_id,
            exp_days=7,
            token_type='refresh'
        )

        return {
            'access_token': access_token,
            'refresh_token': refresh_token,
            'token_type': 'Bearer',
            'expires_in': 900  # 15 minutes
        }
```

### Step 2: Generate JWT Validation Middleware

**Copilot Prompt:**

```text
Create Django middleware that validates JWT access tokens for all API endpoints.
The middleware should:
1. Extract Bearer token from Authorization header
2. Validate JWT signature using RS256 with public key
3. Check token expiration with 5-minute clock skew tolerance
4. Verify all required claims (iss, sub, aud, exp, iat, jti)
5. Prevent token reuse by checking jti (JWT ID) against revocation list
6. Set request.user based on validated token
7. Return proper 401/403 responses with WWW-Authenticate header

Add comprehensive error handling and logging.
```

**Expected Outcome:**

Middleware that implements all JWT security best practices with detailed validation logic.

### Step 3: Test OAuth Flow with Security Tests

**Copilot Prompt:**

```text
Generate pytest security tests for the OAuth 2.0 server that verify:
1. Authorization code cannot be reused
2. Invalid PKCE verifier is rejected
3. Expired tokens are rejected
4. Token signature tampering is detected
5. Missing claims cause validation failure
6. Token refresh requires valid refresh token
7. Scope enforcement works correctly

Use realistic attack scenarios and edge cases.
```

---

## Demo Part 2: Encryption and Key Management (5 minutes)

### Scenario

PyGoat stores sensitive user data in plaintext. We'll implement field-level encryption with Azure Key Vault integration.

### Step 1: Generate Encryption Service with Key Vault

**Copilot Prompt:**

```text
Create a Python encryption service class that:
1. Uses AES-256-GCM for encryption (provides authenticity + confidentiality)
2. Generates unique 96-bit IV for each encryption operation using secrets module
3. Integrates with Azure Key Vault for key management using DefaultAzureCredential
4. Implements key rotation with dual encryption for transition period
5. Handles initialization vector and authentication tag properly
6. Includes decrypt method with authentication tag verification
7. Provides key caching with 1-hour TTL to reduce Key Vault calls
8. Includes comprehensive error handling for Key Vault failures

Add type hints and docstrings following Google style.
Save as utils/encryption_service.py
```

**Expected Outcome:**

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend
import secrets
import base64
from typing import Tuple, Optional
from datetime import datetime, timedelta

class EncryptionService:
    """
    Provides field-level encryption using AES-256-GCM with Azure Key Vault.

    This implementation ensures:
    - Unique IV per encryption (prevents IV reuse vulnerabilities)
    - Authentication via GCM mode (detects tampering)
    - Secure key management via Azure Key Vault
    - Key rotation support
    """

    def __init__(self, key_vault_url: str):
        """
        Initialize encryption service with Azure Key Vault.

        Args:
            key_vault_url: Azure Key Vault URL (e.g., https://myvault.vault.azure.net/)
        """
        credential = DefaultAzureCredential()
        self.secret_client = SecretClient(
            vault_url=key_vault_url,
            credential=credential
        )
        self._key_cache = {}
        self._cache_expiry = {}

    def encrypt(self, plaintext: str, key_name: str = "data-encryption-key") -> str:
        """
        Encrypt plaintext using AES-256-GCM.

        Args:
            plaintext: Data to encrypt
            key_name: Name of encryption key in Key Vault

        Returns:
            Base64-encoded string containing IV + ciphertext + auth_tag

        Raises:
            EncryptionError: If encryption fails
        """
        try:
            # Fetch encryption key from Key Vault (with caching)
            key = self._get_key(key_name)

            # Generate cryptographically secure random IV (96 bits for GCM)
            iv = secrets.token_bytes(12)

            # Create AES-GCM cipher
            cipher = Cipher(
                algorithms.AES(key),
                modes.GCM(iv),
                backend=default_backend()
            )
            encryptor = cipher.encryptor()

            # Encrypt and get authentication tag
            ciphertext = encryptor.update(plaintext.encode()) + encryptor.finalize()
            auth_tag = encryptor.tag

            # Package: IV || ciphertext || auth_tag
            encrypted_data = iv + ciphertext + auth_tag

            # Return base64-encoded for storage
            return base64.b64encode(encrypted_data).decode('utf-8')

        except Exception as e:
            raise EncryptionError(f"Encryption failed: {e}")

    def decrypt(self, ciphertext_b64: str, key_name: str = "data-encryption-key") -> str:
        """
        Decrypt ciphertext and verify authentication tag.

        Args:
            ciphertext_b64: Base64-encoded encrypted data
            key_name: Name of encryption key in Key Vault

        Returns:
            Decrypted plaintext

        Raises:
            DecryptionError: If decryption fails or auth tag invalid
        """
        try:
            # Decode base64
            encrypted_data = base64.b64decode(ciphertext_b64)

            # Extract components: IV (12) || ciphertext || auth_tag (16)
            iv = encrypted_data[:12]
            auth_tag = encrypted_data[-16:]
            ciphertext = encrypted_data[12:-16]

            # Fetch decryption key
            key = self._get_key(key_name)

            # Create cipher with authentication tag
            cipher = Cipher(
                algorithms.AES(key),
                modes.GCM(iv, auth_tag),
                backend=default_backend()
            )
            decryptor = cipher.decryptor()

            # Decrypt and verify tag
            plaintext = decryptor.update(ciphertext) + decryptor.finalize()

            return plaintext.decode('utf-8')

        except Exception as e:
            raise DecryptionError(f"Decryption failed (possible tampering): {e}")

    def _get_key(self, key_name: str) -> bytes:
        """Fetch key from Key Vault with 1-hour caching."""
        # Check cache
        if key_name in self._key_cache:
            if datetime.now() < self._cache_expiry[key_name]:
                return self._key_cache[key_name]

        # Fetch from Key Vault
        secret = self.secret_client.get_secret(key_name)
        key = base64.b64decode(secret.value)

        # Cache for 1 hour
        self._key_cache[key_name] = key
        self._cache_expiry[key_name] = datetime.now() + timedelta(hours=1)

        return key
```

### Step 2: Implement Password Hashing with bcrypt

**Copilot Prompt:**

```text
Generate a password hashing utility for PyGoat that:
1. Uses bcrypt with work factor of 12 (appropriate for 2025)
2. Implements password verification with timing attack protection
3. Includes password strength validation (NIST 800-63B requirements)
4. Provides password rotation checks
5. Logs failed authentication attempts for security monitoring

Include usage examples and security best practices in docstrings.
```

### Step 3: Validate with GHAS Secret Scanning

After implementing encryption:

1. Attempt to commit a test file with a fake API key
2. Demonstrate GitHub Secret Scanning (with Copilot enhancement) catching it
3. Show push protection preventing the commit

**Key Talking Point:** GHAS secret scanning with Copilot uses AI to detect unstructured credentials with low false positive rates (new in 2025).

---

## Demo Part 3: API Gateway Security Middleware (5 minutes)

### Scenario

Implement comprehensive API security controls at the gateway layer.

### Step 1: Generate Rate Limiting Middleware

**Copilot Prompt:**

```text
Create Django middleware for API rate limiting that implements:
1. Sliding window algorithm (not fixed window - prevents burst circumvention)
2. Redis-backed distributed rate limiting for horizontal scaling
3. Different limits per endpoint (e.g., /login: 5/min, /api: 100/min)
4. Per-user and per-IP rate limiting
5. Exponential backoff for repeated violations
6. Custom headers (X-RateLimit-Limit, X-RateLimit-Remaining, Retry-After)
7. Integration with Django's cache framework
8. Graceful degradation if Redis is unavailable (fail open but log)

Use atomic Redis operations to prevent race conditions.
```

**Expected Outcome:**

Production-ready rate limiter with Redis integration and proper error handling.

### Step 2: Generate Request Validation Middleware

**Copilot Prompt:**

```text
Create middleware that validates all API requests for:
1. Content-Type header validation (reject non-JSON for API endpoints)
2. Request body size limits (max 1MB to prevent DoS)
3. JSON schema validation against OpenAPI specs
4. Header injection detection (newlines in headers)
5. SQL injection patterns in all input fields
6. XSS patterns in text inputs
7. Path traversal attempts in file parameters
8. IDOR prevention by validating resource ownership

Return proper 400 Bad Request with detailed validation errors.
Integrate with Django REST framework validators.
```

### Step 3: Implement CORS Security

**Copilot Prompt:**

```text
Configure Django CORS middleware with security best practices:
1. Whitelist specific origins (no wildcards in production)
2. Limit allowed methods to only what's needed
3. Set Access-Control-Max-Age appropriately
4. Handle preflight requests correctly
5. Implement origin validation with subdomain support
6. Add security headers (HSTS, X-Content-Type-Options, etc.)

Include configuration for both development and production environments.
```

---

## Demo Part 4: Integration with GitHub Advanced Security (3 minutes)

### Step 1: Enable GHAS on the Repository

Show GitHub.com repository Settings → Security:

1. Enable Dependabot alerts
2. Enable Code scanning (CodeQL)
3. Enable Secret scanning with push protection
4. Configure security policies

### Step 2: Demonstrate Copilot Autofix Integration

1. Navigate to Security → Code scanning alerts
2. Select a Python security alert (e.g., SQL injection)
3. Click "Fix with Copilot Autofix" (new 2025 feature)
4. Review Copilot-generated remediation PR
5. Show validation tests included in the PR

**Key Talking Point:** Copilot Autofix analyzes the vulnerable code, generates a targeted fix with context awareness, and includes tests to prevent regression.

### Step 3: Show CodeQL Custom Queries

**Copilot Prompt:**

```text
Generate a CodeQL query for Python that detects:
1. Django views missing authentication decorators
2. Hard-coded cryptographic keys in source code
3. SQL queries using string concatenation instead of parameterized queries
4. Sensitive data (SSN, credit card patterns) in logging statements

Provide the query in CodeQL syntax with explanatory comments.
Save as .github/codeql/custom-queries/python-security.ql
```

---

## Validation & Key Takeaways (1 minute)

### What We Demonstrated

1. **OAuth 2.0 with PKCE**: Generated 200+ lines of specification-compliant authentication code
2. **Enterprise Encryption**: Implemented AES-256-GCM with Azure Key Vault integration
3. **API Security**: Created layered defense with rate limiting, validation, and CORS
4. **GHAS Integration**: Leveraged Copilot Autofix for automated vulnerability remediation
5. **Custom CodeQL**: Built organization-specific security queries

### Prompting Best Practices

- **Specify security requirements explicitly**: Mention cipher modes, key lengths, token expiration
- **Request compliance alignment**: Reference NIST, OWASP, RFC standards
- **Ask for production-grade code**: Include error handling, logging, type hints
- **Demand testing**: Security implementations need security tests
- **Context matters**: Provide framework details (Django, Flask, FastAPI)

### Advanced Copilot Techniques Learned

1. **Chaining prompts**: Use output from one prompt as input to next
2. **Iterative refinement**: Start broad, narrow with follow-ups
3. **Security-first framing**: Begin prompts with threat model
4. **Validation requests**: Always ask Copilot to generate tests

---

## Next Steps

1. Implement OAuth in your production APIs
2. Migrate encryption keys to cloud KMS (Azure Key Vault, AWS KMS, GCP Secret Manager)
3. Build comprehensive API security middleware stack
4. Enable GHAS and configure Copilot Autofix
5. Move to Lesson 3: Automated Security Testing

---

## Troubleshooting

**Issue**: Azure Key Vault authentication fails

- **Solution**: Run `az login` and ensure Managed Identity or Service Principal is configured
- For local dev, use `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_CLIENT_SECRET` environment variables
- Check RBAC permissions on Key Vault (need "Get Secret" permission)

**Issue**: Redis connection errors in rate limiter

- **Solution**: Ensure Redis is running (`docker run -p 6379:6379 redis`)
- Update Django settings: `CACHES = {'default': {'BACKEND': 'django_redis.cache.RedisCache', 'LOCATION': 'redis://127.0.0.1:6379/1'}}`

**Issue**: Copilot generates deprecated crypto APIs

- **Solution**: Add to prompt: "Use cryptography library version 42+ APIs, not deprecated primitives"
- Specify Python version: "Generate for Python 3.10+"

---

## Additional Resources

- PyGoat: <https://github.com/adeyosemanputra/pygoat>
- OAuth 2.0 RFC 6749: <https://datatracker.ietf.org/doc/html/rfc6749>
- PKCE RFC 7636: <https://datatracker.ietf.org/doc/html/rfc7636>
- NIST Cryptography Standards: <https://csrc.nist.gov/projects/cryptographic-standards-and-guidelines>
- Azure Key Vault Docs: <https://docs.microsoft.com/azure/key-vault/>
