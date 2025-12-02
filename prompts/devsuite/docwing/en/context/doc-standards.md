# Documentation Standards

## Documentation Formats

### Markdown
- **GitHub-Flavored Markdown (GFM)**: Standard for repository documentation
- **CommonMark**: Cross-platform compatibility
- **MDX**: Markdown with React components (Docusaurus)

### Other Formats
- **AsciiDoc**: For complex technical books and documentation
- **reStructuredText**: Python ecosystem, Sphinx
- **OpenAPI 3.x**: API specifications

## Documentation Types

### API Documentation
- **Purpose**: Help developers understand and use APIs
- **Contents**: Endpoints, parameters, responses, examples
- **Tools**: Swagger UI, Redoc, Stoplight

### User Manuals
- **Purpose**: Support end users in product usage
- **Contents**: Step-by-step instructions, screenshots, FAQs
- **Style**: Simple language, avoids technical jargon

### Reference Documentation
- **Purpose**: Quick lookup of details
- **Contents**: Configuration options, CLI commands, parameters
- **Structure**: Alphabetically or thematically organized

### Concept Documentation
- **Purpose**: Understanding of architecture and design
- **Contents**: Overviews, diagrams, design decisions
- **Audience**: Technical stakeholders, new team members

### Runbooks
- **Purpose**: Operations guides for recurring tasks
- **Contents**: Step-by-step procedures, troubleshooting
- **Requirements**: Always current, tested, complete

## Documentation Tools

### Static Site Generators

#### MkDocs
- **Language**: Python
- **Configuration**: `mkdocs.yml`
- **Theme**: Material for MkDocs (recommended)
- **Features**: Navigation, search, versioning

#### Docusaurus
- **Language**: JavaScript/React
- **Features**: MDX support, i18n, versioning
- **Ideal for**: Open-source projects, product documentation

#### Hugo
- **Language**: Go
- **Features**: Fast, flexible, many themes
- **Ideal for**: Blogs, project sites

### API Documentation

#### Swagger/OpenAPI
- **Standard**: OpenAPI 3.x
- **Tools**: Swagger UI, Swagger Editor
- **Integration**: Generation from code possible

#### Redoc
- **Features**: Responsive design, no dependencies
- **Ideal for**: Static API documentation

#### Stoplight
- **Features**: Design-first approach, mock server
- **Ideal for**: API development teams

## Docs-as-Code

### Principles
- Documentation in the same repository as code
- Version control with Git
- Reviews through pull requests
- CI/CD for automatic deployment

### Benefits
- Documentation stays current
- Developers can easily contribute
- Change tracking and rollback
- Automated quality checks

### Workflow
1. Create change in branch
2. Open pull request
3. Review by team members
4. Merge and automatic deployment
5. Notification on changes

## Diagrams

### Mermaid
- **Integration**: GitHub, GitLab, MkDocs
- **Types**: Flowcharts, sequence diagrams, ER diagrams
- **Syntax**: Text-based in Markdown

### PlantUML
- **Types**: UML diagrams, architecture
- **Integration**: IDE plugins, CI/CD
- **Syntax**: Text-based

### Draw.io
- **Type**: WYSIWYG editor
- **Export**: SVG, PNG, PDF
- **Ideal for**: Complex architecture diagrams

## Best Practices

### Structure
- Flat hierarchy (max 3 levels)
- Consistent file names
- `README.md` as entry point
- `docs/` folder for documentation

### Maintenance
- Regular reviews
- Define responsibilities
- Archive or delete outdated content
- Establish feedback mechanisms

### Quality
- Spell checking
- Link checker
- Consistency check for terminology
- Automated tests for code examples
