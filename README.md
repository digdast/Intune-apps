# 💻 Intune Applications & Lenovo Enterprise Automation

A curated collection of **Microsoft Intune (Endpoint Manager)** deployment packages, Autopilot automation scripts, and Proactive Remediations tailored for enterprise fleet management, with specialized support for Lenovo ThinkPad / ThinkCentre hardware and Intel platforms.

---

## 📂 Repository Architecture

```text
├── Autopilot/
│   ├── System Update/              # Lenovo System Update during Autopilot ESP
│   │   ├── Invoke-SystemUpdate.ps1
│   │   └── Invoke-SystemUpdate.intunewin
│   └── Thin Installer/             # Lenovo Thin Installer driver deployment
│       ├── Detect-AutopilotSession.ps1
│       └── Get-LnvUpdates.ps1
├── Remediations/
│   ├── Apps/
│   │   ├── Commercial Vantage/     # Commercial Vantage System Update logging
│   │   │   ├── Detect-SystemUpdateLogging.ps1
│   │   │   └── Remediate-SystemUpdateLogging.ps1
│   │   └── Thin Installer/         # Thin Installer engine maintenance
│   │       ├── Detect-ThinInstaller.ps1
│   │       └── Remediate-ThinInstaller.ps1
│   └── Asset Tag/                  # BIOS Asset Tag auditing and configuration
│       ├── Detect-AssetTag.ps1
│       └── Remediate-AssetTag.ps1
└── Win32 Apps/
    ├── Commercial_Vantage/         # Lenovo Commercial Vantage deployment
    │   ├── Detect-CommercialVantage.ps1
    │   └── New-CommercialVantageWin32.ps1
    ├── Intel_PPM/                  # Intel Processor Power Management package
    │   ├── Detect-IntelPPMPackage.ps1
    │   ├── Invoke-IntelPPMPackage.ps1
    │   └── Invoke-IntelPPMPackage.intunewin
    └── ThinkBiosConfigTool/        # Lenovo ThinkPad BIOS Supervisor Password
        └── Update-SVP.ps1
```

---

## 🚀 Module Overview

### 1. Autopilot ESP Integrations
* **System Update (`Autopilot/System Update`)**:
  * Runs Lenovo System Update during the Device Preparation or Device Setup phases of Autopilot.
  * Fetches and applies critical BIOS, firmware, and hardware driver updates before the user reaches the desktop.
* **Thin Installer (`Autopilot/Thin Installer`)**:
  * `Detect-AutopilotSession.ps1`: Verifies if the machine is actively undergoing an Out-Of-Box Experience (OOBE) Autopilot provisioning session.
  * `Get-LnvUpdates.ps1`: Downloads and applies vendor packages from an internal network share or repository.

### 2. Proactive Remediations
* **Commercial Vantage Logging**:
  * Detection script checks if System Update logging is configured in registry (`HKLM:\SOFTWARE\Policies\Lenovo\Commercial Vantage`).
  * Remediation script enables logging to simplify remote diagnostics.
* **Asset Tag Management**:
  * Interrogates BIOS WMI classes (`root\wmi:Lenovo_SetBiosSetting` or `Win32_SystemEnclosure`) to ensure hardware asset tags match organization naming conventions.

### 3. Win32 Application Packages
* **Lenovo Commercial Vantage**:
  * `New-CommercialVantageWin32.ps1`: Automates staging the enterprise UWP application, provisioning packages, and required app dependencies.
* **Intel Processor Power Management (PPM)**:
  * Deploys custom power tuning provisioning packages (`.ppkg`) to optimize modern standby, thermals, and battery longevity on supported Intel chipsets.
* **ThinkBiosConfigTool**:
  * `Update-SVP.ps1`: Sets or updates the Lenovo Supervisor Password (SVP) securely across enterprise endpoints without physical user interaction.

---

## 🛠️ Intune Deployment Instructions

1. **Packaging**:
   Convert scripts into `.intunewin` using the Microsoft Win32 Content Prep Tool (`IntuneWinAppUtil.exe`):
   ```cmd
   IntuneWinAppUtil.exe -c <folder> -s <script.ps1> -o <output_dir>
   ```
2. **Execution Context**:
   - Autopilot and BIOS scripts must be assigned as **System** context.
   - For Proactive Remediations, create a new script package under **Intune > Devices > Remediations**, uploading the corresponding `Detect-*.ps1` and `Remediate-*.ps1` files.
