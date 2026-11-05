# Orchestrateur ou intégration continue ? (TP06)

| Critère                           | Prefect (orchestrateur)                                                                 | GitHub Actions (CI)                                                 |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ce qui déclenche l'exécution      | Une planification (cron, lundi 6 h), un Quick run depuis l'interface                    | Un événement Git (git push) ou le bouton Run workflow                |
| Où s'exécute le traitement        | Sur mon poste, par un processus (serve ou worker du work pool)                          | Sur une machine Linux de GitHub (ubuntu-latest)                      |
| Qui surveille, et où              | L'équipe, dans l'interface Prefect (Runs, journaux de chaque tâche)                     | L'équipe, dans l'onglet Actions de GitHub                            |
| Ce qui se passe en cas d'échec    | Deux réessais sur la tâche pipeline, puis run Failed : valider échoue, notifier ne part pas | Étape rouge (code de sortie 1), commit marqué en rouge sur GitHub    |
| Type de tâche adapté              | Traitements récurrents (réentraînement hebdomadaire)                                    | Vérification du code à chaque modification                           |
| Ce qui est versionné dans Git     | flow_mlops.py, planifier.py, prefect.yaml                                               | .github/workflows/ci.yml, verifier_qualite.py, requirements-ci.txt   |

## Conclusion (5 lignes)

L'orchestrateur Prefect exécute et surveille des traitements récurrents, comme le réentraînement du lundi à 6 h.
La chaîne GitHub Actions valide le code à chaque modification, avant qu'elle atteigne le pipeline de production.
Dans les deux cas, la même règle de qualité (seuil_f1 de params.yaml) arrête le processus quand le F1 est trop bas.
Prefect montre les tâches une par une dans une interface, GitHub Actions donne un verdict vert ou rouge par commit.
Les deux sont complémentaires : aucun ne remplace l'autre.