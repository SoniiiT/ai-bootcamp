# Regel 1 - ITGlue Dokumentationsstandards

## Dokumentationsstruktur in ITGlue

### Organisation der Dokumentation
Dokumentation in ITGlue folgt einer hierarchischen Struktur:

```
Organization
├── Documents (Freitext-Dokumentation)
│   ├── Ordner/Folders
│   │   └── Unterordner
│   └── Einzelne Dokumente
├── Core Assets (Strukturierte Daten)
│   ├── Configurations
│   ├── Contacts
│   ├── Passwords
│   ├── Locations
│   ├── Domains
│   └── SSL Certificates
└── Flexible Assets (Benutzerdefiniert)
    ├── Applications
    ├── LAN
    └── [Eigene Templates]
```

### Best Practices für Dokumentstruktur

**Ordnerstruktur:**
```markdown
📁 Onboarding
📁 Standard Operating Procedures (SOPs)
📁 Netzwerk & Infrastruktur
📁 Anwendungen & Software
📁 Sicherheit & Compliance
📁 Notfall & Recovery
📁 Kundenspezifisch
```

**Konsistente Benennung:**
- `[Kundenname] - [Thema] - [Version/Datum]`
- Beispiel: `Mustermann GmbH - VPN Einrichtung - v2.0`

## Dokumenttypen in ITGlue

### 1. Freitext-Dokumente
**Verwendung:** Anleitungen, SOPs, Runbooks, Prozessbeschreibungen

**Struktur:**
```markdown
# Dokumenttitel

## Übersicht
Kurzbeschreibung des Dokuments

## Voraussetzungen
- Voraussetzung 1
- Voraussetzung 2

## Schritt-für-Schritt-Anleitung
1. Erster Schritt
2. Zweiter Schritt

## Fehlerbehebung
### Problem 1
Lösung...

## Verwandte Dokumentation
- [Link zu Related Item]
```

### 2. Flexible Asset Dokumentation
**Verwendung:** Strukturierte, wiederholbare Informationen

**Beispiel Application-Template:**
```
Name: [Anwendungsname]
Hersteller: [Vendor]
Version: [Aktuelle Version]
Lizenztyp: [Subscription/Perpetual]
Server: [Tag zu Configuration]
Verantwortlicher Kontakt: [Tag zu Contact]
Dokumentation URL: [Externe Dokumentation]
Notizen: [Freitext]
```

### 3. Quick Notes
**Verwendung:** Schnelle, organisationsbezogene Notizen

**Format:**
```markdown
## [Datum] - [Kurztitel]
[Inhalt der Notiz]

Erstellt von: [Username]
```

## Relationship Mapping

### Verknüpfungen erstellen
Nutze **Related Items** um Zusammenhänge zu dokumentieren:

| Von | Zu | Beschreibung |
|-----|-----|--------------|
| Server (Configuration) | Anwendung (Flexible Asset) | "Hostet diese Anwendung" |
| Passwort | Configuration | "Zugang zu diesem System" |
| Dokument | Configuration | "Dokumentiert dieses System" |
| Contact | Application | "Verantwortlich für" |

### Best Practice Verknüpfungen
```markdown
Server → Passwörter (Admin-Zugänge)
Server → Anwendungen (Gehostete Apps)
Server → Backup (Backup-Konfiguration)
Anwendung → Kontakte (Support/Verantwortliche)
Netzwerk → Configurations (Geräte im Netzwerk)
```

## Passwort-Dokumentation

### Embedded vs. General Passwords

**Embedded Passwords verwenden für:**
- Gerätespezifische Zugänge (1:1 Beziehung)
- Lokale Admin-Konten
- Geräte-spezifische Web-Interfaces

**General Passwords verwenden für:**
- Mehrfach verwendete Zugänge
- Domain-Admin-Konten
- SaaS-Anwendungen
- Registrar/DNS-Zugänge

### Passwort-Struktur
```markdown
Name: [Aussagekräftiger Name]
Username: [Benutzername]
Password: [Generiert/Manuell]
URL: [Zugangs-URL]
Kategorie: [Passend wählen]
Notizen: [Zusätzliche Informationen]
OTP: [Falls 2FA aktiv]
```

## Versionierung und Revisions

### Änderungsprotokoll im Dokument
```markdown
## Änderungshistorie

| Version | Datum | Autor | Änderung |
|---------|-------|-------|----------|
| 1.0 | 2024-01-15 | Max Mustermann | Erstversion |
| 1.1 | 2024-02-20 | Anna Schmidt | Kapitel 3 aktualisiert |
| 2.0 | 2024-03-10 | Max Mustermann | Komplette Überarbeitung |
```

### ITGlue Revisions nutzen
- Revisions werden automatisch bei jedem Speichern erstellt
- Über "Revisions" Panel frühere Versionen einsehen
- Bei Bedarf auf frühere Version zurücksetzen
