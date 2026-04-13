# Wazuh VirusTotal Threat Intelligence Integration

This project implements an automated threat intelligence enrichment pipeline by integrating the Wazuh SIEM with VirusTotal API.

## Project Overview

The integration enhances **File Integrity Monitoring (FIM)** by automatically cross-referencing detected file hashes against VirusTotal's global malware database. This provides SOC analysts with immediate context regarding the reputation of new or modified files within the environment.

---

## Configuration

Add the following configuration block to the Wazuh Manager `ossec.conf` file:

```xml
<integration>
  <name>virustotal</name>
  <api_key>YOUR_API_KEY</api_key>
  <group>syscheck</group>
  <alert_format>json</alert_format>
</integration>
```
## Security Requirements
- API Key: Replace YOUR_API_KEY with a valid VirusTotal API key.

- Scope: Ensure the VirusTotal module is enabled on the target agents under the syscheck configuration.

## Technical Workflow
- File Detection: The Wazuh agent identifies a file system change (creation or modification) via the Syscheck module.

- Hash Computation: The agent computes cryptographic hashes (MD5, SHA1, SHA256) for the affected file.

- API Query: The Wazuh Manager triggers an integration script that transmits the file hash to the VirusTotal API.

- Data Retrieval: VirusTotal returns a detection report containing:

  - Positive engine detection count.
  
  - Specific malware classifications and categories.

- Alert Enrichment: The intelligence data is merged into the original JSON alert, enabling automated response or advanced filtering.

## Operational Benefits
- Automated Triage: Significantly reduces the time required for initial alert investigation.

- Malware Attribution: Identifies known malicious signatures using 70+ antivirus engines.

- Reduced False Positives: Focuses incident response efforts on confirmed malicious indicators.
