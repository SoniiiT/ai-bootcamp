# Tech-Stack

## Programmiersprachen

### Backend
- **Python 3.11+**: Primäre Sprache für Backend-Services
  - Frameworks: Flask, Django, FastAPI
  - Async: asyncio, aiohttp
- **C# (.NET 8)**: Enterprise-Anwendungen und Windows-Integration
  - ASP.NET Core Web APIs
  - Entity Framework Core
  - SignalR für Real-time
- **Node.js (LTS)**: JavaScript/TypeScript Backend
  - Express.js, NestJS
  - TypeScript bevorzugt

### Frontend
- **React 18+**: Haupt-Frontend-Framework
- **TypeScript**: Standard für neue Projekte
- **HTML5/CSS3**: Moderne Web-Standards
- **Tailwind CSS**: Utility-First CSS Framework

### Scripting & Automation
- **PowerShell 7+**: Windows-Automatisierung
- **Bash**: Linux-Automatisierung
- **Python**: Plattformübergreifende Skripte

## Datenbanken

### Relational
- **PostgreSQL 15+**: Primäre relationale Datenbank
- **Microsoft SQL Server 2022**: Für .NET-Anwendungen
- **MySQL 8+**: Legacy-Systeme und spezifische Anforderungen

### NoSQL
- **MongoDB**: Dokumenten-Datenbank
- **Redis**: Caching und Session-Storage
- **Azure Cosmos DB**: Cloud-native NoSQL

## Container & Orchestrierung

### Container
- **Docker**: Standard für Containerisierung
- **Docker Compose**: Lokale Entwicklung und einfache Deployments
- **Podman**: Alternative für bestimmte Szenarien

### Orchestrierung
- **Kubernetes**: Production-Workloads
  - Azure Kubernetes Service (AKS)
  - On-Premises K8s Cluster
- **Docker Swarm**: Einfache Cluster (Legacy-Migration zu K8s)

### Registry
- **Azure Container Registry (ACR)**: Primäre Registry
- **Harbor**: On-Premises Registry

## Cloud-Plattformen

### Microsoft Azure (Primär)
- **Compute**: VMs, App Services, Container Instances, AKS
- **Storage**: Blob Storage, File Storage, Disk Storage
- **Networking**: VNet, Load Balancer, Application Gateway, VPN
- **Databases**: Azure SQL, Cosmos DB, PostgreSQL Flexible Server
- **Security**: Key Vault, Microsoft Defender for Cloud
- **DevOps**: Azure DevOps, Artifacts, Pipelines
- **Monitoring**: Application Insights, Log Analytics, Azure Monitor

### AWS (Sekundär)
- **Compute**: EC2, ECS, Lambda
- **Storage**: S3, EBS
- **Databases**: RDS, DynamoDB
- **Networking**: VPC, Route 53

## CI/CD & DevOps

### CI/CD-Plattformen
- **Azure DevOps**: Primäres CI/CD-Tool
  - Repos, Pipelines, Artifacts, Boards
- **GitHub Actions**: Open-Source-Projekte und GitHub-basierte Workflows
- **GitLab CI**: Kundenspezifische Anforderungen

### Infrastructure as Code
- **Terraform**: Multi-Cloud IaC
- **Azure Bicep**: Azure-spezifische Deployments
- **Ansible**: Konfigurationsmanagement

### Version Control
- **Git**: Standard-Versionskontrolle
- **Azure Repos**: Interne Projekte
- **GitHub**: Open-Source und externe Kollaboration

## Monitoring & Logging

### Application Performance Monitoring (APM)
- **Azure Application Insights**: Primäres APM-Tool
- **Prometheus + Grafana**: Kubernetes-Monitoring
- **ELK Stack (Elasticsearch, Logstash, Kibana)**: Log-Aggregation

### Infrastructure Monitoring
- **Azure Monitor**: Cloud-Ressourcen
- **Zabbix**: On-Premises Infrastructure
- **Nagios**: Legacy-Systeme (Migration zu moderneren Tools)

### Logging
- **Azure Log Analytics**: Zentrales Logging für Azure
- **Elasticsearch**: On-Premises Log-Aggregation
- **Seq**: Strukturiertes Logging für .NET-Anwendungen

## Systemadministration

### Windows
- **Windows Server 2022**: Standard-Betriebssystem
- **Active Directory**: Benutzerverwaltung
- **Group Policy**: Zentrale Konfiguration
- **Hyper-V**: On-Premises Virtualisierung
- **PowerShell 7+**: Automatisierung

### Linux
- **Ubuntu 22.04 LTS**: Primäre Distribution
- **Debian 12**: Server-Workloads
- **RHEL 9 / Rocky Linux 9**: Enterprise-Anforderungen
- **Alpine Linux**: Container-Base-Images

### Netzwerk
- **pfSense / OPNsense**: Firewall-Lösungen
- **Cisco**: Enterprise-Networking (Switches, Router)
- **Ubiquiti**: SMB-Networking
- **WireGuard / OpenVPN**: VPN-Lösungen

## Security

### Identity & Access Management
- **Azure Active Directory**: Cloud-Identity
- **Active Directory**: On-Premises Identity
- **Okta**: SSO für bestimmte Kunden

### Secrets Management
- **Azure Key Vault**: Primär für Cloud
- **HashiCorp Vault**: Multi-Cloud und On-Premises

### Security Tools
- **Microsoft Defender**: Endpoint Protection
- **Nessus / OpenVAS**: Vulnerability Scanning
- **Wazuh**: SIEM und Intrusion Detection
- **Fail2ban**: Brute-Force-Protection

## Collaboration & Documentation

### Projektmanagement
- **Azure DevOps Boards**: Agile-Projektmanagement
- **Jira**: Kundenspezifische Anforderungen

### Dokumentation
- **Confluence**: Zentrale Wissensdatenbank
- **Markdown**: Code-Dokumentation in Repos
- **Draw.io**: Diagramme und Architekturen

### Kommunikation
- **Microsoft Teams**: Primäres Kommunikationstool
- **Slack**: Kundenspezifisch
- **Email**: Microsoft Exchange / Microsoft 365

## Development Tools

### IDEs & Editors
- **Visual Studio Code**: Primärer Editor
- **Visual Studio 2022**: .NET-Entwicklung
- **JetBrains Suite**: PyCharm, Rider (nach Bedarf)

### API Development
- **Postman**: API-Testing
- **Swagger / OpenAPI**: API-Dokumentation
- **Insomnia**: Alternative zu Postman

## Best Practices

### Code-Qualität
- **SonarQube**: Code-Qualitätsanalyse
- **ESLint / Pylint**: Linting
- **Black / Prettier**: Code-Formatierung
- **Unit Tests**: pytest, Jest, xUnit

### Container-Best-Practices
- Multi-Stage-Builds für kleinere Images
- Non-Root-User in Containern
- Vulnerability-Scanning mit Trivy
- Image-Tagging nach SemVer

### Security-Best-Practices
- Secrets nie im Code
- Principle of Least Privilege
- Regelmäßige Dependency-Updates
- Automatisierte Security-Scans in CI/CD
