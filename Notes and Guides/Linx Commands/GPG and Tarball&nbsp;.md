---
sticker: lucide//shield
banner: Linx Commands/images/pp.webp
---

# GPG Cheat Sheet

**TABLE OF CONTENTS**
```table-of-contents
```

## Generating Keys
- Generate a full key pair:
  ```bash
  gpg --full-generate-key
  ```
  - Select the key type (RSA and RSA is a good default)
  - Choose the key size (2048 or 4096 bits)
  - Set the key expiration (0 for no expiration)
  - Enter your name, email, and a comment
  - Provide a secure passphrase

- Generate a key pair with a specific algorithm and key size:
  ```bash
  gpg --gen-key --expert
  ```
  - Select the key type (e.g., RSA, DSA, ECDSA, EdDSA)
  - Choose the key size (e.g., 2048, 4096, 3072)
  - Set the key expiration (0 for no expiration)
  - Enter your name, email, and a comment
  - Provide a secure passphrase

> [!TIP]
> `fas:Lightbulb` 💡 Use a strong passphrase to protect your private key. A combination of uppercase and lowercase letters, numbers, and special characters is recommended.

## Listing Keys
- List public keys:
  ```bash
  gpg --list-keys
  ```
- List secret keys:
  ```bash
  gpg --list-secret-keys
  ```
- List keys with fingerprints:
  ```bash
  gpg --list-keys --fingerprint
  ```
- List keys with key IDs:
  ```bash
  gpg --list-keys --keyid-format long
  ```

> [!INFO]
> `fas:InfoCircle` ℹ️ The fingerprint and key ID are unique identifiers for a GPG key. They can be used to verify the authenticity of a key.

## Exporting Keys
- Export public key:
  ```bash
  gpg --export --armor "user_id" > public_key.asc
  ```
- Export secret key:
  ```bash
  gpg --export-secret-keys --armor "user_id" > secret_key.asc
  ```
- Export public key with a specific key ID:
  ```bash
  gpg --export --armor "key_id" > public_key.asc
  ```
- Export secret key with a specific key ID:
  ```bash
  gpg --export-secret-keys --armor "key_id" > secret_key.asc
  ```

> [!WARNING]
> `fas:ExclamationTriangle` ⚠️ Keep your secret key secure and never share it with anyone. If your secret key is compromised, revoke the key immediately.

## Importing Keys
- Import a public key:
  ```bash
  gpg --import public_key.asc
  ```
- Import a secret key:
  ```bash
  gpg --import secret_key.asc
  ```

> [!NOTE]
> `fas:StickyNote` 📝 When importing a secret key, you will be prompted to enter the passphrase associated with the key.

## Trusting Keys
- Trust a key:
  ```bash
  gpg --edit-key "user_id"
  ```
  - Enter `trust` at the prompt
  - Select the trust level (e.g., 1 for unknown, 2 for marginal, 3 for full)
  - Enter `quit` to exit the key editing mode

> [!INFO]
> `fas:InfoCircle` ℹ️ Trusting a key indicates that you have verified the authenticity of the key and trust the owner of the key.

## Encrypting Files
- Encrypt a file for a specific recipient:
  ```bash
  gpg --encrypt --recipient "recipient_email" file.txt
  ```
- Encrypt a file for multiple recipients:
  ```bash
  gpg --encrypt --recipient "recipient1_email" --recipient "recipient2_email" file.txt
  ```
- Encrypt a file with a specific output name:
  ```bash
  gpg --encrypt --recipient "recipient_email" --output encrypted_file.gpg file.txt
  ```

> [!TIP]
> `fas:Lightbulb` 💡 When encrypting a file, only the specified recipients will be able to decrypt and access the contents of the file.

## Decrypting Files
- Decrypt a file:
  ```bash
  gpg --decrypt encrypted_file.gpg
  ```
- Decrypt a file with a specific output name:
  ```bash
  gpg --decrypt --output decrypted_file.txt encrypted_file.gpg
  ```

> [!WARNING]
> `fas:ExclamationTriangle` ⚠️ Be cautious when handling decrypted files, as they may contain sensitive or confidential information. Ensure that you have the necessary legal authority and follow proper procedures when working with such files.

## Signing Files
- Sign a file:
  ```bash
  gpg --sign file.txt
  ```
- Sign a file with a specific key:
  ```bash
  gpg --sign --default-key "key_id" file.txt
  ```
- Create a detached signature:
  ```bash
  gpg --detach-sign file.txt
  ```

> [!NOTE]
> `fas:StickyNote` 📝 Signing a file provides integrity and authenticity, allowing others to verify that the file originated from you and has not been tampered with.

## Verifying Signatures
- Verify a signed file:
  ```bash
  gpg --verify signed_file.txt.gpg
  ```
- Verify a detached signature:
  ```bash
  gpg --verify signature_file.asc file.txt
  ```

> [!SUCCESS]
> `fas:CheckCircle` ✅ If the signature verification succeeds, it indicates that the file is authentic and has not been modified since it was signed.

## Encrypting Text Directly
- Encrypt the text content of a file symmetrically (password-based):
  ```bash
  gpg --symmetric --armor < myfile.txt
  ```
  - Save the encrypted text to a file:
    ```bash
    gpg --symmetric --armor < myfile.txt > myfile.txt.asc
    ```

- Encrypt the text content of a file using a recipient's public key:
  ```bash
  gpg --encrypt --armor -r "recipient@example.com" < myfile.txt
  ```
  - Save the encrypted text to a file:
    ```bash
    gpg --encrypt --armor -r "recipient@example.com" < myfile.txt > myfile.txt.asc
    ```

## Decrypting Encrypted Text
- Decrypt text content encrypted symmetrically:
  ```bash
  gpg --decrypt myfile.txt.asc
  ```

- Decrypt text content encrypted with a recipient's public key:
  ```bash
  gpg --decrypt myfile.txt.asc
  ```

## Signing and Encrypting
- Sign and encrypt a file:
  ```bash
  gpg --sign --encrypt --recipient "recipient@example.com" file.txt
  ```
  - Combine signing and encrypting with ASCII-armored output:
    ```bash
    gpg --armor --sign --encrypt -r "recipient@example.com" file.txt
    ```
  - Decrypt and verify the signed and encrypted file:
    ```bash
    gpg --decrypt file.txt.asc
    ```

> [!INFO]
> `fas:InfoCircle` ℹ️ Signing and encrypting a file ensures both confidentiality and authenticity, providing a higher level of security.

## Managing Keys
- Edit key details:
  ```bash
  gpg --edit-key "user_id"
  ```
  - Enter `adduid` to add a new user ID
  - Enter `

deluid` to delete a user ID
  - Enter `expire` to change the key expiration
  - Enter `passwd` to change the key passphrase
  - Enter `save` to save the changes and exit

- Delete a public key:
  ```bash
  gpg --delete-key "user_id"
  ```
- Delete a secret key:
  ```bash
  gpg --delete-secret-key "user_id"
  ```

> [!CAUTION]
> `fas:Fire` 🔥 Be cautious when deleting keys, especially secret keys, as they cannot be recovered once deleted. Make sure you have a backup of your keys before deleting them.

## Advanced Topics
- Generate a revocation certificate:
  ```bash
  gpg --gen-revoke "user_id"
  ```
- Import a revocation certificate:
  ```bash
  gpg --import revocation_certificate.asc
  ```
- Set key server for key retrieval:
  ```bash
  gpg --keyserver hkp://keyserver.ubuntu.com --send-keys "key_id"
  ```
- Refresh keys from a key server:
  ```bash
  gpg --keyserver hkp://keyserver.ubuntu.com --refresh-keys
  ```

> [!INFO]
> `fas:InfoCircle` ℹ️ A revocation certificate is used to invalidate a key in case it is compromised or no longer needed. Importing a revocation certificate will mark the corresponding key as revoked.

# Tarball Cheat Sheet

**TABLE OF CONTENTS**
- [[#Creating Archives|Creating Archives]]
- [[#Extracting Archives|Extracting Archives]]
- [[#Listing Archive Contents|Listing Archive Contents]]
- [[#Appending Files to Archive|Appending Files to Archive]]
- [[#Excluding Files or Directories|Excluding Files or Directories]]
- [[#Verifying Archive Integrity|Verifying Archive Integrity]]
- [[#Modifying Archive Permissions|Modifying Archive Permissions]]
- [[#Splitting Archives|Splitting Archives]]
- [[#Merging Archives|Merging Archives]]
- [[#Compressing Individual Files|Compressing Individual Files]]
- [[#Decompressing Individual Files|Decompressing Individual Files]]
- [[#Advanced Topics|Advanced Topics]]


## Creating Archives
- Create a tar archive:
  ```bash
  tar -cvf archive.tar file1 file2 directory/
  ```
  - `-c`: create a new archive
  - `-v`: verbosely list files processed
  - `-f`: use archive file

- Create a compressed archive:
  - gzip: `tar -czvf archive.tar.gz file1 file2 directory/`
  - bzip2: `tar -cjvf archive.tar.bz2 file1 file2 directory/`
  - xz: `tar -cJvf archive.tar.xz file1 file2 directory/`

> [!TIP]
> `fas:Lightbulb` 💡 Modern versions of tar with compression tools do not use the `-9` flag for maximum compression level. Instead, use:
> ```bash
> gzip -9 archive.tar
> ```

## Extracting Archives
- Extract an archive:
  ```bash
  tar -xvf archive.tar
  ```
  - `-x`: extract files from an archive

- Extract a compressed archive:
  - gzip: `tar -xzvf archive.tar.gz`
  - bzip2: `tar -xjvf archive.tar.bz2`
  - xz: `tar -xJvf archive.tar.xz`

- Extract an archive to a specific directory:
  ```bash
  tar -xvf archive.tar -C /path/to/directory
  ```

> [!INFO]
> `fas:InfoCircle` ℹ️ When extracting an archive, the original directory structure is preserved by default.

## Listing Archive Contents
- List contents of an archive:
  ```bash
  tar -tvf archive.tar
  ```
  - `-t`: list the contents of an archive

- List contents of a compressed archive:
  - gzip: `tar -tzvf archive.tar.gz`
  - bzip2: `tar -tjvf archive.tar.bz2`
  - xz: `tar -tJvf archive.tar.xz`

> [!TIP]
> `fas:Lightbulb` 💡 Use the `-v` option to display verbose information about the archive contents, such as file permissions, ownership, and timestamps.

## Appending Files to Archive
- Append files to an archive:
  ```bash
  tar -rvf archive.tar file3 file4
  ```
  - `-r`: append files to the end of an archive

- Append files to a compressed archive (requires uncompressing first):
  ```bash
  gunzip archive.tar.gz
  tar -rvf archive.tar file3 file4
  gzip archive.tar
  ```

> [!NOTE]
> `fas:StickyNote` 📝 Appending files to an existing archive does not modify the original archive contents; it adds the specified files to the end of the archive.

## Excluding Files or Directories
- Exclude files or directories:
  ```bash
  tar -cvf archive.tar --exclude="file1" --exclude="directory1/*" .
  ```
- Exclude files based on a pattern:
  ```bash
  tar -cvf archive.tar --exclude="*.txt" --exclude="*.log" .
  ```

> [!TIP]
> `fas:Lightbulb` 💡 Use the `--exclude` option to specify files or directories to be excluded from the archive. Wildcard patterns can be used to match multiple files or directories.

## Verifying Archive Integrity
- Verify the integrity of an archive:
  ```bash
  tar -tvf archive.tar
  ```
- Verify the integrity of a compressed archive:
  - gzip: `tar -tzvf archive.tar.gz`
  - bzip2: `tar -tjvf archive.tar.bz2`
  - xz: `tar -tJvf archive.tar.xz`

> [!SUCCESS]
> `fas:CheckCircle` ✅ If the verification process completes without any errors, it indicates that the archive is intact and has not been corrupted.

## Modifying Archive Permissions
- Set specific permissions for extracted files:
  ```bash
  tar -xvf archive.tar --mode='u=rwx,go=rx'
  ```
- Set ownership for extracted files:
  ```bash
  tar -xvf archive.tar --owner=user --group=group
  ```

> [!NOTE]
> `fas:StickyNote` 📝 Use the `--mode` option to set specific permissions for the extracted files, and the `--owner` and `--group` options to set the ownership of the extracted files.

## Splitting Archives
- Split an archive into multiple smaller archives:
  ```bash
  tar -cvf - file1 file2 directory/ | split -b 1G - archive.tar.
  ```

> [!INFO]
> `fas:InfoCircle` ℹ️ The above command creates multiple smaller archives named `archive.tar.aa`, `archive.tar.ab`, etc., each with a maximum size of 1GB.

## Merging Archives
- Merge multiple archives into a single archive:
  ```bash
  tar -cvf merged_archive.tar archive1.tar archive2.tar
  ```

> [!TIP]
> `fas:Lightbulb` 💡 Merging archives can be useful when you have multiple related archives that you want to combine into a single archive for easier management.

## Compressing Individual Files
- Compress a file with gzip:
  ```bash
  gzip file.txt
  ```
- Compress a file with bzip2:
  ```bash
  bzip2 file.txt
  ```
- Compress a file with xz:
  ```bash
  xz file.txt
  ```

> [!NOTE]
> `fas:StickyNote` 📝 Compressing individual files can be done without creating a tar archive. The compressed file will have the corresponding extension (`.gz`, `.bz2`, or `.xz`).

## Decompressing Individual Files
- Decompress a gzip file:
  ```bash
  gunzip file.txt.gz
  ```
- Decompress a bzip2 file:
  ```bash
  bunzip2 file.txt.bz2
  ```
- Decompress an xz file:
  ```bash
  unxz file.txt.xz
  ```

> [!INFO]
> `fas:InfoCircle` ℹ️ Decompressing individual files restores the original file without the compression extension.

## Advanced Topics
- Create an incremental backup:
  ```bash
  tar -cvf archive.tar --listed-incremental=snapshot.file file1 file2 directory/
  ```
- Update an incremental backup:
  ```bash
  tar -cvf archive.tar --listed-incremental=snapshot.file --level=1 file1 file2 directory/
  ```

> [!TIP]
> `fas:Lightbulb` 💡 Incremental backups allow you to efficiently store multiple versions of files and directories while saving storage space. The `--level` option specifies the incremental backup level, with `0` being the full backup and subsequent levels representing incremental changes.
