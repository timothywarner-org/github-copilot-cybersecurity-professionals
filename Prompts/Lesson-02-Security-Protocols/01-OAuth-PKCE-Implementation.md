# OAuth 2.0 Server with PKCE Implementation

**Generate complete OAuth 2.0 authorization server with PKCE support**

## Prompt

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

---

## When to Use

Use this when you need to:
- Implement secure OAuth 2.0 authentication
- Add PKCE protection for mobile/SPA apps
- Replace basic authentication with industry standard
- Comply with OAuth 2.0 security best practices (RFC 6749, RFC 7636)

---

## Expected Output

```python
from authlib.oauth2 import ResourceProtector, AuthorizationServer
from authlib.oauth2.rfc7636 import CodeChallenge
from werkzeug.security import gen_salt
import hashlib
import base64
import secrets

class OAuth2Server:
    def __init__(self):
        # Initialize with PKCE support (RFC 7636)
        # PKCE prevents authorization code interception attacks
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

---

## Customization

### For Node.js/Express
```text
Generate OAuth 2.0 server for Node.js/Express using passport-oauth2.
Include PKCE, JWT with RS256, and token rotation.
```

### For Spring Boot
```text
Generate OAuth 2.0 authorization server for Spring Boot using Spring Security OAuth.
Include PKCE, JWT tokens, and refresh token rotation.
```

---

## Follow-up Prompts

1. **Validation**: [JWT Validation Middleware](./02-JWT-Validation-Middleware.md)
2. **Testing**: [OAuth Security Tests](./03-OAuth-Security-Tests.md)
3. **Code Review**: [Interactive Security Audit](../Lesson-04-Code-Review-Threat-Modeling/Interactive-Code-Review.md)

---

## Security Checklist

- [ ] PKCE with S256 challenge method
- [ ] Cryptographically secure code generation
- [ ] Constant-time comparison for verifier
- [ ] JWT with proper claims (iss, sub, aud, exp, iat, jti)
- [ ] RS256 signing (asymmetric)
- [ ] Token expiration (short-lived access tokens)
- [ ] Refresh token rotation
- [ ] State parameter validation (CSRF protection)
- [ ] Error messages don't leak information

---

[← Back to Lesson 02 Index](./README.md) | [Next: JWT Validation →](./02-JWT-Validation-Middleware.md)
