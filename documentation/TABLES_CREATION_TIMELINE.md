# Chronologie de Création des Tables

## Ordre de Création des Tables Airtable

### 1ère Vague - Tables Principales (10:05 - 10:15)

#### 1. `companies` - tblY1PyOUlM7cHI6e
```
Champs:
- uuid (singleLineText)
- name (singleLineText)
- domain (singleLineText)
- industry (singleLineText)
- size_category (singleSelect)
- country, city, address
- website (url)
- status (singleSelect)
- created_at, updated_at (date)
- created_by (singleLineText)
```

#### 2. `contacts` - tbllBxCON5Z8ajSVp
```
Champs:
- uuid (singleLineText)
- first_name, last_name
- email, phone
- job_title, department
- linkedin_url (url)
- company_id (singleLineText)
- status (singleSelect)
- created_at, updated_at, created_by
```

#### 3. `users` - tblFfJgWdqNvlMCDW
```
Champs:
- uuid, email
- first_name, last_name
- department_id, role_id
- avatar_url (url)
- is_active (singleSelect)
- created_at
```

#### 4. `lead_stages` - tblakD9jfz0nHJz5A
```
Champs:
- name
- order_position (number)
- department_id
- color_code
- is_active (singleSelect)
```

#### 5. `lead_activities` - tblvHCUT3ccRI3uM2
```
Champs:
- uuid
- company_id, contact_id, user_id
- type (singleSelect)
- title, description
- outcome, next_action
- scheduled_at, completed_at
- created_at
```

#### 6. `pipeline_opportunities` - tblRbCDg0Usoo5Wud
```
Champs:
- uuid
- company_id, contact_id, user_id, stage_id
- title, description
- estimated_value (currency)
- probability (number)
- source
- expected_close_date
- created_at, updated_at
```

#### 7. `finance_quotes` - tblY9fDMtXhg4yAeW
```
Champs:
- uuid
- company_id
- quote_number, title
- total_amount (currency)
- currency (singleSelect)
- status (singleSelect)
- valid_until
- created_by, created_at
```

#### 8. `finance_invoices` - tbltY9focbiaiYwFT
```
Champs:
- uuid
- company_id, quote_id
- invoice_number
- total_amount, tax_amount (currency)
- currency (singleSelect)
- status (singleSelect)
- issue_date, due_date, payment_date
- created_by
```

#### 9. `departments` - tbl1C5uScaMRZAiF8
```
Champs:
- name, description
- manager_id
- is_active (singleSelect)
```

#### 10. `kpi_definitions` - tblTj6LlI2lPfKfSp
```
Champs:
- uuid, name, description
- calculation_formula
- unit (singleSelect)
- direction (singleSelect)
- frequency (singleSelect)
- target_value (number)
- department_id
- is_active (singleSelect)
- created_by, created_at
```

#### 11. `kpi_results` - tbldZser5MSlVmiPT
```
Champs:
- kpi_definition_id
- value, target_value (number)
- achievement_rate, previous_period_value, evolution_percentage
- period_start, period_end, calculated_at
- details
- calculated_by
```

#### 12. `audit_logs` - tblRiTDSLcb7RZKHj
```
Champs:
- table_name, entity_id
- action (singleSelect)
- old_values, new_values
- user_id, ip_address
- created_at
```

### 2ème Vague - Tables Additionnelles (10:40 - 10:45)

#### 13. `roles` - tblt2JE8AXhyCJAdZ
```
Champs:
- name
- permissions (multilineText - JSON)
- department_id
- description
```

#### 14. `custom_fields` - tblCg3tdkrGR2GZT6
```
Champs:
- table_name, field_name
- field_type (singleSelect)
- options (multilineText - JSON)
- is_required (singleSelect)
- department_id
```

#### 15. `kpi_alerts` - tbllziRz4owGoeo1H
```
Champs:
- kpi_definition_id
- threshold_value (number)
- operator (singleSelect)
- alert_type (singleSelect)
- recipients (multilineText - JSON)
- is_active (singleSelect)
- message_template
- created_at
```

## Relations Principales

### Hiérarchie des Relations
1. `users` → `departments` → `roles`
2. `companies` → `contacts`
3. `companies` → `pipeline_opportunities` → `lead_stages`
4. `companies` → `lead_activities` → `users`
5. `companies` → `finance_quotes` → `finance_invoices`
6. `kpi_definitions` → `kpi_results`
7. `kpi_definitions` → `kpi_alerts`

### Clés Étrangères
- Toutes les tables utilisent des `uuid` ou `id` comme clés primaires
- Les relations sont établies via des champs `*_id` (ex: `company_id`, `user_id`)
- Les dates utilisent le format date Airtable standard

## Problèmes Rencontrés & Solutions

### 1. Champs dateTime
- **Problème** : Airtable nécessite des options spécifiques pour dateTime
- **Solution** : Utilisation de champs `date` simples

### 2. Champs checkbox
- **Problème** : is_primary pour contacts nécessitait des options spéciales
- **Solution** : Remplacement par singleSelect

### 3. Codes couleur
- **Problème** : Codes couleur invalides (gray → grayBright)
- **Solution** : Utilisation des codes approuvés par Airtable

## Sauvegarde des IDs

### IDs des Tables Principales
```
companies: tblY1PyOUlM7cHI6e
contacts: tbllBxCON5Z8ajSVp
users: tblFfJgWdqNvlMCDW
lead_stages: tblakD9jfz0nHJz5A
lead_activities: tblvHCUT3ccRI3uM2
pipeline_opportunities: tblRbCDg0Usoo5Wud
finance_quotes: tblY9fDMtXhg4yAeW
finance_invoices: tbltY9focbiaiYwFT
departments: tbl1C5uScaMRZAiF8
kpi_definitions: tblTj6LlI2lPfKfSp
kpi_results: tbldZser5MSlVmiPT
audit_logs: tblRiTDSLcb7RZKHj
roles: tblt2JE8AXhyCJAdZ
custom_fields: tblCg3tdkrGR2GZT6
kpi_alerts: tbllziRz4owGoeo1H
```

### Base Airtable ID
```
appfNvB0zFaNQcVPI
```

## Migration/Modification

Pour chaque modification future :
1. Documenter l'état actuel
2. Créer une branche git
3. Effectuer la modification
4. Documenter les changements
5. Commit avec message descriptif
6. Tag si version majeure
