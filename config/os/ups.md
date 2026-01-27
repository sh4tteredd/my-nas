# UPS Configuration

This page is dedicated to the UPS setup, mostly because getting it to work properly took way more time than expected.

## UPS Hardware

- **Model:** Legrand LG-310180
- **Power Rating:** 600 VA
- **Connection:** USB

## Service Configuration

TrueNAS UPS service (just a NUT config GUI)

- **Driver:** `nutdrv_qx`
- **Mode:** Master
- **Port:** auto

### Driver Parameters (`ups.conf`)

The following parameters were required for correct detection and operation of the UPS:

```conf
subdriver = cypress
vendorid = 0665
productid = 5161
