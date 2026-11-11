# Changelog

Toutes les évolutions notables du dépôt `its-dp-fab-ucs-mybsc-snow-managed` (configuration de déploiement,
pipelines, outillage, documentation) sont consignées ici, de la plus récente à la plus ancienne.

Format : [Keep a Changelog](https://keepachangelog.com/fr/1.1.0/). Ce dépôt n'est pas versionné
par tag : chaque entrée est datée (`## AAAA-MM-JJ`). Les évolutions du socle commun sont décrites
dans le `CHANGELOG.md` de `its-dp-fab-cicd-common`, versionné par tag `vX.Y.Z`.

## 2026-11-11

### Added

- Initialisation de la chaîne CI/CD du dépôt sur le socle `its-dp-fab-cicd-common` (tag `v1.5.0`,
  catégorie `ucs`), à partir des fichiers du dépôt UCS Reporting Fees & WIP (référence de forme,
  harmonisée avec le dépôt UCS Referentiel) :
  - `CICD/pipelines/CI.yml` : template KPMG ; SonarQube `Discovery_DP-FAB-UCS-MyBSC/SNOW-Managed`
    (« [Discovery] DP FAB UCS MyBSC / SNOW Managed », clé reprise de l'ancienne CI ; point V-02) ;
    déclencheurs `develop`, `release/*`, `main`.
  - `CICD/pipelines/CD.yml` : `repository: its-dp-fab-ucs-mybsc-snow-managed`, `role: consumer`,
    source CI `its-dp-fab-ucs-mybsc-snow-managed-ci` (reprise de l'ancienne CD), `frameworkGate: "off"`
    (aucune dépendance de déploiement, R5), coffre `frfrkvdevdp0001`, 3 service connections ARM
    en marqueurs `__A_CONFIRMER_SERVICE_CONNECTION_ARM_<ENV>__`. Remplace l'ancienne CD
    (fabric-cicd 1.2.0, `deployment/deploy.py` avec workspaces, capacités, lakehouses et SPN en dur,
    ITG -> PPR -> PRD sans approbation, workspaces cibles de UCS Referentiel par erreur, chemin
    PPR erroné).
  - `CICD/pipelines/PR-Validation.yml` : en-tête UCS MyBSC / SNOW.
  - `CICD/deployment/config.yml` : workspaces `FR - DP - <ENV> - FAB - UCS - MyBSC / SNOW - Managed`
    pour DEV, ITG, PPR et PRD ; `item_types_in_scope` vide.
  - `CICD/deployment/dependencies.yml` : `role: consumer`, `category: ucs`, `framework.required: false` ;
    `guid_policy.forbidden_prefixes` avec les workspaces MyBSC / SNOW DEV (`d7578bcf-`), ITG
    (`dd8af3a7-`) et PRD (`852072cf-`), et les workspaces UCS Referentiel ITG, PPR et PRD visés par
    l'ancien `deploy.py`, tous absents du registre du socle v1.5.0.
  - `CICD/deployment/parameter.yml` : aucune règle (à construire à l'inventaire).
  - `CICD/ref_dynamique/01_inventaire_github.py` à `04_recoupement.py` : outillage d'inventaire ;
    `01` classe aussi les GUID avec les préfixes de `guid_policy` ; `02` vérifie un nom de
    workspace UCS MyBSC / SNOW.
  - `CICD/ref_dynamique/templates/config.yml` et `parameter.yml` : modèles au format du socle.
  - `.gitattributes` et `.gitignore` identiques aux autres dépôts consommateurs.
  - `fabric/.gitkeep` (remplace `fabric/test.md`, contenu « test », référencé nulle part).
  - README.

### Vérifié

- `yamllint` (configuration du socle) sur `CICD/deployment` et `CICD/pipelines` ; compilation des scripts.
- `deploy.py validate` du socle : code 4 sur le dossier `fabric` vide (attendu) ; 0 erreur avec un
  notebook fictif ; `GUID_HARDCODED` levé pour un ID de workspace UCS MyBSC / SNOW en dur.

### À confirmer

- Noms des 3 service connections ARM, service connection GitHub et organisation GitHub.
- Inventaire (`data_dependencies`, `connections`, `variable_libraries`, règles `parameter.yml`)
  après branchement de la Git Integration DEV sur `fabric`.
- Approbations des Environments ADO `fabric-ppr` et `fabric-prd`.
- ID du workspace PPR MyBSC / SNOW : la liste des workspaces lui donne l'ID de DEV (`d7578bcf-`).
- Point V-02 : clé SonarQube contenant « / ».
- Ajout des workspaces UCS MyBSC / SNOW au registre du socle au prochain tag.
