# SSHLight
`SSHLight` is an auto script to turn on an LED Light when you SSH into a Linux computer and an Executible file to turn it off remotely.

This works using the uhubctl utility which can turn port power on and off from specific USB Hubs.

Compatible USB hubs
===================

For a full list of compatible USB Hubs check: https://github.com/mvp/uhubctl

Install Uhubctl
=========

First, you need to install library libusb-1.0 (version 1.0.12 or later, 1.0.16 or later is recommended):

* Ubuntu: `sudo apt-get install libusb-1.0-0-dev`

> :warning: On Linux, use `sudo` or configure USB permissions as described below!

To list all supported hubs:

    ```console
    uhubctl
    ```

You can control the power on a USB port(s) like this:

    ```console
    uhubctl -a off -p 2
    ```

Usage
=====

