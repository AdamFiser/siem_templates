# SIEM Templates for Wazuh and Similar Systems

This repository contains **templates, configurations, and rules** for the [Wazuh](https://wazuh.com) SIEM platform and similar systems.  
The goal is to provide the community with a structured collection of **ready-to-use** configurations that make deployment, management, and security event detection easier.

---

## 📂 Repository Structure
- `rules/` – Custom and modified rules for incident detection.
- `decoders/` – Custom decoders for specific log formats.
- `alerts/` – Examples and recommendations for alert settings.
- `dashboards/` – Visualization panels for Kibana/OpenSearch Dashboards.
- `integrations/` – Configurations for integrations with other systems.

---

## 🚀 Getting Started
1. Clone the repository:
   ```bash
   git clone https://github.com/AdamFiser/siem_templates.git
   ```
2. Select the templates you want to use.
3. Copy them into your Wazuh installation (e.g., `/var/ossec/etc/` or the equivalent directory).
4. Restart the Wazuh Manager service:
   ```bash
   systemctl restart wazuh-manager
   ```

---

## 🤝 How to Contribute
Contributions are highly welcome! You can help by adding new rules, improving existing ones, or fixing mistakes.

1. **Fork** this repository.
2. Create a **new branch** for your changes:
   ```bash
   git checkout -b my-changes
   ```
3. Add or edit files.
4. Test to ensure your rules/configurations work correctly.
5. Submit a **Pull Request** with a clear description:
   - What has been changed/added
   - Why it is useful
   - How to test it

> **Tip:** When adding a new rule, include details of the detected event and example log entries in the PR description.

---

## 📜 License
This project is licensed under the [MIT License](LICENSE).  
You are free to use, modify, and share according to your needs.

---

## 📧 Contact
If you have questions, open an **Issue** or contact the author directly via [GitHub](https://github.com/AdamFiser).
