## Prompt

Design a user registration system. Requirements:
- registration by email
- storing user data
- sending a confirmation email
- action logging

Return: a list of functions, a list of tables, the relations between them, and function contracts.

## Model response (Claude Opus 4.6)

# Scenario 1 — Claude Opus 4.6

- model: claude-opus-4.6
- date: 2026-03-15
- temperature: 0
- prompt: scenario-01-naive.md
- raw: yes (response preserved without editing)
- response-mode: text-only (after an explicit instruction to answer in text format)

---

# User Registration System

## Tables

**users**

| Column | Type | Note |
|---|---|---|
| id | uuid | PK, default gen_random_uuid() |
| email | varchar(255) | UNIQUE, NOT NULL |
| password_hash | varchar(255) | NOT NULL |
| is_verified | boolean | default false |
| created_at | timestamptz | default now() |

**email_tokens**

| Column | Type | Note |
|---|---|---|
| id | uuid | PK |
| user_id | uuid | FK → users.id ON DELETE CASCADE |
| token | varchar(64) | UNIQUE, NOT NULL |
| expires_at | timestamptz | NOT NULL |

**audit_log**

| Column | Type | Note |
|---|---|---|
| id | bigserial | PK |
| user_id | uuid | FK → users.id, nullable (for failed attempts) |
| action | varchar(50) | NOT NULL ('register', 'verify', 'verify_failed') |
| detail | jsonb | nullable, extra info (ip, email, error reason) |
| created_at | timestamptz | default now() |

## Relations

```text
users.id  ←──  email_tokens.user_id   (1 : N)
users.id  ←──  audit_log.user_id      (1 : N)
```

## Functions

### 1. `register_user(email, password) → User`

Creates a user and launches confirmation.

```text
Input:  email: str, password: str
Output: User (id, email, is_verified, created_at)
Errors: ValueError — email already taken or invalid

Logic:
  1. validate_email(email)
  2. hash = hash_password(password)
  3. INSERT INTO users (email, password_hash)
  4. token = create_verification_token(user.id)
  5. send_verification_email(email, token)
  6. log_action(user.id, 'register', {ip})
  7. return user
```

### 2. `create_verification_token(user_id) → str`

Creates a confirmation token.

```text
Input:  user_id: uuid
Output: token: str (64 characters)

Logic:
  1. token = secrets.token_urlsafe(48)
  2. expires_at = now() + 24h
  3. INSERT INTO email_tokens (user_id, token, expires_at)
  4. return token
```

### 3. `send_verification_email(email, token) → None`

Sends an email with the confirmation link.

```text
Input:  email: str, token: str
Output: None
Errors: SMTPError — sending failure

Logic:
  1. url = f"{BASE_URL}/verify?token={token}"
  2. send the email with `url` through SMTP/API
```

### 4. `verify_email(token) → User`

Confirms the user's email.

```text
Input:  token: str
Output: User
Errors: ValueError — token invalid/expired

Logic:
  1. SELECT * FROM email_tokens WHERE token = ? AND expires_at > now()
  2. if not found → log_action(None, 'verify_failed', {token}), raise
  3. UPDATE users SET is_verified = true WHERE id = user_id
  4. DELETE FROM email_tokens WHERE user_id = user_id
  5. log_action(user_id, 'verify', {})
  6. return user
```

### 5. `log_action(user_id, action, detail) → None`

Writes to audit_log.

```text
Input:  user_id: uuid | None, action: str, detail: dict
Output: None

Logic:
  1. INSERT INTO audit_log (user_id, action, detail)
```

### 6. `hash_password(password) → str`

```text
Input:  password: str
Output: bcrypt hash: str
```

### 7. `validate_email(email) → None`

```text
Input:  email: str
Output: None
Errors: ValueError — invalid format
```

## Call order

```text
register_user
  ├── validate_email
  ├── hash_password
  ├── INSERT users
  ├── create_verification_token
  │     └── INSERT email_tokens
  ├── send_verification_email
  └── log_action
        └── INSERT audit_log

verify_email
  ├── SELECT email_tokens
  ├── UPDATE users
  ├── DELETE email_tokens
  └── log_action
```

That is all. 7 functions, 3 tables, 2 relations. If you need an implementation in Python, say so.
