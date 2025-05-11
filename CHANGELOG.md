# Changelog - Gestion Entreprise

## Version 2.0.0 - TPE/PME Focus (2025-05-11)

### Added
- Système de scoring inversé favorisant TPE/PME
- Intégration API Pappers pour scraping du CA
- Fallback vers Societe.com en cas d'échec
- Analyse automatique de l'urgence basée sur l'évolution du CA
- Champs supplémentaires : `last_ca`, `ca_evolution`, `urgency`
- Assignment intelligent selon urgency (commercial senior pour leads urgents)
- Templates d'emails personnalisés selon l'analyse financière

### Changed
- Score TPE: 5 → 30 points (priorité maximale)
- Score PME: 15 → 25 points
- Score ETI: 20 → 15 points  
- Score GE: 30 → 5 points
- Logique de priorisation basée sur CA et tendances

### Technical
- Workflow `lead-scoring-workflow-advanced.json` créé
- Gestion des erreurs API améliorée
- Structure de données étoffée

## Version 1.1.0 - Corrections Workflow (2025-05-11)

### Fixed
- Syntaxe des template literals dans JSON
- Structure des données Airtable (`items[0].json.fields`)
- Paramètres Airtable (`filters` → `filterByFormula`)
- Gestion des IDs (UUID vs recordId)
- Gestion d'erreurs manquante
- Formatage inconsistant des dates

### Technical
- Workflow `lead-scoring-workflow-fixed.json` créé

## Version 1.0.0 - Architecture Initiale (2025-05-11)

### Added
- 12 tables dans Airtable :
  - companies, contacts, users
  - lead_stages, lead_activities, pipeline_opportunities
  - finance_quotes, finance_invoices
  - departments, kpi_definitions, kpi_results
  - audit_logs
- 3 workflows n8n :
  - Lead Scoring & Assignment
  - Pipeline Health Monitor
  - Client Success Automation
- Documentation complète :
  - CRM_ORCHESTRATION.md
  - SYSTEM_DOCUMENTATION.md
  - database-relations.svg
  - dashboard-mockup.svg

### Architecture
- Base de données structurée avec séparation par départements
- Système de KPIs automatisé
- Traçabilité complète des actions
- Orchestration n8n pour l'automatisation
