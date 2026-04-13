Jellyfin Media Server: Windows Deployment Guide
This guide covers the installation, library configuration, and local network exposure of a Jellyfin server on a Windows-based home server.

## 1. Prerequisites
OS: Windows 10/11 or Windows Server 2016+.

Hardware: Minimum 4GB RAM. If you plan on Transcoding (converting 4K to 1080p on the fly), a dedicated GPU or an Intel CPU with QuickSync is highly recommended.

Storage: A dedicated drive or folder for your media (Movies, TV Shows, Music).

## 2. Installation
Download: Head to the Jellyfin Windows Downloads page.

Choose Version: * Installer (Recommended): Sets Jellyfin up as a "Tray App."

Service (Advanced): Runs in the background without a user logged in (best for dedicated home servers).

Run the Installer: Follow the prompts. If prompted for "Install Type," the Basic Install is usually sufficient.

## 3. Initial Configuration (The Web Wizard)
Once installed, Jellyfin will open a browser window at http://localhost:8096.

Create Admin: Set a strong username and password.

Add Media Libraries:

Select the content type (e.g., Movies).

Map the folder path (e.g., D:\Media\Movies).

Pro Tip: Enable "Real-time monitoring" so Jellyfin updates the UI the moment you drop a new file into the folder.

Metadata: Choose your preferred language for posters and descriptions.

## 4. Exposing to the Local Network (The Firewall Step)
By default, Windows Firewall will block other devices from seeing your server. You must open Port 8096.

Search for "Windows Defender Firewall with Advanced Security."

Click Inbound Rules > New Rule.

Rule Type: Port.

Protocol/Port: TCP, Specific local ports: 8096.

Action: Allow the connection.

Profile: Ensure Private is checked (keep Public unchecked for security).

Name: "Jellyfin Server."

## 5. Accessing from Other Devices
To connect from your phone, TV, or another laptop, you need your server’s Local IP Address.

Open Command Prompt and type ipconfig.

Look for "IPv4 Address" (usually something like 192.168.1.XX).

On your other device, open the Jellyfin app or browser and enter:
http://192.168.1.XX:8096

## 6. Essential "Tinkerer" Optimizations
Hardware Acceleration: Go to Dashboard > Transcoding. If you have an Intel chip, select Intel QuickSync. This offloads the heavy lifting from your CPU to your GPU, keeping the server responsive.

Static IP: In your router settings, assign a "Static Lease" to your server. This ensures the IP address doesn't change every time the router reboots.

Kiwix/Syncthing Integration: Since you run these, ensure they aren't competing for the same ports. Jellyfin is hungry for bandwidth during high-bitrate streaming; if Syncthing is syncing 50GB of data, your movie might buffer.

Security Note: If you want to access this outside your home (over the internet), do not just port forward 8096 on your router. Use a Reverse Proxy (like Nginx or Caddy) or a VPN (like WireGuard or Tailscale) to tunnel back into your network safely.
