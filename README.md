2.  **(Optional and NOT RECOMMENDED) Install Wazuh Agent on the same VM**

    ``` bash
    sudo apt update
    sudo apt install wazuh-agent
    ```

    Configure agent to send logs to the local manager:

    ``` bash
    sudo nano /var/ossec/etc/ossec.conf
    # Set the address:
    <address>127.0.0.1</address>
    ```

    Then start the agent:

    ``` bash
    sudo systemctl daemon-reload
    sudo systemctl enable --now wazuh-agent
    ```
