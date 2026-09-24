# HTB — Enigma

- **NFS enumeration:** `showmount -e` → `/srv/nfs/onboarding`. The mount failed
  until I added `-o nolock,vers=3`.
- **Mail stack:** Dovecot + Roundcube.
- **Initial access:** phishing callback via the mail system.
