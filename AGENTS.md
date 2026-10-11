# AGENTS.md — protocole TestGpOrchestrator

Lire d'abord `ordres-de-mission.md`. Cette expérience mesure la **vraie orchestration de trois sous-agents**, pas seulement la qualité du mini-jeu.

- Démarrer exactement trois sous-agents spécialisés (artiste, C++/Android, CI/contrôle); rendre leurs fils/identifiants visibles et ne jamais feindre qu'un sous-agent existe.
- Coordination par le maître, écriture dans des périmètres séparés. Agent 1 : `assets/` + partie assets de `brain.md`. Agent 2 : `src/`, `android/`, `CMakeLists.txt`, `todo.md`. Agent 3 : `.github/workflows/`, `tests/`, `debughistorical.md`.
- Les graphismes sont générés localement par l'agent 1. Gameplay C++17/SDL3. Build Android arm64-v8a via SDK/NDK/Gradle. Artefact debug APK; AAB facultatif.
- Travailler uniquement sur `experiment/codex-3-agents-space-invaders`. Ne jamais merger `main` ou créer de release sans validation. Aucune clé API OpenAI ni achat de crédits.
- Les 5 mémoires vivantes sont légères et spécifiques à ce dépôt. `brainmap.md` est la carte d'entrée. Ne jamais recopier les journaux techniques dans le chat maître.
- Renvoyer seulement un feu VERT/ORANGE/ROUGE, une phrase et un chemin/commit/URL par agent.
- Ne déclarer un APK disponible que si le build a réellement produit l'artefact.
