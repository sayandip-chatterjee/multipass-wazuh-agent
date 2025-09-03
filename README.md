# 🚀 Wazuh Agent Installer (Multipass)

This project contains a Python automation script
(`setup_wazuh_agent.py`) to provision a new
[Multipass](https://multipass.run) VM, install the **Wazuh Agent**, and
configure it to connect with a Wazuh Manager / Dashboard running on
another VM or host.

#### ❌ This is NOT WINDOWS AGENT !!!
#### ✅ This is ONLY LINUX AGENT installed on WINDOWS / LINUX via MULTIPASS !!!
------------------------------------------------------------------------

## 📑 Table of Contents

- [📡 Architecture](#-architecture)
- [🛠️ Requirements](#️-requirements)
- [📦 Installation & Usage](#-installation--usage)
- [🔍 Verificationn](#-verification)
- [⚠️ Notes](#-notes)
- [🛠️ Troubleshooting](#-troubleshooting)
- [🧹 Cleanup](#-cleanup)

------------------------------------------------------------------------

## 📡 Architecture

    +-------------------+             +-----------------------+
    |   Wazuh Agent VM  |  <------->  |  Wazuh Manager /      |
    | (Provisioned via  |   1514/1515 |  Dashboard VM/agent  |
    |  Multipass)       |             |                       |
    +-------------------+             +-----------------------+

-   **Agent VM** is created by this script.\
-   **Manager VM** should already be running Wazuh Dashboard.\
-   Communication happens on ports `1514/udp` and `1515/tcp`.

------------------------------------------------------------------------

## 🛠️ Requirements

- OS : Ubuntu (tested on **22.04+**) _or_ Windows (tested on **Windows11**)
- Interpreter/Runtime : **Python 3.8+** (MUST be installed in the system)
- A running **Wazuh Manager / Dashboard** (in another Multipass VM or
    agent).
- Open ports on the Wazuh Manager (default: `1514/udp`, `1515/tcp`).
- Ensure that the wazuh-server and wazuh-agent versions are same.(Using 4.12 in this repo) 

------------------------------------------------------------------------

## 📦 Installation & Usage

1.  Clone or copy this repository.

    [LINUX] Clone the repository and run the setup script:
    ```bash
    git clone https://github.com/sayandip-chatterjee/multipass-wazuh-agent.git
    cd multipass-wazuh-agent/
    python3 setup_wazuh_agent.py
    ```
    
    [WINDOWS] Ensure all the steps are done as mentioed:
    ```bash
    - In the Windows machine BIOS setup, make sure that virtualization is turned on
    - Install git bash - https://git-scm.com/downloads/win and close the git bash window, do not clone yet.
    - Install python3.8 from Microsoft Store
    - Go to Windows Features from the Start Menu -> Search and make sure You enable the
      "HyperV", "Virtual Machine Platform", and the "Windows Hypervisor Platform" to run the VM.
    - Restart the machine.
    - Open powershell (NOT AS Administrator)
    - git clone https://github.com/sayandip-chatterjee/multipass-wazuh-agent.git
    - cd multipass-wazuh-agent/
    - python3 setup_wazuh_agent.py
    ```

2.  Follow the prompts:

    -   Enter a **unique VM name** (e.g., `wazuh-agent1`).
    -   Enter the **Wazuh agent IP** (IP of your Wazuh Manager/Dashboard VM).
    -   The script:
        -   Verifies Multipass is installed.
        -   Checks required ports (`1514`, `1515`).
        -   Launches a Multipass VM.
        -   Transfers `wazuh-agent-install.sh` after replacing `WS_IP` with your Manager IP.
        -   Installs and configures the Wazuh Agent.
        -   Drops you into a shell session inside the VM.

------------------------------------------------------------------------

## 🔍 Verification

Inside the agent VM:

``` bash
sudo systemctl status wazuh-agent
```

You should see the agent running and connected to your Wazuh Manager.

Check from the **Wazuh Dashboard** → Agents tab → your agent should appear as "Active".

------------------------------------------------------------------------

## ⚠️ Notes

-   The script modifies `wazuh-agent-install.sh` **in-place** by replacing `WS_IP` with the IP you provide.
-   If you want to keep a clean copy, back up the original script first.
-   Works on **Linux** and **Windows** (with `multipass.exe` detection).
-   Not recommended to run inside **WSL** (nested virtualization issues).

------------------------------------------------------------------------

## 🛠️ Troubleshooting

-   If you see:

        ERROR: (4112): Invalid agent address found: 'MANAGER_IP'

    → It means the placeholder wasn't replaced. Re-run `setup_wazuh_agent.py` and provide a valid IP.
    
    → Investigate the issue of IP by `sudo cat /var/ossec/etc/ossec.conf | grep -A3 '<agent>'`
    
    → Verify whether the IP is replaced correctly or IP is valid or not!

    → Check the logs `sudo tail -f /var/ossec/logs/ossec.log`
    
-   If agent doesn't connect, make sure ports `1514/1515` are open on the Manager VM or check for possible firewall issues.

------------------------------------------------------------------------

## 🧹 Cleanup

```bash
multipass list
multipass delete <NAME>
multipass purge
```

In case you have accidentally deleted and not purged yet
```
multipass list
multipass recover <NAME>
```
