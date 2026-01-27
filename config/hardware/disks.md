# Storage Configuration

This document describes the disk layout and storage configuration of my DXP2800 running TrueNAS.

## Disk Setup

### HDDs (Data Pool)

- **2 × WD Red Pro 4TB (7200 RPM)**
  - Configuration: **Mirror (ZFS, obviously, one of the main reason that let me choose TrueNAS)**
  - Purpose: Main data storage
  - Notes:
    - Chosen for reliability and performance
    - Mirror setup provides redundancy in case of a single disk failure

### SSD (System & Apps)

- **1 × Lexar NM620 256GB NVMe SSD (just a random one from Amazon)**
  - Purpose:
    - TrueNAS OS
    - Applications (Apps)
  - Configuration:
    - The SSD is split during installation to host both the OS and the Apps pool

  - Guide followed:  
    https://www.reddit.com/r/truenas/comments/lgf75w/scalehowto_split_ssd_during_installation/

## ⚠️ Important Notice

> **This setup is NOT officially supported by TrueNAS.**

Splitting a single SSD to host both the operating system and the Apps pool is an **unsupported procedure**.  
While it works in practice and helps save a drive bay, it may cause issues during upgrades or system recovery.

**I do NOT recommend this configuration** unless:
- You understand the risks
- You are comfortable with recovery and reinstall procedures
- You accept potential breakage during updates

If you want a supported setup, use:
- A dedicated boot device for the OS
- A separate SSD or pool for Apps
