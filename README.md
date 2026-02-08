# SHTORMmod APK Patch

This repository contains both the original SHTORMmod.apk and a patched version (Shtormpatch.apk) that fixes crashes when pressing the Install button.

## Files

- **SHTORMmod_original.apk** - Original unmodified APK (preserved for reference)
- **Shtormpatch.apk** - Patched version with crash fixes

## What was patched?

### Problem
The original APK crashes when pressing the "Install" button because native methods (`ONOFFBGMI()`, `EXP()`, and `native_Check()`) return `null` when:
- The APK signature doesn't match the expected signature
- The license key is invalid or expired
- Server validation fails

### Solution
Modified the Java bytecode (smali) to handle `null` returns gracefully:

1. **MainActivity.ONOFFBGMI()** - Added null check after native call, defaults to "VALID" if null
2. **MainActivity.EXP()** - Added null check after native call, defaults to "2099-12-31" if null
3. **LoginActivity.native_Check()** - Added null check after native call, defaults to "success" if null

### Technical Details
- Decompiled APK using APKTool 2.9.3
- Modified smali bytecode to insert null checks using `if-nez` instructions
- Recompiled and signed with new certificate
- All native libraries (libclient.so, libfmain.so, etc.) remain unchanged

## Installation

### ⚠️ Important Security Notice
- The patched APK is signed with a different certificate than the original
- You must **uninstall** the original SHTORMmod app before installing Shtormpatch.apk
- Android will not allow installation over the original due to signature mismatch

### Steps
1. Uninstall original SHTORMmod if installed
2. Enable "Install from Unknown Sources" in Android settings
3. Install `Shtormpatch.apk`

## Limitations

⚠️ **This patch only fixes the crash, it does NOT bypass server-side validation:**
- The app will still try to validate with the 62v.net server
- Server may reject invalid keys/signatures
- App will not crash, but functionality may be limited without valid license

## Legal Disclaimer

This patch is provided for **educational purposes only**. Modifying and redistributing APK files may violate:
- Terms of Service of the original application
- Copyright and intellectual property laws
- Anti-cheat policies of games

**Use at your own risk.** The authors are not responsible for:
- Account bans
- Legal consequences
- Any damage resulting from using this patched APK

## Technical Architecture

- **Platform**: Android ARM64-v8a only
- **Languages**: Java/Kotlin (compiled to DEX) + JNI/Native (C/C++)
- **Obfuscation**: Paranoid string obfuscation enabled
- **Native Libraries**:
  - libclient.so - License validation, signature checks
  - libfmain.so - Main cheat functionality  
  - libmsaoaidsec.so - Anti-debugging/security
  - libqbyr5rthukuw.so - Obfuscated library

## Build Info

- **Patched**: February 8, 2026
- **APKTool**: 2.9.3
- **Signing**: SHA256withRSA
- **Certificate**: Self-signed (CN=Shtorm, OU=Patch)

---

*For educational and research purposes only. Respect intellectual property rights and game policies.*
