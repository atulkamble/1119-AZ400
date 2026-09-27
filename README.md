# Azure DevOps – Windows Development Environment Setup

A practical installation and verification guide for students preparing a Windows machine for Azure DevOps, Python, Git, Docker, Kubernetes, and Terraform labs.

## 1. Prerequisites and accounts

- Windows 11 or a supported Windows 10 version, administrator access, stable internet, and hardware virtualization enabled for WSL 2 / Docker Desktop.
- Check **Settings → System → About** to identify your CPU architecture (**x64/AMD64** or **ARM64**) before downloading Docker Desktop.
- **Microsoft Azure:** [Create an Azure account](https://azure.microsoft.com/) and open the [Azure portal](https://portal.azure.com/). Eligible new customers may receive a **30-day introductory credit offer**; the amount, eligibility, regions and continuing free-service limits depend on the current offer. Check the signup page and set a budget / cost alerts before creating resources.
- **GitHub:** [Sign up](https://github.com/).
- **Docker Hub:** [Sign up](https://hub.docker.com/).
- **Azure DevOps:** [Create or open your organization](https://dev.azure.com/).

> **Cost reminder:** Azure free-tier credits are not unlimited. AKS, VMs, networking, storage and other lab resources may incur charges; delete unused resources promptly.

## 2. Install PowerShell 7 (optional, recommended)

Official instructions: [Install PowerShell on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell?view=powershell-7.6).

```powershell
winget install --id Microsoft.PowerShell --source winget
pwsh --version
```

Windows PowerShell 5.1 is built into most Windows installations; PowerShell 7 is a separate, cross-platform, open-source edition. Open an **Administrator** terminal where required.

## 3. Install Chocolatey

Official instructions: [Chocolatey installation](https://chocolatey.org/install).

Open **Windows PowerShell as Administrator**:

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Start a new terminal and verify:

```powershell
choco --version
```

Only run installation scripts after checking their official source and your organization's security policy.

## 4. Install Python

Official download: [python.org/downloads](https://www.python.org/downloads/).

```powershell
choco install python -y
```

Open a **new** terminal and verify:

```powershell
python --version
python -m pip --version
```

Azure CLI bundles a Python runtime for its own operation, but install Python separately for Python development and labs.

## 5. Install Azure CLI and sign in

Official Windows instructions: [Microsoft Learn – Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=msi).

```powershell
choco install azure-cli -y
```

In a new terminal:

```powershell
az --version
az version
az login
az account show
az account list --output table
```

If the browser cannot launch, use `az login --use-device-code`. Select the intended subscription before deploying resources.

## 6. Install WSL 2 with Ubuntu

Run PowerShell **as Administrator**:

```powershell
wsl --install -d Ubuntu
```

Restart if requested and complete Ubuntu's first-run username/password setup. Verify:

```powershell
wsl --list --verbose
wsl --set-default-version 2
```

To inspect available Linux distributions: `wsl --list --online`.

## 7. Install Git

Official site: [git-scm.com](https://git-scm.com/) and [Windows installer](https://git-scm.com/install/windows).

Choose **one** installation method:

```powershell
choco install git -y
```

or:

```powershell
winget install --id Git.Git -e --source winget
```

Verify and configure:

```powershell
git --version
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
git config --global --list
```

## 8. Install Visual Studio Code

Official download: [code.visualstudio.com](https://code.visualstudio.com/).

```powershell
choco install vscode -y
```

Verify after opening a new terminal:

```powershell
code --version
```

Suggested extensions for labs: Python, Docker, Kubernetes, HashiCorp Terraform, YAML, and Azure Tools.

## 9. Install Docker Desktop

Official download: [Docker Desktop](https://www.docker.com/products/docker-desktop/).

Download the installer matching your **Windows AMD64 or ARM64** device. Complete Docker Desktop setup; enable the **WSL 2 backend** and appropriate WSL integration when available. Start Docker Desktop before testing:

```powershell
docker --version
docker compose version
docker info
docker run --rm hello-world
```

Docker Desktop licensing terms may apply in larger organizations; consult its current license before commercial use.

## 10. Install Kubernetes tools and Minikube

Official documentation: [Minikube](https://minikube.sigs.k8s.io/docs/).

```powershell
choco install minikube kubernetes-cli -y
```

With Docker Desktop running:

```powershell
minikube version
kubectl version --client
minikube start --driver=docker
kubectl get nodes
minikube status
```

Minikube is for **local Kubernetes practice**. Azure Kubernetes Service (AKS) is a **managed Azure service**; it is not installed with Chocolatey. An AKS cluster can be created later using the Azure portal or Azure CLI and requires an appropriate subscription and permissions.

## 11. Install and configure the Azure DevOps CLI extension

```powershell
az extension add --name azure-devops
az extension update --name azure-devops
az extension show --name azure-devops --output table
```

Sign in to Azure with `az login`. Configure the DevOps organization and project:

```powershell
az devops configure --defaults organization=https://dev.azure.com/YOUR_ORG project=YOUR_PROJECT
az devops project list --output table
```

For environments where PAT authentication is required, create a **least-privilege** PAT in Azure DevOps and follow the interactive command below; do **not** place the PAT in notes or commit it to Git:

```powershell
az devops login --organization https://dev.azure.com/YOUR_ORG
```

## 12. Install Terraform

Official instructions: [HashiCorp Terraform install](https://developer.hashicorp.com/terraform/install).

```powershell
choco install terraform -y
```

Open a new terminal and verify:

```powershell
terraform version
```

## 13. Final verification checklist

Run each command in a newly opened PowerShell terminal. Some tools require Docker Desktop or an authenticated Azure session.

```powershell
pwsh --version
choco --version
python --version
python -m pip --version
az --version
wsl --list --verbose
git --version
code --version
docker --version
docker compose version
minikube version
kubectl version --client
terraform version
az extension show --name azure-devops --output table
```

## Official reference links

| Tool / account | Official link |
|---|---|
| Microsoft Azure account | https://azure.microsoft.com/ |
| Azure portal | https://portal.azure.com/ |
| Azure DevOps | https://dev.azure.com/ |
| GitHub signup | https://github.com/ |
| Docker Hub signup | https://hub.docker.com/ |
| Python | https://www.python.org/downloads/ |
| Azure CLI on Windows | https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=msi |
| PowerShell 7 | https://learn.microsoft.com/en-us/powershell/scripting/install/install-powershell?view=powershell-7.6 |
| VS Code | https://code.visualstudio.com/ |
| Chocolatey | https://chocolatey.org/install |
| Git | https://git-scm.com/ |
| Docker Desktop | https://www.docker.com/products/docker-desktop/ |
| Minikube | https://minikube.sigs.k8s.io/docs/ |
| Terraform | https://developer.hashicorp.com/terraform/install |


Azure DevOps – Create and Manage Personal Access Token (PAT)
A Personal Access Token (PAT) is used to authenticate Azure DevOps CLI commands without entering your username and password each time.
1. Create an Azure DevOps Organization
Open Azure DevOps
 and sign in with your Microsoft account. Create an organization and a project.
Example:
- Organization: cloudnautic
- Project: project
- URL: https://dev.azure.com/cloudnautic/project

2. Install and Configure Azure DevOps CLI
Install the Azure DevOps extension:
```
az extension add --name azure-devops
```

Configure the default organization and project:
```
az devops configure --defaults \
  organization=https://dev.azure.com/cloudnautic \
  project=project
```

Note: Use the organization URL, not the project URL, for the organization parameter.
Verify the configuration:
```
az devops configure --list
```

3. Create a Personal Access Token
Azure DevOps Portal
1. Open https://dev.azure.com/cloudnautic
2. Click User settings in the upper-right corner.
3. Select Personal access tokens.
4. Click New Token.
5. Enter token name: mytoken.
6. Select the organization and expiration date.
7. Select the minimum permissions required. For this lab, use Project and Team: Read to list projects.
8. Click Create and immediately copy the generated token.

Keep your PAT secure. Azure DevOps displays its value only when it is created. Do not commit it to GitHub or include it in screenshots.
4. Log In Using the Token
Open your terminal:
```
az devops login \
  --organization https://dev.azure.com/cloudnautic
```

Paste your PAT when prompted and press Enter.
Alternatively, authenticate using an environment variable without placing the token directly in a command:
read -s AZURE_DEVOPS_EXT_PAT
export AZURE_DEVOPS_EXT_PAT


Paste the token at the prompt and press Enter. The environment variable will be available to the Azure DevOps CLI for that shell session.
5. Verify Authentication
List the available projects:
```
az devops project list --output table
```

Get information about your project:
```
az devops project show \
  --project project \
  --output table
```

6. Maintain and Revoke Tokens
To manage existing tokens, return to User settings → Personal access tokens in Azure DevOps.
You can create a replacement token, regenerate an existing token where supported, or revoke a token that is no longer required. Use short expiration periods and rotate tokens before they expire.
To remove stored CLI authentication:
az devops logout \
  --organization https://dev.azure.com/cloudnautic


If you used the environment variable, clear it:
unset AZURE_DEVOPS_EXT_PAT


For production automation, prefer Microsoft Entra identities or managed identities where supported rather than long-lived PATs.
