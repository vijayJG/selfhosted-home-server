# Complete Setup Guide

A cost-effective, minimal-hardware guide to building a self-hosted home cloud using TrueNAS SCALE and Nextcloud.

## Prerequisites

- 2 spare HDDs (one for the OS, one for data)
- A PC or server to host TrueNAS
- A USB drive (8GB+) for the installer
- A router with LAN access
- Another device (laptop/phone) to access the dashboard

## Step 1 - Install TrueNAS SCALE

### 1.1 Prepare the Installation Media

1. Download the TrueNAS SCALE ISO from the official site:
   https://www.truenas.com/download-truenas-scale/
2. Flash the ISO to a USB drive using [Rufus](https://rufus.ie/) or [Balena Etcher](https://www.balena.io/etcher/).
3. Plug the USB into your host machine.

### 1.2 Boot and Install

1. Boot from the USB drive (set boot priority in BIOS if needed).
2. Select **Install/Upgrade TrueNAS SCALE**.
3. Choose the HDD you want to install the OS on.
   > **Warning:** This will erase everything on that drive.
4. Set a root password when prompted.
5. Wait for installation to complete, then reboot and remove the USB.

### 1.3 Access the Web Dashboard

After reboot, the console will show a local IP address like:

```
http://192.168.x.xx
```

From another device on the same LAN, open a browser, visit that IP, and log in:

```
Username: root
Password: (the one you set during installation)
```

## Step 2 - Create a Storage Pool

1. Go to **Storage → Pools** and click **Create Pool**.
2. Give it a name (e.g., `NAS_Storage`).
3. Select your second (data) HDD from the available disks.
4. Choose a layout:

   | Layout | Description |
   |--------|-------------|
   | Stripe | No redundancy - all space usable (one disk) |
   | Mirror | Redundancy - two disks, same data mirrored |
   | RAIDZ | Multiple drives with parity-based redundancy |

5. Click **Create → Confirm**.

## Step 3 - Create a Dataset

1. Go to **Storage → Pools → NAS_Storage → Add Dataset**.
2. Name it (e.g., `NextcloudData` or `PersonalFiles`).
3. Keep defaults, or optionally:
   - Set **Share Type:** `SMB`
   - Adjust **Compression** (`LZ4` is a good default)
4. Click **Save**.

## Step 4 - Set Up SMB Share (Windows & LAN Access)

### 4.1 Create the Share

1. Go to **Sharing → Windows Shares (SMB)** and click **Add**.
2. Set **Path** to your dataset (e.g., `/mnt/NAS_Storage/NextcloudData`).
3. Name it (e.g., `NAS_Share`).
4. Optionally enable **Allow guest access** (for testing only).
5. Click **Save** and enable the SMB service when prompted.

### 4.2 Set Permissions

1. Go to **Storage → Pools → NextcloudData → Edit Permissions**.
2. Set **User** to `root` or a dedicated user you've created.
3. Check **Apply User and Group recursively**.
4. Click **Save**.

## Step 5 - Access NAS Storage from Windows

1. Open **File Explorer → This PC → Map Network Drive**.
2. Choose a drive letter (e.g., `Z:`).
3. Enter the path:
   ```
   \\192.168.x.xx\NAS_Share
   ```
4. Enter your username and password if prompted.

You can now read and write files directly as if the NAS were a local drive.

## Step 6 - Install Nextcloud on TrueNAS SCALE

### 6.1 Find Nextcloud in the App Store

1. Go to **Apps → Discover**.
2. Search for **Nextcloud** and click **Install**.

### 6.2 Configure the App

| Setting | Value |
|---------|-------|
| Application Name | `nextcloud` |
| Data Directory | `/mnt/NAS_Storage/NextcloudData` |
| Networking | Static IP recommended (or leave as dynamic) |
| Admin Credentials | Set your Nextcloud username and password |

> **Note:** A static LAN IP keeps the Nextcloud URL predictable. You can reserve an IP for the server in your router's DHCP settings.

Click **Deploy App** and wait for it to start.

### 6.3 Access Nextcloud

Once deployed, TrueNAS will show the app URL. Open it in a browser and log in with your Nextcloud credentials.

## Step 7 - Access from Multiple Devices

### From a PC or Laptop (Browser)

```
http://192.168.x.xx
```

Log in to view, upload, or manage files.

### From a Mobile Device

1. Install the **Nextcloud** app from the [Play Store](https://play.google.com/store/apps/details?id=com.nextcloud.client) or [App Store](https://apps.apple.com/app/nextcloud/id1125420102).
2. Enter the server address:
   ```
   http://192.168.x.xx
   ```
3. Log in with the same Nextcloud credentials.

## Final Architecture

```
                   +----------------------+
                   |    Nextcloud App     |
                   | (on TrueNAS SCALE)   |
                   +----------+-----------+
                              |
             +---------------------------------------+
             | TrueNAS SCALE Dashboard (192.168.x.x) |
             |  NAS_Storage -> Dataset -> SMB Share  |
             +---------------------------------------+
                              |
    --------------------------------------------------------
    |               |               |                      |
[Windows PC]   [Laptop]     [Android Phone]           [Smart TV]
 Map SMB Z:    Browser WebUI  Nextcloud App            LAN Sync
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Can't reach dashboard after reboot | Check the IP shown on the TrueNAS console; it may have changed if using DHCP |
| SMB share not visible on Windows | Ensure the SMB service is running in TrueNAS under **Services** |
| Nextcloud app shows red/error | Check dataset path and permissions in TrueNAS |
| Slow transfer speeds | Use wired ethernet instead of Wi-Fi for best LAN performance |

> **Want remote access?** This setup is LAN-only. To access your server from outside your home network, consider adding [WireGuard VPN](https://www.wireguard.com/) (available as a TrueNAS app) or a reverse proxy with HTTPS.
