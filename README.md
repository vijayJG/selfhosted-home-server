# Self-Hosted Home Server

> Repurposed old hard drives into a private, self-hosted cloud storage system - no subscriptions, no external providers, full control.

![TrueNAS SCALE](https://img.shields.io/badge/TrueNAS-SCALE-blue?logo=truenas&logoColor=white)
![Nextcloud](https://img.shields.io/badge/Nextcloud-LAN-0082C9?logo=nextcloud&logoColor=white)
![SMB](https://img.shields.io/badge/SMB-Windows%20Share-0078D4?logo=windows&logoColor=white)
![License](https://img.shields.io/github/license/vijayJG/selfhosted-home-server)

## Table of Contents

- [Overview](#overview)
- [Hardware Used](#hardware-used)
- [Architecture](#architecture)
- [Features](#features)
- [Screenshots](#screenshots)
- [Setup Guide](#setup-guide)
- [Tools & Technologies](#tools--technologies)
- [Repository Purpose](#repository-purpose)

## Overview

This project documents how I turned unused hard drives into a fully functional **private home cloud** using [TrueNAS SCALE](https://www.truenas.com/) and [Nextcloud](https://nextcloud.com/) - all running locally on my LAN with no dependency on Google Drive, Dropbox, or any external service.

## Hardware Used

| Component | Details |
|-----------|---------|
| OS Drive | Old HDD (repurposed) - TrueNAS SCALE installed here |
| Data Drive | Second unused HDD - used as the NAS storage pool |
| Host Machine | Any PC/server with two HDD slots |
| Network | Local LAN (router + ethernet or Wi-Fi) |

## Architecture

```mermaid
graph TD
    A[Nextcloud App\non TrueNAS SCALE] --> B[TrueNAS SCALE Dashboard\n192.168.x.x]
    B --> C[(NAS_Storage Pool\nDataset - SMB Share)]
    B --> D[Windows PC\nMap SMB Drive Z:]
    B --> E[Laptop\nBrowser WebUI]
    B --> F[Android Phone\nNextcloud App]
    B --> G[Smart TV\nLAN Sync]
```

## Features

- Private cloud - 100% self-hosted, no third-party access
- LAN-speed transfers - no internet bottleneck
- SMB network drive - mounts as a local drive on Windows
- Nextcloud interface - file sync, web browser access, mobile app support
- Multi-device access - PC, laptop, phone, tablet on the same LAN
- Zero recurring cost - runs on repurposed hardware
- Multi-drive pool management via TrueNAS Dashboard

## Screenshots

### TrueNAS Home Dashboard
![TrueNAS Home Dashboard](screenshots/TrueNAS%20Home%20Dashboard.png)

### Datapool Storage Dashboard
![Datapool Storage Dashboard](screenshots/Datapool%20Storage%20Dashboard.png)

### Creating Datasets in a Data Pool
![Creation of Datasets in a datapool](screenshots/Creation%20of%20Datasets%20in%20a%20datapool.png)

### Dataset Mounted to Windows SMB (LAN Access)
![Dataset mounted to SMB](screenshots/A%20dataset%20is%20created%20and%20path%20mounted%20to%20the%20Wndows%20SMB%20so%20we%20can%20connect%20locally%20in%20a%20LAN.png)

### User Created with Admin Access
![Admin user creation](screenshots/An%20user%20is%20created%20and%20given%20full%20Administrative%20access.png)

### NAS Cloud Storage (Locally Accessible)
![NAS cloud storage](screenshots/NAS%20cloud%20storage%20hsoted%20which%20can%20be%20locally%20accessed.png)

### Nextcloud Being Deployed on TrueNAS
![Nextcloud deployment](screenshots/NextCloud%20is%20being%20deployed%20in%20the%20TrueNAS%20home%20server.png)

### Nextcloud Running and Accessible Across Devices
![Nextcloud running](screenshots/NextCloud%20is%20successfully%20running%20in%20the%20server%20and%20can%20be%20accessed%20via%20multiple%20devices%20using%20an%20IP%20and%20access.png)

## Setup Guide

For the full step-by-step installation and configuration walkthrough, see **[SETUP.md](SETUP.md)**.

High-level steps:
1. Install TrueNAS SCALE on an old HDD
2. Create a storage pool with a second HDD
3. Create a dataset and set up an SMB share
4. Map the network drive on Windows
5. Deploy Nextcloud via TrueNAS Apps
6. Access from any LAN-connected device

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| [TrueNAS SCALE](https://www.truenas.com/) | NAS OS and storage management |
| [Nextcloud](https://nextcloud.com/) | Cloud storage UI, file sync, mobile access |
| SMB (Windows Shares) | Network drive mapping on Windows |
| Local LAN | Internal network for access and control |

## Repository Purpose

This repo is a personal documentation of my home server setup. It's useful for anyone who wants to:

- Build a personal NAS from old hardware
- Self-host private cloud storage without a subscription
- Learn TrueNAS SCALE and Nextcloud integration
- Get inspiration for a low-cost, low-power home lab

> **Note:** All access in this setup is LAN-only. For remote access, consider setting up a VPN (e.g., WireGuard) or a reverse proxy with HTTPS.

*Made by [vijayJG](https://github.com/vijayJG)*
