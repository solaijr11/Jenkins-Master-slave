# 🧠 Jenkins Master–Slave CI/CD Setup (with Two Slaves)
## 🏗️ Architecture Overview
```css
[ Developer Commit ]
          │
          ▼
 [ Jenkins Master ]
   │     │
   │     ├──> [ CI Agent 1: Build Node ]      
   │     │         - Build code
   │     │         - Run tests
   │     │         - Send to SonarQube
   │     │         - Upload artifact to Nexus
   │     │
   │     └──> [ Done by Master  ]
   │               - Fetch artifact from Nexus
   │               - Deploy to Tomcat
   │               - Run post-deploy checks
   │
   ▼
 [ Notification / Feedback ]

```

---
## ☁️ EC2 Instance Setup

### Master Node

| **Property**       | **Value**                                                  |
|--------------------|------------------------------------------------------------|
| **OS**             | Ubuntu 22.04 LTS                                           |
| **Purpose**        | Jenkins Master / CI Orchestrator                           |
| **Required Ports** | 22 (SSH), 8080 (Jenkins), 50000 (Agent Connection)         |
| **Tools**          | Jenkins, Git, Java (OpenJDK 11 or higher)                  |

### CI-agent Node (Build & Test)
| **Property**       | **Value**                                                  |
|--------------------|------------------------------------------------------------|
| **OS**             | Ubuntu 22.04 LTS                                           |
| **Purpose**        | Build & Test Tasks                                         |
| **Required Ports** | 22 (SSH)                                                   |
| **Tools**          | Maven, Java (OpenJDK 11 or higher)                         |

### Tomcat server (Deploy)
| **Property**       | **Value**                                                  |
|--------------------|------------------------------------------------------------|
| **OS**             | Ubuntu 22.04 LTS                                           |
| **Purpose**        | deploy                                                     |
| **Required Ports** | 22 (SSH) , CustomTCP(8080)                                 |
| **Tools**          | Java (OpenJDK 11 or higher)                         |

# 🔧 Jenkins Master Configuration

1. Install Jenkins

```bash
#!/bin/bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
```
- [🔗link](https://www.jenkins.io/download/)

2. Get Jenkins Initial Admin Password - ```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```

## Access Jenkins:
👉 ```http://<master-public-ip>:8080```

 # ⚙️ Jenkins Node Setup (via Web UI)
  1. Go to Manage Jenkins → Nodes → New Node
  2. Create Node:
   - Name: ```CI-agent```
   - Type: Permanent Agent
   - Remote root directory: ```/home/ubuntu``` - (use 'pwd' command or make one seperate ```dir```)
   - Labels: ```build node```
   - Launch method: Launch agents via SSH
   - Host: ```<slave1-private-ip>```
   - Credentials: Jenkins SSH key
   - (note: Host Key Verification Strategy -> Non verification Strategy ) only for demo
   - Save and Connect

---
# 🔎SonarQube Setup Guide
- [Setuplink🔗](https://github.com/solaijr11/SonarQube)

---
# 🗃️Nexus  Setup Guide
- [Setuplink🔗](fidyfu)
- 
---
# 📡 Tomcat Setup Guide
- [Setuplink🔗](fidyfu)




