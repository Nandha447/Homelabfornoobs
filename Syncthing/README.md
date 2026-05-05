# Syncthing: Windows Deployment Guide
This guide covers the installation, local backup configuration, and redundant storage setup for Syncthing on a Windows-based server environment.

## 1. Prerequisites
**OS:** Windows 10/11 or Windows Server 2016+.

**Permissions:** Administrator privileges (required for firewall rules and service installation).

**Storage:** A dedicated internal or external drive for redundant backups. Avoid using mapped network drives (SMB) as Syncthing performs best with direct filesystem access.

## 2. Installation
**Download:** Grab the latest Windows release from the Syncthing Downloads page.

**Choose Version:**
* **SyncTrayzor (Recommended):** Provides a Windows-native wrapper, system tray integration, and auto-start capabilities without manual scripting.
* **Base Binary (Advanced):** For headless servers, run the standalone `syncthing.exe`. Use **NSSM** (Non-Sucking Service Manager) to wrap it as a Windows Service so it runs without a user session.

**Run the Setup:** Launch the installer. If using SyncTrayzor, it will automatically handle the initial startup of the Syncthing core.

## 3. Initial Configuration (The Web GUI)
By default, the management interface is accessible at `http://127.0.0.1:8384`.

**Set Credentials:** Immediately go to **Actions > Settings > GUI** and set a Username and Password.

**Add Backup Folders:**
* Click **Add Folder**.
* Map your source folder (e.g., `C:\Users\Admin\Documents`).
* Map your destination/redundant folder (e.g., `E:\Syncthing_Backup`).

**Pro Tip:** Enable "File Versioning" (Trash Can or Staggered) on your backup drive. This creates a "time machine" effect, protecting you against accidental deletions or ransomware by moving changed files to a hidden `.stversions` folder.

## 4. Exposing to the Local Network (The Firewall Step)
To sync between multiple computers on your network, Windows Firewall must allow specific traffic.
1.  Search for "Windows Defender Firewall with Advanced Security."
2.  **Inbound Rule 1 (Data):** New Rule > Port > TCP > 22000. Allow the connection.
3.  **Inbound Rule 2 (Discovery):** New Rule > Port > UDP > 21027. Allow the connection.
4.  **Profile:** Ensure "Private" is checked to keep the service invisible on public Wi-Fi.
5.  **Name:** "Syncthing Traffic."

## 5. Setting Up Redundancy (Peer-to-Peer)
To create a truly redundant system, connect your server to a second device (laptop or another server).
* On the second device, find its **Device ID** (Actions > Show ID).
* On your main server, click **Add Remote Device** and paste that ID.
* Under the **Sharing** tab for your backup folder, check the box for the new device.
* The second device will receive a prompt to "Accept" the folder and choose its local storage path.

## 6. Essential "Tinkerer" Optimizations
**Folder Type:** If your server is purely for backups, set the Folder Type to **"Receive Only"**. This ensures that changes made to the backup files by mistake won't sync back and overwrite your original data.

**Scan Interval:** Increase the "Full Scan" interval to 3600 seconds (1 hour) if you have millions of files; this reduces disk thrashing. Syncthing still uses "Watch for Changes" to catch live edits.

**Database Location:** For high-speed performance, ensure the Syncthing database (found in `%LocalAppData%\Syncthing`) is located on an SSD, even if the actual data is on a slow HDD.

**Security Note:** Syncthing uses certificate-based authentication and TLS for all transfers. If you want to sync across the internet without a VPN, Syncthing handles this via "Relay Servers" automatically, keeping your data encrypted end-to-end.
