# ESET Integration with Wazuh

1. Create ESET Connect API user account and get API key. - https://help.eset.com/eset_connect/en-US/create_api_user_account.html
- Create a user account in the ESET Connect API with the necessary permissions to access threat detection events.
- Authorize API user with swagger https://help.eset.com/eset_connect/en-US/work_with_swagger.html (swagger https://help.eset.com/eset_connect/en-US/use_api_with_swagger.html#swagger_regions)
- Copy the API key generated (`access_token`) for the user account, as you will need it to authenticate requests in the integration script.
- On the top Select a definition, chome some for example User Management. Click on Authorize button and enter the API key in the format `Bearer <access_token>`.

2. Copy `eset_rules.xml` to `/var/ossec/etc/rules/eset_rules.xml`
3. Create `eset_integration.log` file int the `/var/log/` directory:
  ```bash
  touch /var/log/eset_integration.log
  ```
4. Edit the `/var/ossec/etc/ossec.conf` file and append the settings below to configure Wazuh to monitor the ESET log file `/var/log/eset_integration.log`:
```xml
<ossec_config>

<!--Configuration of other local files -->

  <localfile>
    <log_format>json</log_format>
    <location>/var/log/eset_integration.log</location>
  </localfile>

</ossec_config>
```

5. Restart the Wazuh manager service to apply the changes:
```bash
systemctl restart wazuh-manager
```
6. Create a new directory eset_integration in the /opt directory:
```bash
mkdir /opt/eset_integration
```
7. Create an empty `/opt/eset_integration/last_detection_time.yml` file to store and track the time of occurrence of the last events. This is to ensure the script always pulls the most recent events since the last pull.
```bash
touch  /opt/eset_integration/last_detection_time.yml
```
8. Create an `eset_integration.py` script in the `/opt/eset_integration` directory.
This script periodically retrieves threat detection events, wraps each in a standardized JSON format, and appends them to the `/var/log/eset_integration.log` file.
>The `INTERVAL` is set to 300 seconds. You can set it to any value suitable for your requirements
9. Install the Python modules required to run the script:
```bash
pip install requests pyyaml python-dotenv python-dateutil tzlocal
```
10. Create a `.env` file in the `/opt/eset_integration` directory to store the integration variables:
Replace:

- `<USERNAME_INTEGRATION>` with the ESET Connect API user login username or email address.
- `<PASSWORD_INTEGRATION>` with the ESET Connect API user password.
- `<INSTANCE_REGION>` with the location of your ESET PROTECT, ESET Inspect, or ESET Cloud Office Security instance. The allowed options are ca, de, eu, jpn, us.

11. Run the following command to make the `/opt/eset_integration/eset_integration.py` script executable:
```bash
chmod +x /opt/eset_integration/eset_integration.py
```

12. `Create the /etc/systemd/system/eset_integration.service` SystemD service file to run the integration script as a daemon
> The file `/var/log/eset_integration.err.log` stores the service logs and can be used for troubleshooting purposes.

Start and enable the service to execute the script on startup:
```bash
systemctl daemon-reload
systemctl enable eset_integration.service
systemctl start eset_integration.service
```

## Summary
| Component              | Location                                     |
|------------------------|----------------------------------------------|
| Python Log Collector   | `/opt/eset/eset_logcollector.py`               |
| Systemd Service        | `/etc/systemd/system/eset_integration.service`     |
| Wazuh Custom Rules     | `/var/ossec/etc/rules/eset_rules.xml`   |
| Output Log Monitored   | `/var/log/eset-integration.log`                |


# Sources
- https://wazuh.com/blog/integrating-eset-protect-hub-with-wazuh/
- https://help.eset.com/eset_connect/en-US/create_api_user_account.html
- https://raw.githubusercontent.com/eset/ESET-Integration-Wazuh/69ec85343541f1f8d435028a0120ab49066f0826/eset_local_rules.xml
- https://github.com/bayusky/wazuh-custom-rules-and-decoders/tree/main/ESET-integration