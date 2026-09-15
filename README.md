# Static Website CI/CD --- Windows Server

<p align="center">
<strong>Automated Static HTML & CSS Deployment with GitHub, Jenkins & IIS</strong>
</p>
<p align="center">
A practical CI/CD implementation for deploying a static website to
Microsoft IIS on Windows Server.
</p>
<p align="center">
<img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5">
<img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3">
<img src="https://img.shields.io/badge/Git-GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
<img src="https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?style=for-the-badge&logo=jenkins&logoColor=white" alt="Jenkins">
<img src="https://img.shields.io/badge/IIS-Windows%20Server-0078D4?style=for-the-badge&logo=windows&logoColor=white" alt="IIS">
</p>

---

## 📌 Project Overview

This project demonstrates a complete **CI/CD workflow for a static
HTML + CSS website** deployed on a Windows Server using **Microsoft
IIS** and **Jenkins**.

The source code is maintained in GitHub. Jenkins retrieves the latest
state of the `main` branch and copies the website files into the IIS
deployment directory. IIS then serves the static files directly.

| Property | Configuration |
|---|---|
| Project | Static Website Deployed on Windows Server Through CI/CD |
| Application | Static HTML + CSS |
| Source Control | Git + GitHub |
| CI/CD | Jenkins |
| Web Server | Microsoft IIS |
| IIS Site | `Static-Website-Portfolio` |
| Application Pool | `Static-Website-Portfolio` |
| HTTP Port | `8082` |
| Branch | `main` |
| License | MIT |

## ✨ Key Features

-   Static HTML5 website
-   CSS3 styling
-   Git-based version control
-   GitHub repository
-   Jenkins CI/CD automation
-   Explicit Git fetch/update workflow
-   Automated deployment using `xcopy`
-   Dedicated IIS website
-   Dedicated IIS application pool
-   Windows Server hosting
-   MIT licensed repository
-   No Node.js runtime
-   No npm installation
-   No PM2
-   No reverse proxy required

---

## 🏗️ Architecture

``` text
Developer
    │
    │ git add / commit / push
    ▼
GitHub Repository
    │
    │ Jenkins retrieves latest code
    ▼
Jenkins Pipeline
    │
    ├── Checkout and Update Code
    │
    └── Deploy Website Files
             │
             │ xcopy
             ▼
C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server
             │
             ▼
Microsoft IIS
    Static-Website-Portfolio
          HTTP :8082
             │
             ▼
       End User Browser
```

### Request Flow

``` text
Browser
   │
   │ http://localhost:8082
   ▼
IIS :8082
   │
   ├── index.html
   │
   └── style.css
   │
   ▼
Browser renders website
```

Unlike the Node.js deployment, IIS does **not** forward requests to
another application process. The HTML/CSS files themselves are the
deployed application.

---

## 🧰 Technology Stack

| Technology | Purpose |
|---|---|
| HTML5 | Website structure/content |
| CSS3 | Presentation and layout |
| Git | Version control |
| GitHub | Source repository |
| Jenkins | CI/CD automation |
| Microsoft IIS | Static web server |
| Windows Server | Hosting environment |
| CMD / PowerShell | Administration and deployment |

## 📁 Project Structure

``` text
Static-Website-CICD-Windows-Server/
│
├── .gitignore
├── index.html
├── style.css
├── Jenkinsfile
├── LICENSE
└── README.md
```

| File | Purpose |
|---|---|
| `index.html` | Main HTML document |
| `style.css` | Website styling |
| `Jenkinsfile` | Jenkins CI/CD pipeline |
| `.gitignore` | Files excluded from Git |
| `README.md` | Project README |
| `LICENSE` | MIT license |

## 🔗 Repository

**GitHub:**
`https://github.com/MutahirTayyab/Static-Website-CICD-Windows-Server`

The deployment source is the `main` branch.

---

# 🚀 Local Development

### Project Location

``` text
D:\Cloud\Projects\Project#5\Static-Website-CICD-Windows-Server
```

Open the folder in Visual Studio Code and verify:

``` text
index.html
style.css
Jenkinsfile
.gitignore
LICENSE
README.md
```

### Run Locally

This is a static website, so there is no Node.js server, `npm install`,
`package.json`, or PM2 process.

You can open `index.html` directly in a browser or use a local static
web server.

Verify:

-   HTML loads correctly
-   CSS is applied
-   Relative paths work
-   Required files are present
-   No Node.js backend is required

---

# 🌿 Git & GitHub

``` powershell
cd D:\Cloud\Projects\Project#5\Static-Website-CICD-Windows-Server
git init
git status
git add .
git commit -m "Initial Static Website Setup"
git branch -M main
git remote add origin https://github.com/MutahirTayyab/Static-Website-CICD-Windows-Server.git
git remote -v
git push -u origin main
```

---

# 🖥️ IIS Configuration

## Deployment Directory

``` text
C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server
```

## Website Configuration

| Setting | Value |
|---|---|
| Site Name | `Static-Website-Portfolio` |
| Physical Path | `C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server` |
| Type | HTTP |
| IP Address | All Unassigned |
| Port | `8082` |
| Host Name | None |

Access the website:

``` text
http://localhost:8082
```

---

# ⚙️ IIS Application Pool

| Setting | Value |
|---|---|
| Application Pool | `Static-Website-Portfolio` |
| .NET CLR Version | `No Managed Code` |
| Managed Pipeline | `Integrated` |
| Start Immediately | Enabled |

### Why No Managed Code?

The project does not execute ASP.NET/.NET application code. IIS only
reads and serves static files.

``` text
Browser
   │
   ▼
IIS :8082
   │
   ├── index.html
   └── style.css
   │
   ▼
Browser
```

---

# 🔄 Jenkins CI/CD

The pipeline contains two stages:

``` text
Checkout and Update Code
          │
          ▼
Deploy Website Files
          │
          ▼
IIS Deployment Directory
```

There is intentionally **no build stage** because HTML/CSS does not
require compilation.

There is also **no restart stage** because there is no Node.js/PM2
process.

---

# 🧩 Jenkinsfile

``` groovy
pipeline {

    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout and Update Code') {

            steps {

                bat """

                if exist .git (

                    echo Repository exists
                    git fetch origin main
                    git reset --hard origin/main

                ) else (

                    echo First time checkout
                )

                """

            }
        }

        stage('Deploy Website Files') {

            steps {

                bat """

                echo Deploying Static Website...

                xcopy /E /I /Y C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Static-Website-Windows-Server\\* C:\\inetpub\\wwwroot\\Static-Website-CICD-Windows-Server\\

                echo Deployment Completed

                """

            }
        }
    }
}
```

## 🔍 Stage 1 --- Checkout and Update Code

If the Jenkins workspace already contains `.git`:

``` bat
git fetch origin main
git reset --hard origin/main
```

Jenkins retrieves the latest `main` branch state and synchronizes the
workspace with `origin/main`.

For a first-time workspace:

``` bat
git clone https://github.com/MutahirTayyab/Static-Website-CICD-Windows-Server.git
```

---

## 📦 Stage 2 --- Deploy Website Files

``` bat
xcopy /E /I /Y SOURCE DESTINATION
```

| Option | Meaning |
|---|---|
| `/E` | Copies subdirectories, including empty directories |
| `/I` | Assumes destination is a directory |
| `/Y` | Suppresses overwrite confirmation |

Destination:

``` text
C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server
```

---

# 🧪 End-to-End CI/CD Test

Modify:

``` text
index.html
```

or:

``` text
style.css
```

Then:

``` powershell
git status
git add .
git commit -m "Update static website"
git push
```

Jenkins performs:

``` text
GitHub
   │
   ▼
Fetch / Update
   │
   ▼
Jenkins Workspace
   │
   ▼
xcopy
   │
   ▼
IIS Deployment Directory
   │
   ▼
http://localhost:8082
```

Refresh the browser and verify the change.

---

# 📊 Static Website vs Node.js Deployment

| Capability | Node.js Project | Static Website |
|---|---|---|
| Runtime | Node.js + Express | None |
| Frontend | HTML/CSS/JS | HTML/CSS |
| Dependencies | npm | None |
| Build | Application-dependent | Not required |
| Process Manager | PM2 | Not required |
| Reverse Proxy | IIS + URL Rewrite/ARR | Not required |
| Deployment | Copy + PM2 restart | Copy files |
| IIS Role | Reverse proxy / web server | Direct static web server |
| Pipeline | Checkout → install → deploy → restart | Checkout/update → deploy |


The static project preserves the same core DevOps principles while using
a simpler deployment model:

**Source Control → CI/CD Automation → Deployment → IIS Hosting**

---

# 🔐 Security Practices

-   Do not commit passwords, API keys, tokens, or private keys.
-   Use `.gitignore` for unnecessary local files.
-   Restrict write permissions on the IIS deployment directory.
-   Use authenticated GitHub access if the repository becomes private.
-   Review Jenkins service-account permissions.
-   Keep production/server-specific configuration outside source control
    where appropriate.

---

# 📋 Useful Commands

### Git

``` powershell
git status
git add .
git commit -m "Update static website"
git push
git remote -v
git branch
```

### IIS

``` powershell
iisreset
```

``` powershell
Get-WindowsOptionalFeature -Online -FeatureName IIS-WebServerRole
```

### Deployment Directory

``` powershell
mkdir C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server
```

``` powershell
xcopy /E /I /Y "SOURCE\*" "C:\inetpub\wwwroot\Static-Website-CICD-Windows-Server\"
```

### Website

``` text
http://localhost:8082
```

### Jenkins

``` text
http://localhost:8080
```

---

# ✅ Completion Checklist

-   [x] Static HTML/CSS website
-   [x] Git repository
-   [x] GitHub repository
-   [x] `main` branch
-   [x] MIT License
-   [x] README
-   [x] IIS installed
-   [x] Dedicated IIS website
-   [x] Dedicated IIS application pool
-   [x] `No Managed Code`
-   [x] Port `8082`
-   [x] IIS deployment directory
-   [x] Manual deployment
-   [x] Jenkins pipeline
-   [x] Explicit Git update workflow
-   [x] Automated `xcopy` deployment
-   [x] End-to-end CI/CD testing

---

# 🎯 Final Workflow

``` text
Developer
    │
    │ Code Changes
    ▼
Git
    │
    │ git push
    ▼
GitHub
    │
    │ fetch / update
    ▼
Jenkins Pipeline
    │
    ├── Checkout / Update Code
    │
    └── Deploy Website Files
             │
             │ xcopy
             ▼
IIS Deployment Directory
             │
             ▼
Static-Website-Portfolio
             │
             │ HTTP :8082
             ▼
Live Website
```

---
