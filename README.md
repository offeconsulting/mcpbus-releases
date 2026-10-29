# mcpbus

A local Windows gateway that controls what AI agents can do in SQL Server and PostgreSQL.

Website and documentation: [mcpbus.sh](https://mcpbus.sh) · [Docs](https://mcpbus.sh/docs)

This repository holds the releases of mcpbus: the signed `mcpbus.exe`, the installer script `install.ps1` and a `SHA256SUMS.txt` for each release.

## Install

In PowerShell:

```powershell
irm https://mcpbus.sh/install.ps1 | iex
```

`mcpbus.sh/install.ps1` serves the `install.ps1` of the latest release. To read it before running it, download it from the [latest release](https://github.com/offeconsulting/mcpbus-releases/releases/latest) and check its hash against `SHA256SUMS.txt` the same way as for the exe below.

What `install.ps1` does, step by step:

1. Asks the GitHub API for the latest release of this repository.
2. Downloads `mcpbus.exe` and `SHA256SUMS.txt` from that release, from `github.com/offeconsulting/mcpbus-releases`.
3. Verifies the download before using it: the SHA-256 hash must match the one in `SHA256SUMS.txt`, and the file must carry a valid Authenticode signature from **Offe Consulting AB**. On any mismatch it stops, and an installed mcpbus is left untouched.
4. Stops a running mcpbus from that location and places the exe at `%LOCALAPPDATA%\mcpbus\bin\mcpbus.exe`.
5. Looks for Claude Code, Claude Desktop and VS Code, and stops if none is installed.
6. Runs `mcpbus setup`, which:
   - registers mcpbus with the clients it found;
   - pre-approves the mcpbus tools in Claude Code (`%USERPROFILE%\.claude\settings.json`);
   - adds `%LOCALAPPDATA%\mcpbus\bin` to your user PATH;
   - creates `%LOCALAPPDATA%\mcpbus\` with the key used for hashing values.
7. Offers to open the local config UI.

Everything stays in your user profile; no administrator rights are needed. `mcpbus uninstall` removes the registrations, the Claude Code permission and the PATH entry.

## Manual install with verification

1. Download `mcpbus.exe` and `SHA256SUMS.txt` from the [latest release](https://github.com/offeconsulting/mcpbus-releases/releases/latest).
2. In PowerShell, in the download folder, compare the hash:

   ```powershell
   $expected = (@(Get-Content .\SHA256SUMS.txt) -match 'mcpbus\.exe$' -split '\s+')[0]
   $actual   = (Get-FileHash .\mcpbus.exe -Algorithm SHA256).Hash
   if ($actual -eq $expected) { 'OK' } else { 'MISMATCH - do not run this file' }
   ```

3. Check the signature — `Status` must be `Valid` and the signer `Offe Consulting AB`:

   ```powershell
   Get-AuthenticodeSignature .\mcpbus.exe | Format-List Status, SignerCertificate
   ```

4. Move `mcpbus.exe` to a permanent folder and run `.\mcpbus.exe setup` from there.

## Releases

Each release contains `mcpbus.exe` (signed), `install.ps1` and `SHA256SUMS.txt`. The release notes list the changes and the SHA-256 of every file.

## Support

Questions and problems: [mcpbus.sh](https://mcpbus.sh) or hello@mcpbus.sh. Security issues: see [SECURITY.md](SECURITY.md).
