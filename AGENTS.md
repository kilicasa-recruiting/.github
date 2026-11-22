# kilicasa-recruiting/.github : consignes IA

Le dépôt de profil de l'organisation de recrutement : la page publique (`profile/README.md`), le formulaire « Apply » (`.github/ISSUE_TEMPLATE/apply.yml`) et les workflows qui déclenchent l'orchestrateur. Branche vivante : `main`. **Ce dépôt est public** : rien d'interne n'y entre (adresse, secret, donnée de candidat, critère de notation).

La doc mainteneur du test vit dans `kilicasa-recruiting/recruiting-admin` (`AGENTS.md`, `README.md`, `docs/cross-repo-setup.md`). Écrit le 2026-11-22 (DX-025).

## Écosystème

Ce dépôt fait partie de l'écosystème KiliCasa. La liste des dépôts, leur rôle, leur branche vivante et le routage « quel sujet → quel dépôt → quel doc » vivent dans un seul fichier : `documentation/for-ai/ecosystem.md` du dépôt `KILICASA/KiliCasa_MVP`. Dépôts clonés côte à côte : `../KiliCasa_MVP/documentation/for-ai/ecosystem.md`. Version à jour, quelle que soit la branche du checkout : `git -C ../KiliCasa_MVP show origin/develop:documentation/for-ai/ecosystem.md`. Lis-la dès qu'une question sort de ce dépôt.

## Règles

- Réponses en français. Le contenu visible des candidats reste en anglais, comme aujourd'hui.
- Un changement du formulaire « Apply » se vérifie contre le parseur de `recruiting-admin` (`tools/apply-issue-reconcile.ts`) : ses champs sont lus par machine.
- Les workflows utilisent le secret `GH_ADMIN_TOKEN`, dont les droits sont décrits dans `recruiting-admin` (`docs/cross-repo-setup.md`).
