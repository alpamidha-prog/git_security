# GitHub Security Investigation: Sensitive Data Management

## 1. What Never to Push to Git
To maintain the security of your applications and infrastructure, the following information must **NEVER** be committed to a version control system like GitHub:

- **API Keys**: Access keys for services like AWS, Google Cloud, Stripe, or OpenAI.
- **Private Keys**: SSH keys (`id_rsa`), SSL/TLS certificates (`.pem`, `.key`).
- **Database Credentials**: Hostnames, usernames, and passwords for databases.
- **Environment Variables**: Local configuration files like `.env` or `config.json`.
- **Personal Information**: Passwords, email addresses, or phone numbers in code comments or test data.

## 2. Investigation: Risks and Vulnerabilities
### Risks of Committed Secrets
Committing secrets to a repository can lead to:
- **Unauthorized Access**: Attackers can use stolen keys to access your cloud resources, databases, or third-party services.
- **Financial Loss**: Automated bots constantly scan public repositories for API keys (e.g., AWS keys) to mine cryptocurrency or use expensive services at your expense.
- **Data Breaches**: Stolen database credentials can lead to the exposure of sensitive user data, resulting in legal and reputational damage.
- **Account Takeover**: Private keys can be used to impersonate developers and push malicious code into production.

### Public vs. Private Repositories
- **Public Repositories**: Open to the entire internet. Any secret pushed here is considered compromised immediately. Bots usually find them within seconds of pushing.
- **Private Repositories**: Restricted to specific users. While "safer" than public repos, secrets should still be avoided. If a team member's account is compromised, the attacker gains access to all secrets in the private repo.

## 3. Best Practices for Protection
### Using .gitignore
The `.gitignore` file tells Git which files or directories to ignore. This is the first line of defense against committing sensitive data.

### Environment Variables (.env)
- **Use `.env` files**: Store configuration in a local `.env` file that is listed in `.gitignore`.
- **Use Templates**: Provide a `.env.example` file with placeholder values so other developers know which variables are required without exposing real values.
- **Secret Management Services**: For production, use services like AWS Secrets Manager, HashiCorp Vault, or GitHub Actions Secrets.

## 4. How to Handle a Leak
If you accidentally push a secret:
1. **Rotate the Secret Immediately**: Revoke the key and generate a new one. This is the only way to ensure security.
2. **Remove the Secret from History**: Use tools like `git-filter-repo` or BFG Repo-Cleaner to scrub the secret from the entire Git history (not just the latest commit).
3. **Notify Security**: Inform your team or the service provider if sensitive data was exposed.
