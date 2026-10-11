# Ordre de mission — TEST-ORCH-001 (11/10/2026)

## But du test
Tester **trois véritables sous-agents Codex**, visibles dans des fils distincts, sans API OpenAI payante. Livrer un mini Space Invaders inspiré du genre (aucune ressource copiée) dont le gameplay est en **C++**, empaqueté en **APK Android arm64-v8a**. En complément, produire un AAB si la chaîne de compilation le permet sans compliquer le test.

**Dépôt :** https://github.com/greenpower2669/TestGpOrchestrator  
**Branche autorisée :** `experiment/codex-3-agents-space-invaders`  
**Interdit :** écrire/merger directement sur `main`, publier une release, utiliser une clé API OpenAI ou engager une dépense payante. Aucune action sur les autres dépôts.

## Instruction initiale au chat maître
**Crée exactement 3 sous-agents Codex distincts** et signale leurs noms/fils. Ne simule pas une délégation par trois étapes exécutées seul. Si la création de sous-agents n'est pas prise en charge dans le client ou la session actuelle, déclare `BLOQUÉ — multi-agents indisponible` et n'invente aucun résultat.

Dès le départ, distribue les périmètres non concurrents. Laisse les fils spécialisés conserver leurs logs ; ne remonte au fil principal que les statuts et liens vers les fichiers. Les dépendances entre agents nécessitent un point de synchronisation, pas un travail simultané sur les mêmes fichiers.

### Agent 1 — ARTISTE
Périmètre exclusif : `assets/` et, si absent, `brain.md` (partie ressources uniquement).
- Créer **réellement** un sprite de vaisseau joueur, un sprite d'envahisseur et un fond spatial (PNG), plus une icône d'application PNG 512×512.
- Utiliser les outils locaux disponibles : génération SVG puis conversion PNG, Pillow ou dessin programmatique. Pas d'appel à une API d'images payante.
- Fournir les dimensions, chemins et aperçu si le client le permet.
- Faire un commit uniquement de son périmètre.
- Retour : `VERT|ORANGE|ROUGE — 1 phrase — chemins ou commit`.

### Agent 2 — DÉVELOPPEUR C++
Périmètre exclusif : `src/`, `android/`, `CMakeLists.txt` et fichiers nécessaires au jeu, plus `todo.md` (suivi gameplay).
- Créer un mini Space Invaders original en C++17, rendu via **SDL3**, avec petite enveloppe Android standard si nécessaire.
- Joueur en bas, déplacements tactiles, tirs, rangées d'ennemis, collisions, score, victoire/défaite et recommencer.
- Préparer d'abord le moteur avec des textures temporaires, sans modifier `assets/`.
- Dès feu vert de l'agent 1, intégrer ses PNG sans les réécrire.
- Prévoir construction Android SDK/NDK/Gradle et ABI `arm64-v8a`.
- Fournir commandes de compilation précises. Un simple exécutable desktop n'est pas une réussite APK.
- Faire un commit uniquement de son périmètre.
- Retour : `VERT|ORANGE|ROUGE — 1 phrase — chemins ou commit`.

### Agent 3 — INTÉGRATION, CI ET REMONTÉE
Périmètre exclusif : `.github/workflows/`, `tests/` et, si absent, `debughistorical.md` (faits techniques constatés).
- Préparer GitHub Actions pour compiler le jeu Android avec SDK/NDK, JDK et Gradle compatibles. Utiliser une version traçable de SDL3.
- Attendre les sources intégrées avant le build définitif.
- Exécuter la compilation ou exploiter le CI; analyser erreurs et faire corriger l'agent responsable sans modifier ses fichiers.
- Publier l'APK **comme artefact de workflow**, signé avec la clé de débogage seulement; produire aussi un AAB si faisable.
- Nom de l'APK : `TestGpOrchestrator-v0.1.0-debug-arm64-v8a.apk`. Nom de l'AAB si présent : `TestGpOrchestrator-v0.1.0.aab`.
- Vérifier le fichier effectivement généré et son lien; ne jamais annoncer une compilation réussie sans preuve.
- Faire un commit uniquement de son périmètre.
- Retour : `VERT|ORANGE|ROUGE — 1 phrase — URL de run et artefact, ou erreur`.

## Mémoires vivantes — propriété des écritures
Le projet utilise cinq fichiers légers : `brain.md`, `brainmap.md`, `debughistorical.md`, `todo.md`, `ordres-de-mission.md`.
- L'agent 1 met à jour seulement les conventions d'assets de `brain.md`.
- L'agent 2 tient `todo.md` à jour pour le gameplay.
- L'agent 3 consigne les constats confirmés dans `debughistorical.md`.
- Le maître maintient **uniquement** une carte `brainmap.md` très courte et, au besoin, cette mission; aucune recopie des journaux complets.
- Ne pas écraser les mémoires existantes ; les créer si absentes. Si un conflit d'édition survient, interrompre l'écriture conflictuelle et coordonner.

## Barrières de synchronisation
1. Lancer les 3 sous-agents indépendants : graphismes, squelette gameplay, préparation CI.
2. Attendre graphismes validés → demander au développeur d'intégrer les PNG.
3. Attendre intégration → lancer build/test CI.
4. Si échec, renvoyer le diagnostic à l'agent compétent; **pas** de réparation simultanée du même fichier.
5. Attendre les 3 bilans ; le maître restitue **seulement** le tableau ci-dessous, un lien vers la branche, un lien vers l'artefact si réel, et les décisions humaines requises.

## Rapport final du maître
```
Agent 1 — Image : VERT / ORANGE / ROUGE — lien
Agent 2 — C++ : VERT / ORANGE / ROUGE — lien
Agent 3 — APK/CI : VERT / ORANGE / ROUGE — lien
Sous-agents réellement créés : OUI / NON (fils ou identifiants)
APK arm64-v8a généré : OUI / NON (preuve et lien)
AAB généré : OUI / NON (preuve et lien)
Branche de travail : lien
Point bloquant / validation demandée : ...
```
**Feu vert uniquement après preuve du build.** Aucun merge dans `main` sans autorisation explicite.
