# Secret Repository

This repository contains encrypted files and YubiKey-backed SSH keys.

## SSH Key Setup

This repository contains YubiKey-backed SSH keys (encrypted private keys and public keys). Follow the platform-specific instructions below to set them up on a new device.

### Prerequisites
- YubiKey with the private key loaded
- GPG installed and configured
- SSH client

### Windows

1. **Install GPG4Win**: Download from https://gpg4win.org/
2. **Import your GPG key** (if not already done):
   ```cmd
   gpg --import your-gpg-key.asc
   ```
3. **Decrypt the private key**:
   ```cmd
   gpg --decrypt ssh\id_yubi_primary.gpg > %USERPROFILE%\.ssh\id_yubi_primary
   gpg --decrypt ssh\id_yubi_secondary.gpg > %USERPROFILE%\.ssh\id_yubi_secondary
   ```
4. **Copy public keys**:
   ```cmd
   copy ssh\id_yubi_primary.pub %USERPROFILE%\.ssh\
   copy ssh\id_yubi_secondary.pub %USERPROFILE%\.ssh\
   ```
5. **Set proper permissions**:
   ```cmd
   icacls %USERPROFILE%\.ssh\id_yubi_primary /inheritance:r /grant:r %USERNAME%:F
   icacls %USERPROFILE%\.ssh\id_yubi_secondary /inheritance:r /grant:r %USERNAME%:F
   ```
6. **Add to SSH config** (`%USERPROFILE%\.ssh\config`):
   ```
   Host *
       IdentityFile ~/.ssh/id_yubi_primary
       IdentityFile ~/.ssh/id_yubi_secondary
   ```

### Linux

1. **Decrypt the private key**:
   ```bash
   gpg --decrypt ssh/id_yubi_primary.gpg > ~/.ssh/id_yubi_primary
   gpg --decrypt ssh/id_yubi_secondary.gpg > ~/.ssh/id_yubi_secondary
   ```
2. **Copy public keys**:
   ```bash
   cp ssh/id_yubi_primary.pub ~/.ssh/
   cp ssh/id_yubi_secondary.pub ~/.ssh/
   ```
3. **Set proper permissions**:
   ```bash
   chmod 600 ~/.ssh/id_yubi_primary ~/.ssh/id_yubi_secondary
   chmod 644 ~/.ssh/id_yubi_primary.pub ~/.ssh/id_yubi_secondary.pub
   ```
4. **Add to SSH config** (`~/.ssh/config`):
   ```
   Host *
       IdentityFile ~/.ssh/id_yubi_primary
       IdentityFile ~/.ssh/id_yubi_secondary
   ```

### macOS

1. **Install GPG**: `brew install gnupg`
2. **Decrypt the private key**:
   ```bash
   gpg --decrypt ssh/id_yubi_primary.gpg > ~/.ssh/id_yubi_primary
   gpg --decrypt ssh/id_yubi_secondary.gpg > ~/.ssh/id_yubi_secondary
   ```
3. **Copy public keys**:
   ```bash
   cp ssh/id_yubi_primary.pub ~/.ssh/
   cp ssh/id_yubi_secondary.pub ~/.ssh/
   ```
4. **Set proper permissions**:
   ```bash
   chmod 600 ~/.ssh/id_yubi_primary ~/.ssh/id_yubi_secondary
   chmod 644 ~/.ssh/id_yubi_primary.pub ~/.ssh/id_yubi_secondary.pub
   ```
5. **Add to SSH config** (`~/.ssh/config`):
   ```
   Host *
       IdentityFile ~/.ssh/id_yubi_primary
       IdentityFile ~/.ssh/id_yubi_secondary
   ```
6. **Add to keychain** (optional):
   ```bash
   ssh-add --apple-use-keychain ~/.ssh/id_yubi_primary
   ssh-add --apple-use-keychain ~/.ssh/id_yubi_secondary
   ```

### Android (Termux)

1. **Install required packages**:
   ```bash
   pkg update && pkg upgrade
   pkg install gnupg openssh
   ```
2. **Create .ssh directory**:
   ```bash
   mkdir -p ~/.ssh
   ```
3. **Decrypt the private key**:
   ```bash
   gpg --decrypt ssh/id_yubi_primary.gpg > ~/.ssh/id_yubi_primary
   gpg --decrypt ssh/id_yubi_secondary.gpg > ~/.ssh/id_yubi_secondary
   ```
4. **Copy public keys**:
   ```bash
   cp ssh/id_yubi_primary.pub ~/.ssh/
   cp ssh/id_yubi_secondary.pub ~/.ssh/
   ```
5. **Set proper permissions**:
   ```bash
   chmod 600 ~/.ssh/id_yubi_primary ~/.ssh/id_yubi_secondary
   chmod 644 ~/.ssh/id_yubi_primary.pub ~/.ssh/id_yubi_secondary.pub
   ```
6. **Add to SSH config** (`~/.ssh/config`):
   ```
   Host *
       IdentityFile ~/.ssh/id_yubi_primary
       IdentityFile ~/.ssh/id_yubi_secondary
   ```

### Security Notes

- Always verify the GPG signature before decrypting
- Keep your YubiKey with you when using these keys
- The private keys are encrypted and require GPG decryption
- Consider using SSH agent forwarding for remote connections
- Regularly backup your GPG keys and YubiKey configuration

### Troubleshooting

- If GPG decryption fails, ensure your YubiKey is connected and recognized
- For "Permission denied (publickey)" errors, check file permissions
- Use `ssh -v` for verbose output to debug connection issues
- Ensure SSH agent is running: `eval "$(ssh-agent -s)"`