# Documentation du Système de Gestion d'Entreprise

## 📊 Vue d'ensemble

Ce système de gestion centralise toutes les données de prospection, vente, finance et RH pour une exploitation optimale via des KPIs automatisés et des workflows n8n.

## 🔄 Flux de données par processus

### 1. PROSPECTION (Ajout de nouveau prospect)

```
1. Création entreprise → companies (status: "Prospect")
2. Ajout contact → contacts (company_id)
3. Première activité → lead_activities (type: "Email")
4. Création opportunité → pipeline_opportunities (stage_id: "Qualification")
```

### 2. SUIVI COMMERCIAL (Progression pipeline)

```
1. Activités régulières → lead_activities
2. Changement d'étape → pipeline_opportunities (stage_id mis à jour)
3. Proposition → finance_quotes
4. Négociation → Mise à jour probability
5. Signature → companies (status: "Client") + finance_invoices
```

### 3. FACTURATION (Conversion en client)

```
1. Devis accepté → finance_quotes (status: "Accepté")
2. Création facture → finance_invoices (quote_id)
3. Envoi → finance_invoices (status: "Envoyé", issue_date)
4. Paiement → finance_invoices (status: "Payé", payment_date)
```

### 4. CALCUL KPIs (Automatisé par n8n)

```
1. Triggers temporels → Calcul selon frequency dans kpi_definitions
2. Extraction données → Requêtes sur tables source
3. Calcul valeurs → Application des formulas
4. Stockage → kpi_results (achievement_rate, evolution_percentage)
5. Alertes → Si achievement_rate < seuil
```

## 🎯 Moments de sollicitation des tables

| Action | Tables sollicitées | Résultat |
|--------|-------------------|----------|
| Nouveau prospect | companies, contacts | Lead créé |
| Activité commerciale | lead_activities, contacts | Historique mis à jour |
| Qualification lead | pipeline_opportunities, lead_stages | Progression pipeline |
| Création devis | finance_quotes, companies | Document commercial |
| Conversion client | companies, finance_invoices | Nouveau client |
| Calcul KPI | Toutes les tables concernées | kpi_results |
| Modification data | audit_logs | Traçabilité |

## 📈 Interprétation des données

### 1. Dashboard Principal

- **Revenue**: Somme des finance_invoices (status: "Payé") par période
- **Pipeline**: Somme des pipeline_opportunities par stage_id
- **Taux conversion**: Ratio prospects → clients
- **Nouvelles leads**: Count companies créées cette semaine

### 2. Pipeline par étapes

- **Source**: pipeline_opportunities.estimated_value GROUP BY stage_id
- **Visualisation**: Barre horizontale par étape
- **Couleur**: Selon probabilité et valeur

### 3. Activités commerciales

- **Source**: lead_activities + companies + contacts
- **Filtre**: Date, status, type
- **Indicateurs**: Complété vs Planifié

### 4. Finances

- **Source**: finance_invoices + finance_quotes
- **Métriques**: 
  - DSO (Days Sales Outstanding)
  - Taux recouvrement
  - Évolution CA

### 5. Performance équipe

- **Source**: lead_activities + pipeline_opportunities + users
- **Calculs**:
  - Leads par commercial
  - Taux de conversion
  - CA généré

## 🚪 Visibilité par rôle

### Direction (Accès complet)
- Tous les KPIs
- Tableaux de bord consolidés
- Données financières complètes
- Performance équipe

### Commercial
- Pipeline personnel
- Activités assignées
- Clients prospects
- Objectifs individuels

### Finance
- Devis et factures
- Suivi des paiements
- Analyse des retards
- Reporting financier

### RH
- Données utilisateurs
- Contrats et temps
- Performance individuelle
- Gestion des départements

## 🔥 Points de vigilance

### Performance
1. Index sur company_id, user_id dans toutes les tables
2. Archivage des kpi_results > 1 an
3. Limitation des requêtes complexes

### Sécurité
1. Audit systématique (audit_logs)
2. Séparation par departments
3. Validation des permissions via users.role_id

### Maintenance
1. Purge régulière des audit_logs
2. Vérification des KPIs aberrants
3. Mise à jour des formules selon business

## 🛠️ Intégration n8n

### Workflows automatisés

1. **KPI quotidiens**
   - Trigger: Tous les jours à 6h
   - Action: Calcul et stockage kpi_results
   - Notification: Slack si objectifs non atteints

2. **Pipeline hebdomadaire**
   - Trigger: Lundi matin
   - Action: Rapport pipeline par commercial
   - Distribution: Email aux managers

3. **Facturation mensuelle**
   - Trigger: 1er du mois
   - Action: Analyse des impayés
   - Suivi: Relances automatiques

4. **Qualification prospects**
   - Trigger: Nouveau lead_activity
   - Action: Score de qualification
   - Résultat: Mise à jour pipeline_opportunities

## ⚡ Performance attendue

| Métrique | Objectif | Source |
|----------|----------|---------|
| Temps de réponse | < 2s | Optimisation requêtes |
| Taux de conversion | 25% | pipeline_opportunities |
| DSO | < 30 jours | finance_invoices |
| Satisfaction client | > 4/5 | Enquêtes externes |

## 📋 Prochaines évolutions

1. **Intégrations**
   - CRM externe
   - Comptabilité (Sage, QuickBooks)
   - Marketing (HubSpot, Mailchimp)

2. **Intelligence**
   - Prédiction de conversion (ML)
   - Scoring automatique des leads
   - Alertes prédictives sur pipeline

3. **Mobilité**
   - API REST complète
   - Application mobile
   - Notifications push

4. **Reporting avancé**
   - Dashboards temps réel
   - Analyse comparative
   - Benchmarks sectoriels
