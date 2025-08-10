# GPG Key Recovery

## Key Location
Primary GPG key backup is stored on: **NAS**

## Recovery Instructions

### 1. Locate Key Backup
- Access NAS storage
- Navigate to GPG backup directory
- Verify key files are present and accessible

### 2. Import Keys
```bash
# Import private key
gpg --import /path/to/private-key.asc

# Import public key (if needed)
gpg --import /path/to/public-key.asc
```

### 3. Verify Import
```bash
# List imported keys
gpg --list-secret-keys
gpg --list-keys

# Test decryption with a test file
gpg --decrypt test-file.gpg
```

### 4. Trust Key
```bash
gpg --edit-key YOUR_KEY_ID
# In GPG prompt: trust, then select trust level (usually 5 - ultimate)
```

## Security Checklist

- [ ] Verify key fingerprint matches original
- [ ] Test key functionality with existing encrypted files
- [ ] Update key expiration if necessary
- [ ] Sync with YubiKey if applicable

## TODO
- [ ] **URGENT**: Print GPG keys and store in secure physical location (safe, bank deposit box, etc.)
- [ ] Create additional backup copies on separate storage devices
- [ ] Document key passphrases securely
- [ ] Test recovery process periodically

## Emergency Contacts
- Document who has access to backup locations
- Include instructions for trusted individuals if needed

## Notes
- Keep this document updated with any location changes
- Review and test recovery procedures annually
- Consider geographic distribution of backups