# Tech Stack

## Programming Languages

### Backend
- **Python 3.11+**: Primary language for backend services
  - Frameworks: Flask, Django, FastAPI
  - Async: asyncio, aiohttp
- **C# (.NET 8)**: Enterprise applications and Windows integration
  - ASP.NET Core Web APIs
  - Entity Framework Core
  - SignalR for real-time
- **Node.js (LTS)**: JavaScript/TypeScript backend
  - Express.js, NestJS
  - TypeScript preferred

### Frontend
- **React 18+**: Main frontend framework
- **TypeScript**: Standard for new projects
- **HTML5/CSS3**: Modern web standards
- **Tailwind CSS**: Utility-first CSS framework

### Scripting & Automation
- **PowerShell 7+**: Windows automation
- **Bash**: Linux automation
- **Python**: Cross-platform scripts

## Databases

### Relational
- **PostgreSQL 15+**: Primary relational database
- **Microsoft SQL Server 2022**: For .NET applications
- **MySQL 8+**: Legacy systems and specific requirements

### NoSQL
- **MongoDB**: Document database
- **Redis**: Caching and session storage
- **Azure Cosmos DB**: Cloud-native NoSQL

## Container & Orchestration

### Container
- **Docker**: Standard for containerization
- **Docker Compose**: Local development and simple deployments
- **Podman**: Alternative for certain scenarios

### Orchestration
- **Kubernetes**: Production workloads
  - Azure Kubernetes Service (AKS)
  - On-premises K8s cluster
- **Docker Swarm**: Simple clusters (legacy migration to K8s)

### Registry
- **Azure Container Registry (ACR)**: Primary registry
- **Harbor**: On-premises registry

## Cloud Platforms

### Microsoft Azure (Primary)
- **Compute**: VMs, App Services, Container Instances, AKS
- **Storage**: Blob Storage, File Storage, Disk Storage
- **Networking**: VNet, Load Balancer, Application Gateway, VPN
- **Databases**: Azure SQL, Cosmos DB, PostgreSQL Flexible Server
- **Security**: Key Vault, Microsoft Defender for Cloud
- **DevOps**: Azure DevOps, Artifacts, Pipelines
- **Monitoring**: Application Insights, Log Analytics, Azure Monitor

### AWS (Secondary)
- **Compute**: EC2, ECS, Lambda
- **Storage**: S3, EBS
- **Databases**: RDS, DynamoDB
- **Networking**: VPC, Route 53

## CI/CD & DevOps

### CI/CD Platforms
- **Azure DevOps**: Primary CI/CD tool
  - Repos, Pipelines, Artifacts, Boards
- **GitHub Actions**: Open-source projects and GitHub-based workflows
- **GitLab CI**: Customer-specific requirements

### Infrastructure as Code
- **Terraform**: Multi-cloud IaC
- **Azure Bicep**: Azure-specific deployments
- **Ansible**: Configuration management

### Version Control
- **Git**: Standard version control
- **Azure Repos**: Internal projects
- **GitHub**: Open-source and external collaboration

## Monitoring & Logging

### Application Performance Monitoring (APM)
- **Azure Application Insights**: Primary APM tool
- **Prometheus + Grafana**: Kubernetes monitoring
- **ELK Stack (Elasticsearch, Logstash, Kibana)**: Log aggregation

### Infrastructure Monitoring
- **Azure Monitor**: Cloud resources
- **Zabbix**: On-premises infrastructure
- **Nagios**: Legacy systems (migration to modern tools)

### Logging
- **Azure Log Analytics**: Central logging for Azure
- **Elasticsearch**: On-premises log aggregation
- **Seq**: Structured logging for .NET applications

## System Administration

### Windows
- **Windows Server 2022**: Standard operating system
- **Active Directory**: User management
- **Group Policy**: Central configuration
- **Hyper-V**: On-premises virtualization
- **PowerShell 7+**: Automation

### Linux
- **Ubuntu 22.04 LTS**: Primary distribution
- **Debian 12**: Server workloads
- **RHEL 9 / Rocky Linux 9**: Enterprise requirements
- **Alpine Linux**: Container base images

### Network
- **pfSense / OPNsense**: Firewall solutions
- **Cisco**: Enterprise networking (switches, routers)
- **Ubiquiti**: SMB networking
- **WireGuard / OpenVPN**: VPN solutions

## Security

### Identity & Access Management
- **Azure Active Directory**: Cloud identity
- **Active Directory**: On-premises identity
- **Okta**: SSO for certain customers

### Secrets Management
- **Azure Key Vault**: Primary for cloud
- **HashiCorp Vault**: Multi-cloud and on-premises

### Security Tools
- **Microsoft Defender**: Endpoint protection
- **Nessus / OpenVAS**: Vulnerability scanning
- **Wazuh**: SIEM and intrusion detection
- **Fail2ban**: Brute-force protection

## Collaboration & Documentation

### Project Management
- **Azure DevOps Boards**: Agile project management
- **Jira**: Customer-specific requirements

### Documentation
- **Confluence**: Central knowledge base
- **Markdown**: Code documentation in repos
- **Draw.io**: Diagrams and architectures

### Communication
- **Microsoft Teams**: Primary communication tool
- **Slack**: Customer-specific
- **Email**: Microsoft Exchange / Microsoft 365

## Development Tools

### IDEs & Editors
- **Visual Studio Code**: Primary editor
- **Visual Studio 2022**: .NET development
- **JetBrains Suite**: PyCharm, Rider (as needed)

### API Development
- **Postman**: API testing
- **Swagger / OpenAPI**: API documentation
- **Insomnia**: Alternative to Postman

## Best Practices

### Code Quality
- **SonarQube**: Code quality analysis
- **ESLint / Pylint**: Linting
- **Black / Prettier**: Code formatting
- **Unit Tests**: pytest, Jest, xUnit

### Container Best Practices
- Multi-stage builds for smaller images
- Non-root user in containers
- Vulnerability scanning with Trivy
- Image tagging according to SemVer

### Security Best Practices
- Never secrets in code
- Principle of least privilege
- Regular dependency updates
- Automated security scans in CI/CD
