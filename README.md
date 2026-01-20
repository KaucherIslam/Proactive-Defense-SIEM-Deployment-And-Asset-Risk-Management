<h1 align="center">Proactive Defense: SIEM Deployment & Asset Risk Management</h1>

<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; border-left: 5px solid #007acc; color: #ffffff;">
  <strong>Tools Used:</strong>
  <ul>
    <li><a href="https://documentation.wazuh.com/current/installation-guide/index.html" style="color: #4db8ff;">Wazuh (Ubuntu/APT) </a></li>
    <li><a href="https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-windows.html" style="color: #4db8ff;">Wazuh Agent (Windows) </a></li>
  </ul>
</div>

<h2>1. Infrastructure Setup and Emergency Recovery</h2>

<p>The project started with the deployment of the Wazuh SIEM/XDR stack on an Ubuntu server to act as a central hub for security telemetry.</p>

<ul>
  <li><b>The Failure:</b> During the installation, the server ran out of disk space, causing the installation script to fail and the services to crash.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-11 213800" src="https://github.com/user-attachments/assets/1e53ca7e-bec6-4519-9117-a7b321cbe6b4" />
<br />
<em>Figure 1: Initial installation failure due to "No space left on device" error.</em>
</p>

<ul>
  <li><b>Resolution:</b> I manually expanded the Logical Volume Management (LVM) partition and resized the filesystem to allow the installation to resume and complete.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-11 215938" src="https://github.com/user-attachments/assets/6259672c-111a-4fed-83ff-54d2f6f15492" />
<br />
<em>Figure 2: Resolving the infrastructure failure through partition management.</em>
</p>

<h2>2. OS Stability and Package Recovery</h2>

<p>The storage crash occurred while the system was writing critical files, leading to deep-level corruption of the package manager (DPKG).</p>

<ul>
  <li><b>The Error:</b> Encountered "DPKG Error 127," which effectively locked the operating system from performing any further installations or updates.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-12 163922" src="https://github.com/user-attachments/assets/468130d7-d125-4c14-8cb7-627a80c9af1f" />
<br />
<em>Figure 3: Encountering the "Sub-process /usr/bin/dpkg returned an error code" failure.</em>
</p>

<ul>
  <li><b>The Fix:</b> I identified the corrupted records within the system status files and manually purged them. This stabilized the OS and allowed for a successful repair of the broken dependencies.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-12 155650" src="https://github.com/user-attachments/assets/c65bcbd4-5c85-4c6e-b0d7-416d39c25ab4" />
<br />
<em>Figure 4: Recovering the package manager from internal file corruption and restarting the installation.</em>
</p>

<h2>3. Agent Deployment and Connection</h2>

<p>Once the manager was stable, I deployed the Wazuh agent to a Windows 11 host to begin active monitoring.</p>

<ul>
  <li><b>Enrollment:</b> Installed the agent software and configured it to point to the server's IP address and configuration key</li> 
</ul>

<p align="center">
<img width="98%" height="98%" alt="IMG_20260120_220556" src="https://github.com/user-attachments/assets/456b21d6-dc3e-4433-a0e0-c3a166f306a7" />
<br />
<em>Figure 5: Manually configuring the Manager IP and Authentication Key for secure enrollment.</em>
</p>

<ul>
  <li><b>Validation:</b> The connection was verified through the dashboard, confirming the Windows host was in an "Active" state and shipping telemetry.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-20 173224" src="https://github.com/user-attachments/assets/35c9475c-37ed-4f84-ac32-d8220288413d" />
<br />
<em>Figure 6: Successful agent handshake showing 'Active' status for Agent 003.</em>
</p>

<h2>4. Vulnerability Audit and Findings</h2>

<p>With the connection live, I conducted a baseline security audit to evaluate the vulnerability posture of the Windows 11 endpoint.</p>

<ul>
  <li><b>Audit Results:</b> The scan identified <b>67 vulnerabilities</b>, including 6 classified as critical.</li>
  <li><b>Baseline Methodology:</b> The workstation was maintained as a baseline lab with unpatched third-party applications, such as older versions of VLC Media Player and Python. This provided a realistic dataset of Common Vulnerabilities and Exposures (CVEs) to verify the platform’s detection capabilities.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-20 184144" src="https://github.com/user-attachments/assets/32bd6cb0-aad5-4dc6-8b67-82fa040e81f4" />
<br />
<em>Figure 7: Detailed vulnerability audit identifying unpatched software risks.</em>
</p>

<h2>5. Remediation and Lifecycle Verification</h2>

<p>The final phase demonstrated the full security lifecycle by patching the identified flaws and verifying the results through the SIEM.</p>

<ul>
  <li><b>Patch Execution:</b> I used PowerShell and the Windows Package Manager (winget) to update the vulnerable software libraries and applications.</li>
  <li><b>Final Scan:</b> I forced a manual refresh from the manager to update the inventory. The dashboard updated to show a successful reduction in the vulnerability count, confirming the host reached a more secure state.</li>
</ul>

<p align="center">
<img width="98%" height="98%" alt="Screenshot 2026-01-20 193524" src="https://github.com/user-attachments/assets/296b8efc-a3af-4250-acf9-16bb8fe00533" />
<br />
<em>Figure 8: Post-remediation dashboard confirming a reduced risk surface.</em>
</p>

<h2>Technical Skills Demonstrated</h2>

<ul>
  <li><b>Linux Systems Engineering:</b> Managing LVM partitions and filesystem expansion under failure conditions.</li>
  <li><b>Advanced Troubleshooting:</b> Recovering from DPKG/APT corruption by manually purging status records.</li>
  <li><b>XDR/SIEM Administration:</b> Deploying agents and managing centralized security telemetry.</li>
  <li><b>Vulnerability Management:</b> Conducting baseline audits, mapping CVEs, and executing remediation cycles.</li>
</ul>

<h1>Final Reflection</h1>

<p>This project was less about a standard installation and more about managing the technical failures that occur in real-world environments. The primary takeaway was the recovery of a corrupted OS environment and the ability to use a SIEM to uncover a "shadow" attack surface that standard desktop security tools often overlook. This lab proved the effectiveness of using centralized monitoring to drive a structured patching and remediation plan.</p>

<p>Thank you for taking the time to review my project and for your consideration of my work.</p>
