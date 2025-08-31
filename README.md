# 🚀 Wazuh Agent Installer (Multipass)

This project contains a Python automation script
(`setup_wazuh_agent.py`) to provision a new
[Multipass](https://multipass.run) VM, install the **Wazuh Agent**, and
configure it to connect with a Wazuh Manager / Dashboard running on
another VM or host.

------------------------------------------------------------------------

## ⚙️ Prerequisites

-   **Python 3.8+**
-   **Multipass** installed on your host:
    -   [Windows](https://multipass.run/download/windows)
    -   [Linux](https://multipass.run/download/linux)
    -   [macOS](https://multipass.run/download/macos)
-   A running **Wazuh Manager / Dashboard** (in another Multipass VM or
    server).\
-   Open ports on the Wazuh Manager (default: `1514/udp`, `1515/tcp`).

------------------------------------------------------------------------

## ▶️ Usage

1.  Clone or copy this repository.

    ``` bash
    git clone <your-repo-url>
    cd <your-repo-dir>
    ```

2.  Make sure the scripts are executable:

    ``` bash
    chmod +x setup_wazuh_agent.py wazuh-agent-install.sh
    ```

3.  Run the setup:

    ``` bash
    ./setup_wazuh_agent.py
    ```

4.  Follow the prompts:

    -   Enter a **unique VM name** (e.g., `wazuh-agent1`).\
    -   Enter the **Wazuh Server IP** (IP of your Wazuh
        Manager/Dashboard VM).\
    -   The script:
        -   Verifies Multipass is installed.
        -   Checks required ports (`1514`, `1515`).
        -   Launches a Multipass VM.
        -   Transfers `wazuh-agent-install.sh` after replacing `WS_IP`
            with your Manager IP.
        -   Installs and configures the Wazuh Agent.
        -   Drops you into a shell session inside the VM.

------------------------------------------------------------------------

## 🔍 Verification

Inside the agent VM:

``` bash
sudo systemctl status wazuh-agent
```

You should see the agent running and connected to your Wazuh Manager.

Check from the **Wazuh Dashboard** → Agents tab → your agent should
appear as "Active".

------------------------------------------------------------------------

## ⚠️ Notes

-   The script modifies `wazuh-agent-install.sh` **in-place** by
    replacing `WS_IP` with the IP you provide.\
-   If you want to keep a clean copy, back up the original script
    first.\
-   Works on **Linux** and **Windows** (with `multipass.exe`
    detection).\
-   Not recommended to run inside **WSL** (nested virtualization
    issues).

------------------------------------------------------------------------

## 🛠️ Troubleshooting

-   If you see:

        ERROR: (4112): Invalid server address found: 'MANAGER_IP'

    → It means the placeholder wasn't replaced. Re-run
    `setup_wazuh_agent.py` and provide a valid IP.

-   If agent doesn't connect, make sure ports `1514/1515` are open on
    the Manager VM.

------------------------------------------------------------------------

## 📜 License

This project is licensed under the terms of the [LICENSE](LICENSE) file.
