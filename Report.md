SQL Injection Playground — Report

## 1. Executive summary
I built a small Flask application with two login pages — a deliberately vulnerable login (to
demonstrate SQL injection) and a secure login (to demonstrate the correct fix). This report walks
step-by-step through the environment setup, how I reproduced the SQL injection on the vulnerable
page (with screenshots as evidence), why it worked (root cause), and the fixes I applied (and
recommended best practices).
## 2. Scope & purpose
Purpose: Educational — show how a simple insecure SQL query in a web app can be abused to
bypass authentication, and how parameterized queries / best practices stop that.
Scope: Local development environment only (a Flask app run at 127.0.0.1:5000).
## 3. Environment / setup
Python version: 3.13.7
Virtual environment: venv\Scripts\activate
Flask installed with pip install flask
## 4. Application overview
The app contains two login routes: /vulnerable (intentionally SQL injection vulnerable) and /secure
(safe with parameterized queries).
## 5. Step-by-step reproduction
Step 1 — Start the app using python app.py.
Step 2 — Open vulnerable login page and input username: admin' -- with any password.
Step 3 — Vulnerable login allowed unauthorized access. Logs show: SELECT * FROM users
WHERE username = 'admin' --' AND password = 'anything'
Step 4 — Secure login resisted the same input.
## 6. Root cause analysis
The vulnerable code concatenated user input into SQL. The `--` comment sequence bypassed
password checks, making authentication depend only on username.
## 7. Fixes & mitigations
1. Use parameterized queries (prepared statements).
2. Use an ORM (e.g., SQLAlchemy) and hash passwords.
3. Apply principle of least privilege for DB users.
4. Validate input length and types (defense-in-depth).
## 8. Conclusion & next steps
The exercise demonstrated how SQL injection works and why parameterized queries fix it. Next
steps: replace any concatenated SQL, hash passwords, and add automated tests for malicious
inp
