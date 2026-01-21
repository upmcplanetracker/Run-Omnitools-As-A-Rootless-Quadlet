Omni-tools Podman User Quadlet
==============================

**Disclaimer:** I am not affiliated with, associated, authorized, endorsed by, or in any way officially connected with the Omni-tools project. This is intended as a drop in replacement but back up important data before trying this.

This guide demonstrates how to deploy **Omni-tools** as a **User (Rootless) Quadlet** on Ubuntu 25.10. Omni-tools provides a suite of web-based utilities managed via Nginx.

**Port Configuration Note:** This Quadlet uses `Network=host`. This means the application will bind to whichever port is specified in your `default.conf` file. **You must edit `default.conf` to use an open port** (e.g., 8999) that does not conflict with existing services on your system.

System Information
------------------

*   **Tested Environment:** Ubuntu 25.10
*   **Podman Version:** 5.4.x
*   **Type:** User-level Quadlet (Rootless)

1\. The Quadlet File (omni-tools.container)
-------------------------------------------

This configuration mounts your local Nginx configuration and `.htpasswd` files. Ensure these exist in `~/omnitools/` before starting.

2\. Installation & Setup
------------------------

### Step A: Prepare Configuration Files

Create the directory and ensure your custom Nginx config is ready:

`mkdir -p ~/omnitools`

Make sure to check the `listen` directive in `~/omnitools/default.conf` and change it to an available port.

### Step B: Move the Quadlet

Place the file in your user-level systemd folder:

`mkdir -p ~/.config/containers/systemd`
`mv omni-tools.container ~/.config/containers/systemd/`

### Step C: Enable Linger

`sudo loginctl enable-linger $USER`

3\. Activation
--------------

`systemctl --user daemon-reload`
`systemctl --user start omnitools`
`systemctl --user enable omnitools`

4\. Verification
----------------

### Check Service Status

`systemctl --user status omnitools`

### Check Listening Port

To confirm which port the container is using on the host network:

`ss -tulpn | grep nginx`

5\. Maintenance
---------------

Update Omni-tools automatically:

`podman auto-update`
