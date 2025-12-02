# ITGlue Plattform-Kontext

## Plattformübersicht

ITGlue ist Kaseyas führende IT-Dokumentationsplattform für Managed Service Provider (MSPs). Die Plattform dient als zentrale Wissensdatenbank ("Single Source of Truth") für alle IT-bezogenen Dokumentationen.

## Kernkonzepte

### Organisationsstruktur
- **Organizations**: Hauptcontainer für Kundendaten (entspricht Kundenunternehmen)
- **Sub-Organizations**: Hierarchische Unterorganisationen für komplexe Strukturen
- **Primary Organization**: Die eigene MSP-Organisation

### Core Assets (Kerndaten)
| Asset-Typ | Beschreibung | Typische Inhalte |
|-----------|--------------|------------------|
| **Configurations** | IT-Geräte und Systeme | Server, Workstations, Netzwerkgeräte, Drucker |
| **Contacts** | Ansprechpartner | Name, E-Mail, Telefon, Rolle, Standort |
| **Passwords** | Zugangsdaten | Benutzername, Passwort, URL, OTP, Notizen |
| **Locations** | Standorte | Adressen, Gebäudeinformationen |
| **Domains** | Domänen | Registrar, Ablaufdatum, DNS-Einträge |
| **SSL Certificates** | SSL-Zertifikate | Zertifikatsdaten, Ablaufdatum |
| **Documents** | Dokumente | Freitext-Dokumentation, Anleitungen, SOPs |

### Flexible Assets
Benutzerdefinierte Dokumentationsvorlagen für strukturierte Informationen:
- **Applications**: Software und deren Konfiguration
- **LAN**: Netzwerkinfrastruktur
- **Backup**: Backup-Lösungen und -Pläne
- **Licensing**: Lizenzverwaltung
- **Eigene Templates**: Frei erstellbare Strukturen

### Spezielle Features
- **Checklists/Runbooks**: Automatisierte Dokumentationsworkflows
- **Quick Notes**: Schnellnotizen auf Organisationsebene
- **Related Items**: Verknüpfungen zwischen Assets
- **Tags**: Kategorisierung und Filterung
- **Embedded Passwords**: In Assets eingebettete Zugangsdaten
- **GlueFiles**: Dateianhänge

## Network Glue
Automatische Netzwerkdokumentation durch Agenten:
- Active Directory Integration
- Netzwerktopologie-Mapping
- Geräte-Autodiscovery
- Configuration Auto-Matching
- Password Rotation für AD-Konten

## MyGlue
Kundenportal für eingeschränkten Zugriff:
- Kunden können eigene Passwörter und Dokumente verwalten
- Eingeschränkte Sichtbarkeit basierend auf Berechtigungen
- Chrome-Extension für Autofill

## Integrationen

### PSA-Integrationen
- **Autotask**: Bidirektionale Synchronisation
- **ConnectWise Manage**: Organisations- und Ticket-Sync
- **Kaseya BMS**: Native Integration
- **Tigerpaw**: Konfigurationssync
- **Pulseway PSA**: Zwei-Wege-Sync

### RMM-Integrationen
- **Datto RMM**: Geräte- und Konfigurationssync
- **VSA 10**: Native Kaseya-Integration
- **Auvik**: Netzwerkgeräte-Matching

### Microsoft-Integrationen
- **Microsoft 365**: Benutzer- und Lizenzsync
- **Microsoft GDAP**: Tenant-Management
- **Azure AD/Entra ID**: Identitätssynchronisation

## API & Automation
- RESTful API für programmatischen Zugriff
- Webhook-Unterstützung für Ereignisbenachrichtigungen
- Export/Import-Funktionen (CSV, JSON)

## Sicherheitsfeatures
- **Vault**: Zusätzliche Verschlüsselungsebene für sensible Passwörter
- **IP Access Control**: Zugriffsbeschränkung nach IP-Adresse
- **SSO/SAML**: Single Sign-On Integration
- **MFA**: Multi-Faktor-Authentifizierung
- **Audit Logs**: Vollständige Aktivitätsprotokollierung
- **Revisions**: Versionskontrolle für alle Assets

## Best Practices Terminologie
- **SOPs** (Standard Operating Procedures): Standardisierte Arbeitsanweisungen
- **Runbooks**: Automatisierte Checklisten und Workflows
- **Documentation Standards**: Konsistente Dokumentationsrichtlinien
- **Relationship Mapping**: Verknüpfung zusammengehöriger Assets
