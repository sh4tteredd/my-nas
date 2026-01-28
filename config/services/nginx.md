# Nginx / Reverse Proxy

This page describes how I set up Nginx Proxy Manager (NPM) to handle reverse proxying and SSL for my TrueNAS services.

## Setup Overview

I followed these guides to configure Nginx Proxy Manager, since with the OOTB configuration, the app will never start:

- [TrueNAS Forum: Nginx Proxy Manager Configuration Tips](https://forums.truenas.com/t/nginx-proxy-manager-configuration-tips-especially-if-you-are-stuck-at-deploying/14653)
- [FamilyBrown Wiki: Configure Apps / Other / NPM](https://wiki.familybrown.org/en/fester/configure-apps/other/npm)

## SSL Certificates

- I actively use the SSL certificates generated and automatically renewed by TrueNAS for my domain.  
- These certificates cover the main domain and subdomains such as `immich.example.com`.  
- This ensures secure HTTPS access to all my self-hosted services behind the reverse proxy.

## Notes

- The guides were extremely helpful, otherwise I wouldn't be able to deploy NPM in TrueNAS.
- Certificates integration with TrueNAS keeps renewal and management simple and automated.
