Running a Kiwix server on your local network is the ultimate "homelab" move—it essentially creates a localized version of the internet (Wikipedia, StackOverflow, WikiHow) that stays online even if your ISP goes down.

Since you're on Windows, you have two options: the **GUI (Desktop)** method for ease, or the **kiwix-serve (CLI)** method for a more robust, "server-like" feel.

---

# Kiwix Server: Offline Knowledge Deployment Guide

This guide covers setting up a Kiwix instance to serve `.zim` files to any device on your local network.

## ## 1. Prerequisites
* **ZIM Files:** Download these from the [Kiwix Library](https://library.kiwix.org/). 
    * *Recommendation:* Start with a small one (e.g., "Wikipedia Movie Quotes") before committing 100GB to the full "Wikipedia English + Images" archive.
* **Kiwix Tools:** Download the `kiwix-tools` Windows zip from the [Kiwix Releases](https://github.com/kiwix/kiwix-tools/releases).

---

## ## 2. Option A: The "Quick & Easy" Way (Kiwix Desktop)
If you just want it running now without using the command line:
1.  Open **Kiwix Desktop**.
2.  Add your `.zim` files to the library.
3.  Go to **Settings** (or the Menu) and look for **"Local Server"** or **"Serve on Local Network."**
4.  Toggle it **ON**. It will usually default to port `8080`.

---

## ## 3. Option B: The "Pro" Way (CLI kiwix-serve)
This is better for your home server because it uses fewer resources and can be automated to start on boot.

1.  Extract `kiwix-tools` to a folder (e.g., `C:\Kiwix\`).
2.  Open **Command Prompt** (cmd) and navigate to that folder:
    ```cmd
    cd C:\Kiwix
    ```
3.  Run the server command, pointing it to your `.zim` file:
    ```cmd
    kiwix-serve --port 8080 "C:\Path\To\Your\Wikipedia.zim"
    ```
    *To serve a whole folder of files, use:* `kiwix-serve --port 8080 *.zim`

---

## ## 4. Exposing to the Network (Firewall)
Just like Jellyfin, Windows will block the connection by default.

1.  Open **Windows Defender Firewall with Advanced Security**.
2.  **Inbound Rules** > **New Rule**.
3.  Select **Port** > **TCP** > Specific local ports: `8080`.
4.  **Allow the connection** on **Private** networks.
5.  Name it "Kiwix Server."

---

## ## 5. Accessing the "Offline Internet"
From any phone, tablet, or laptop on your Wi-Fi:
1.  Open a browser.
2.  Enter your server’s IP and the port: 
    `http://192.168.1.XX:8080`
3.  You will see a library interface where you can browse all your downloaded sites.

---

## ## 6. Technical Tips for your Setup
* **The "Invisible" Edge:** Since you're interested in privacy and Tor, Kiwix is the ultimate "low-signature" way to consume information. No one—not your ISP, not Google—knows what you are reading when you browse a local `.zim` file.
* **Automation:** Since you're running a home server on Windows, you can use **Task Scheduler** to run the `kiwix-serve` command at "System Startup" so it’s always available even if you aren't logged in.
* **Storage:** Large ZIM files are "read-intensive." If you're running this off an old laptop HDD, the search function might be slow. If you have a spare SSD, put the ZIM files there for instant search results.
