Here is an example **README.md** file for the project using your `auto_restart.service` systemd unit:

---

# **Auto-Restart Service**
🙊 Sometimes, a service may hang or unexpectedly terminate, and you need to restart it automatically. Instead of using cron jobs or ad-hoc scripts, leverage systemd with its powerful automatic recovery capabilities.

🚀This project sets up an automated recovery for a script that is expected to run continuously. It ensures that the specified script (`/usr/local/bin/important_script.sh`) is restarted automatically in case it crashes, hangs, or stops unexpectedly. The service uses `systemd` to manage the process.

## **Features**
- **Automatic Restart**: The service will automatically restart the script if it fails or stops.
- **Configurable Restart Delay**: The script will wait 5 seconds before restarting (`RestartSec=5s`).
- **Max Runtime Limit**: The script is allowed to run for a maximum of 10 minutes (600 seconds). If it exceeds this time, it will be forcibly terminated.
- **Graceful Termination**: The script is killed as a process if it exceeds the runtime limit (`KillMode=process`).
- **Startup Dependencies**: The script will start after the network is ready (`After=network.target`).

---

## **Installation**

1. **Create the systemd service unit file**  
   Create a new systemd unit file:
   ```bash
   sudo nano /etc/systemd/system/auto_restart.service
   ```
   Paste the following content into the file:
   ```ini
   [Unit]
   Description=My Script Auto-Restart
   After=network.target

   [Service]
   ExecStart=/usr/local/bin/important_script.sh
   Restart=always
   RestartSec=5s
   RuntimeMaxSec=600
   KillMode=process

   [Install]
   WantedBy=multi-user.target
   ```

2. **Make the script executable**  
   Ensure that the script `/usr/local/bin/important_script.sh` is executable:
   ```bash
   sudo chmod +x /usr/local/bin/important_script.sh
   ```

3. **Reload systemd**  
   Reload the systemd daemon to pick up the new unit file:
   ```bash
   sudo systemctl daemon-reload
   ```

4. **Enable and start the service**  
   Enable the service to start on boot:
   ```bash
   sudo systemctl enable auto_restart.service
   ```
   Start the service immediately:
   ```bash
   sudo systemctl start auto_restart.service
   ```

5. **Verify the service status**  
   To check the status of the service, use:
   ```bash
   systemctl status auto_restart.service
   ```

---

## **Configuration**

- `ExecStart=/usr/local/bin/important_script.sh`: Path to the script that should be run.
- `Restart=always`: Ensures that the script will be restarted if it crashes or stops unexpectedly.
- `RestartSec=5s`: Wait time (in seconds) before restarting the script.
- `RuntimeMaxSec=600`: Maximum runtime in seconds before the script is killed if it exceeds this time limit.
- `KillMode=process`: If the service exceeds the time limit, the script process will be terminated.

---

## **Troubleshooting**

- To view logs for the service, use:
  ```bash
  journalctl -u auto_restart.service
  ```

- To stop the service:
  ```bash
  sudo systemctl stop auto_restart.service
  ```

- To disable the service from starting at boot:
  ```bash
  sudo systemctl disable auto_restart.service
  ```

---

✳️ Now your script will automatically restart even if it hangs!

---

