# 🔐 Security & Secrets Management

## ✅ Security Best Practices Implemented

### AWS Systems Manager Parameter Store (SSM)
All sensitive credentials are stored **encrypted** in AWS SSM Parameter Store, **NEVER in code or GitHub**.

### Stored Parameters

| Parameter Path | Type | Description | Safe in Frontend? |
|---------------|------|-------------|-------------------|
| `/urlstatuschecker/production/stripe/public-key` | String | Stripe public key | ✅ YES (meant to be public) |
| `/urlstatuschecker/production/stripe/secret-key` | **SecureString** | Stripe secret key | ❌ NO (backend only, encrypted) |
| `/urlstatuschecker/production/google/client-id` | String | Google OAuth client ID | ✅ YES (meant to be public) |

### Access Control
- **Frontend** can use public keys (Stripe public key, Google client ID)
- **Backend Lambda** functions retrieve secret key from SSM at runtime
- SSM parameters are encrypted at rest using AWS KMS
- Lambda IAM roles grant minimal required permissions

---

## 🚫 What's NOT in GitHub

### Protected by `.gitignore`:
- ❌ `.env` files (all variants)
- ❌ `node_modules/`
- ❌ AWS credentials
- ❌ Private keys (`.pem`, `.key`)
- ❌ `credentials.md` files
- ❌ Build artifacts (`.zip`)

---

## 🔑 Secret Keys Reference

### Stripe Keys
- **Public Key** (pk_live_...): Safe to use in frontend JavaScript
  - Stored in SSM: `/urlstatuschecker/production/stripe/public-key`
  - Used in: Frontend `index.html` for Stripe.js
  - Purpose: Initialize Stripe checkout client-side

- **Secret Key** (sk_live_...): **NEVER expose in frontend**
  - Stored in SSM: `/urlstatuschecker/production/stripe/secret-key` (encrypted)
  - Used by: Backend Lambda functions only
  - Purpose: Process payments, create subscriptions, webhooks

### Google OAuth
- **Client ID**: Safe to use in frontend
  - Stored in SSM: `/urlstatuschecker/production/google/client-id`
  - Used in: Frontend for Google Sign-In button
  - Purpose: OAuth authentication flow

- **Client Secret**: **Backend only** (if needed)
  - Would be stored in SSM as SecureString
  - Never included in frontend code

---

## 🛡️ Backend Lambda Access to Secrets

### How Lambda Functions Retrieve Secrets

```javascript
// Example: Lambda function retrieving Stripe secret key
const AWS = require('aws-sdk');
const ssm = new AWS.SSM();

async function getStripeSecretKey() {
  const params = {
    Name: '/urlstatuschecker/production/stripe/secret-key',
    WithDecryption: true
  };

  const result = await ssm.getParameter(params).promise();
  return result.Parameter.Value;
}

// Use in handler
exports.handler = async (event) => {
  const stripeSecretKey = await getStripeSecretKey();
  const stripe = require('stripe')(stripeSecretKey);

  // Process payment...
};
```

### IAM Permissions Required
Lambda execution role needs:
```json
{
  "Effect": "Allow",
  "Action": [
    "ssm:GetParameter",
    "ssm:GetParameters"
  ],
  "Resource": "arn:aws:ssm:us-east-1:*:parameter/urlstatuschecker/production/*"
}
```

Plus KMS decryption for SecureString:
```json
{
  "Effect": "Allow",
  "Action": "kms:Decrypt",
  "Resource": "arn:aws:kms:us-east-1:*:key/*"
}
```

---

## 📝 Adding New Secrets

### Store a new secret:
```bash
# String (not encrypted)
aws ssm put-parameter \
  --name "/urlstatuschecker/production/service/key-name" \
  --value "your-value" \
  --type "String" \
  --description "Description"

# SecureString (encrypted with KMS)
aws ssm put-parameter \
  --name "/urlstatuschecker/production/service/secret-name" \
  --value "sensitive-value" \
  --type "SecureString" \
  --description "Description"
```

### Retrieve a secret:
```bash
# Get single parameter
aws ssm get-parameter \
  --name "/urlstatuschecker/production/stripe/secret-key" \
  --with-decryption

# Get all parameters for the app
aws ssm get-parameters-by-path \
  --path "/urlstatuschecker/production" \
  --recursive \
  --with-decryption
```

### Update a secret:
```bash
aws ssm put-parameter \
  --name "/urlstatuschecker/production/stripe/secret-key" \
  --value "new-secret-value" \
  --type "SecureString" \
  --overwrite
```

---

## ⚠️ Security Checklist

Before committing code, verify:

- [ ] No `.env` files in commit
- [ ] No API keys in code
- [ ] No passwords in code
- [ ] No AWS credentials in code
- [ ] `.gitignore` is up to date
- [ ] Public keys only in frontend
- [ ] Secret keys in SSM Parameter Store
- [ ] Lambda IAM roles have minimal permissions

---

## 🚨 Emergency: Secret Compromised

If a secret is exposed:

1. **Immediately rotate the key**:
   - Stripe: Generate new keys in Stripe Dashboard
   - Google: Regenerate in Google Cloud Console

2. **Update SSM parameter**:
   ```bash
   aws ssm put-parameter \
     --name "/urlstatuschecker/production/stripe/secret-key" \
     --value "NEW-SECRET-KEY" \
     --type "SecureString" \
     --overwrite
   ```

3. **Redeploy Lambda functions** (they cache parameters):
   ```bash
   aws lambda update-function-code \
     --function-name urlstatuschecker-api-production-functionName \
     --zip-file fileb://function.zip
   ```

4. **Revoke old keys** in service dashboard

---

## 🎯 Safe vs Unsafe

### ✅ SAFE to include in frontend code:
- Stripe **public** key (pk_live_...)
- Google OAuth **client ID**
- API endpoint URLs
- CloudFront distribution URLs
- Public constants

### ❌ UNSAFE - NEVER in frontend:
- Stripe **secret** key (sk_live_...)
- Google OAuth **client secret**
- Database credentials
- AWS access keys
- Private API keys
- Webhook signing secrets

---

## 📞 Questions?

If unsure whether something should be in SSM or code:
- **If it starts with "secret", "private", or "sk_"** → SSM SecureString
- **If documentation says "don't expose"** → SSM SecureString
- **If it's marked "public" or "publishable"** → Can be in code
- **When in doubt** → Use SSM SecureString

---

**Security is maintained. All sensitive data encrypted in AWS SSM.**

*Last updated: October 6, 2025*
