# Security Policy

## Supported Versions

Use this section to tell people about which versions of your project are
currently being supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| 7.0.X   | enable an AI prompt|
| 6.X.X   |unsupported         |
| 5.1.x   | :white_check_mark: |
| 5.0.x   | :x:                |
| 4.0.x   | :white_check_mark: |
| < 4.0   | :x:                |

## Reporting a Vulnerability

Use the group discussion form. 

Tell them where to go, how often they can expect to get an update on a
reported vulnerability, what to expect if the vulnerability is accepted or
declined, etc.

In cybersecurity, "AI hacking" generally refers to two distinct threats: attackers using AI tools to break into your Ubuntu system, and attackers exploiting AI models or agents you are running on Ubuntu.

1. How the Current Policy Stops AI-Powered Attacks

Attackers use AI to automate reconnaissance, guess passwords faster, and weaponize newly disclosed vulnerabilities within hours instead of weeks. The baseline policy counters these directly:
• Automated Vulnerability Exploitation: AI scans the internet for unpatched software as soon as a CVE is published. Enabling unattended-upgrades and Kernel Livepatch closes those windows automatically before automated bots find you.
• AI Credential Stuffing & Brute Force: Attackers use AI to generate targeted password lists. Disabling SSH passwords (PasswordAuthentication no), requiring MFA, and running fail2ban make password-guessing useless.
• Automated Lateral Movement: If an AI-driven worm breaches a web app, AppArmor and strict least-privilege (sudo) rules trap the process so it cannot take over the rest of the operating system.

2. Additional Rules Needed If You Run AI on Ubuntu

If you are hosting local LLMs (like Ollama or vLLM), running autonomous AI agents, or training models on this Ubuntu machine, add this section to your policy:

AI & Machine Learning Workload Security

• Bind Inference APIs to Localhost: Never expose local LLM ports (e.g., Ollama's default port 11434) directly to the public internet. Bind services to 127.0.0.1 and place them behind an authenticated reverse proxy (like Nginx with API keys or mTLS).
• Sandbox AI Agents & Code Execution: If an AI agent is allowed to run bash commands or Python scripts on Ubuntu, isolate it inside a rootless container (Docker/Podman) or a microVM (Firecracker/Kata Containers) with read-only filesystem mounts where possible.
• Verify Model Integrity & Formats: Prohibit loading unverified .pkl (Pickle) model files from public repositories, as they can execute arbitrary malicious code upon loading. Require safer formats like .safetensors and verify SHA-256 checksums before deployment.
• Egress Firewall Filtering: Restrict outbound network access (ufw default deny outgoing with explicit allowlists) on servers running AI agents so a prompt injection attack cannot silently exfiltrate sensitive files to an external server.
• Protect Training Data & Weights: Store proprietary datasets and fine-tuned model weights on encrypted volumes (LUKS) with strict directory permissions (700) restricted to the dedicated AI service user.
If you are running AI tools on Ubuntu, tell me:
• Which AI frameworks or tools you use (e.g., Ollama, vLLM, LangChain, local agents)
• Whether the server is internet-facing or internal-only
I can write the exact firewall, AppArmor, and container isolation rules for your setup.

