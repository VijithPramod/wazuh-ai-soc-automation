# Wazuh Webhook Integration

This configuration enables the Wazuh Manager to forward specific security alerts to an external n8n webhook for automated AI analysis and Telegram notifications.

## Configuration

Add the following block to your Wazuh Manager configuration file located at `/var/ossec/etc/ossec.conf`:

```xml
<integration>
  <name>webhook</name>
  <hook_url>http://YOUR_N8N_IP:5678/webhook/wazuh-alert</hook_url>
  <level>7</level>
  <alert_format>json</alert_format>
</integration>
```
## Parameters Explanation
- name: Specifies the integration type (webhook).

- hook_url: The destination URL provided by your n8n Webhook node.

- level: Only alerts with severity Level 7 or higher will be forwarded. This prevents the automation pipeline from being overwhelmed by low-priority logs.

- alert_format: Set to json to ensure the data is structured for AI processing.

## Prerequisites
- n8n Webhook Node: Ensure your n8n workflow is active and the Webhook URL matches the hook_url above.

- Network Connectivity: The Wazuh Manager must have outbound HTTP/HTTPS access to the n8n server IP.

- Manager Restart: After saving the changes to ossec.conf, restart the Wazuh manager service:

Bash
```
systemctl restart wazuh-manager
```
## Security Note
Do not expose your internal n8n IP or Webhook URL in public repositories. Use environment variables or restricted firewall rules to secure the endpoint.
