# FileSign
Code Signing Tool

<img width="576" height="584" alt="Screenshot 2025-09-14 110233" src="https://github.com/user-attachments/assets/297cf7dc-8c95-4bde-9ee4-5c8bb560bd1c" />

# FileSign User Guide

`FileSign.exe` is a simple tool to digitally sign `.exe` and `.dll` files using a code signing certificate. This guide explains how to use the program to sign files.

## What You Need
- **FileSign.exe**: The main program file.
- **Settings.txt**: A configuration file with default settings (included with the program).
- A valid code signing certificate (e.g., `C:\MyCertificate\CodeSignCert.pfx`).
- The folder containing `.exe` and `.dll` files to sign (e.g., `C:\MyProjects\files`).
- Windows SDK installed, which provides `signtool.exe` (usually at `C:\Program Files (x86)\Windows Kits\10\bin\10.0.22621.0\x64\signtool.exe`).
- Write permissions in the folder where `FileSign.exe` runs to save `Settings.txt`.

## Default Settings
The program uses a file named `Settings.txt` to load default settings. The default content of `Settings.txt` is:
```
C:\MyCertificate\CodeSignCert.pfx
yourpfxpassword
C:\MyProjects\files
C:\Program Files (x86)\Windows Kits\10\bin\10.0.22621.0\x64\signtool.exe
```
These settings define the certificate file, password, folder to sign, and `signtool.exe` path. You can change these settings in the program.

## How to Use FileSign
1. **Run the Program:**
   - Double-click `FileSign.exe` to open the program. If it doesn’t work, right-click and select "Run as Administrator."
2. **Check or Update Settings:**
   - The program loads default settings from `Settings.txt` (if available) or uses built-in defaults.
   - You’ll see four fields:
     - **Certificate File**: Path to your `.pfx` certificate file (e.g., `C:\MyCertificate\CodeSignCert.pfx`).
     - **Password**: Password for the certificate (e.g., `yourpfxpassword`).
     - **Folder to Sign**: Folder containing `.exe` and `.dll` files (e.g., `C:\MyProjects\files`).
     - **signtool.exe Path**: Path to `signtool.exe` (e.g., `C:\Program Files (x86)\Windows Kits\10\bin\10.0.22621.0\x64\signtool.exe`).
   - To change any setting, type a new value or click the **Browse** button next to each field to select a file or folder.
3. **Save New Default Settings (Optional):**
   - If you change any settings and want to save them for future use, click **Save Default Values**.
   - A pop-up message will confirm that the settings were saved to `Settings.txt` or show an error if saving fails.
4. **Sign Files:**
   - Click **Sign Files** to start signing all `.exe` and `.dll` files in the specified folder.
   - The text area at the bottom shows the list of files found, signing progress, and verification results.
   - When the process finishes, a pop-up message will show:
     - "All files signed successfully" if everything worked.
     - A list of any files that failed to sign, along with error details.
5. **Check Signed Files:**
   - Go to the folder (e.g., `C:\MyProjects\files`), right-click on a signed file, select **Properties**, and check the **Digital Signatures** tab to confirm the signature.

## Troubleshooting
- **Program doesn’t open or save settings:**
   - Run `FileSign.exe` as Administrator (right-click > Run as Administrator).
   - Ensure the folder containing `FileSign.exe` allows writing to save `Settings.txt`.
- **Certificate or folder not found:**
   - Check that the certificate file (`.pfx`) and the folder with `.exe` and `.dll` files exist at the paths entered.
- **signtool.exe not found:**
   - Ensure Windows SDK is installed, and the path to `signtool.exe` is correct. You can select it using the **Browse** button.
- **Incorrect password:**
   - Verify the certificate password in the **Password** field.
- **Internet connection issues:**
   - The program uses an online timestamp server. If you’re offline, contact the administrator for a version without timestamping.
- **Signed files show trust warnings:**
   - To trust the certificate, run this PowerShell command as Administrator:
     ```powershell
     $cert = Get-PfxCertificate -FilePath "C:\MyCertificate\CodeSignCert.pfx"
     Move-Item -Path $cert.PSPath -Destination "Cert:\CurrentUser\TrustedPublisher"
     ```
     Enter the certificate password (`yourpfxpassword`) when prompted.

## Notes
- Ensure the paths in `Settings.txt` match your system’s setup (certificate, folder, and `signtool.exe`).
- The program requires an internet connection for timestamping signed files. Contact the administrator if you need an offline version.
- Keep `Settings.txt` secure, as it contains the certificate password in plain text.
