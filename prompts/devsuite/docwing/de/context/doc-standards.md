# Dokumentationsstandards

## Dokumentationsformate

### Markdown
- **GitHub-Flavored Markdown (GFM)**: Standard für Repository-Dokumentation
- **CommonMark**: Plattformübergreifende Kompatibilität
- **MDX**: Markdown mit React-Komponenten (Docusaurus)

### Weitere Formate
- **AsciiDoc**: Für komplexe technische Bücher und Dokumentation
- **reStructuredText**: Python-Ökosystem, Sphinx
- **OpenAPI 3.x**: API-Spezifikationen

## Dokumentationstypen

### API-Dokumentation
- **Zweck**: Entwickler verstehen und nutzen APIs
- **Inhalte**: Endpunkte, Parameter, Responses, Beispiele
- **Tools**: Swagger UI, Redoc, Stoplight

### Benutzerhandbücher
- **Zweck**: Endnutzer bei der Produktnutzung unterstützen
- **Inhalte**: Schritt-für-Schritt-Anleitungen, Screenshots, FAQs
- **Stil**: Einfache Sprache, vermeidet Fachjargon

### Referenzdokumentation
- **Zweck**: Schnelles Nachschlagen von Details
- **Inhalte**: Konfigurationsoptionen, CLI-Befehle, Parameter
- **Struktur**: Alphabetisch oder thematisch geordnet

### Konzeptdokumentation
- **Zweck**: Verständnis von Architektur und Design
- **Inhalte**: Übersichten, Diagramme, Designentscheidungen
- **Zielgruppe**: Technische Stakeholder, neue Teammitglieder

### Runbooks
- **Zweck**: Betriebsanleitungen für wiederkehrende Aufgaben
- **Inhalte**: Schritt-für-Schritt-Prozeduren, Troubleshooting
- **Anforderungen**: Immer aktuell, getestet, vollständig

## Dokumentationstools

### Static Site Generators

#### MkDocs
- **Sprache**: Python
- **Konfiguration**: `mkdocs.yml`
- **Theme**: Material for MkDocs (empfohlen)
- **Features**: Navigation, Suche, Versionierung

#### Docusaurus
- **Sprache**: JavaScript/React
- **Features**: MDX-Support, i18n, Versionierung
- **Ideal für**: Open-Source-Projekte, Produktdokumentation

#### Hugo
- **Sprache**: Go
- **Features**: Schnell, flexibel, viele Themes
- **Ideal für**: Blogs, Projektseiten

### API-Dokumentation

#### Swagger/OpenAPI
- **Standard**: OpenAPI 3.x
- **Tools**: Swagger UI, Swagger Editor
- **Integration**: Generierung aus Code möglich

#### Redoc
- **Features**: Responsives Design, keine Abhängigkeiten
- **Ideal für**: Statische API-Dokumentation

#### Stoplight
- **Features**: Design-First-Ansatz, Mock-Server
- **Ideal für**: API-Entwicklungsteams

## Docs-as-Code

### Prinzipien
- Dokumentation im gleichen Repository wie Code
- Versionskontrolle mit Git
- Reviews durch Pull Requests
- CI/CD für automatisches Deployment

### Vorteile
- Dokumentation bleibt aktuell
- Entwickler können leicht beitragen
- Änderungsverfolgung und Rollback
- Automatisierte Qualitätsprüfungen

### Workflow
1. Änderung in Branch erstellen
2. Pull Request öffnen
3. Review durch Teammitglieder
4. Merge und automatisches Deployment
5. Benachrichtigung bei Änderungen

## Diagramme

### Mermaid
- **Integration**: GitHub, GitLab, MkDocs
- **Typen**: Flowcharts, Sequenzdiagramme, ER-Diagramme
- **Syntax**: Textbasiert im Markdown

### PlantUML
- **Typen**: UML-Diagramme, Architektur
- **Integration**: IDE-Plugins, CI/CD
- **Syntax**: Textbasiert

### Draw.io
- **Typ**: WYSIWYG-Editor
- **Export**: SVG, PNG, PDF
- **Ideal für**: Komplexe Architekturdiagramme

## Best Practices

### Struktur
- Flache Hierarchie (max. 3 Ebenen)
- Konsistente Dateinamen
- `README.md` als Einstiegspunkt
- `docs/` Ordner für Dokumentation

### Wartung
- Regelmäßige Reviews
- Verantwortlichkeiten festlegen
- Veraltete Inhalte archivieren oder löschen
- Feedback-Mechanismen einrichten

### Qualität
- Rechtschreibprüfung
- Link-Checker
- Konsistenzprüfung für Terminologie
- Automatisierte Tests für Code-Beispiele
