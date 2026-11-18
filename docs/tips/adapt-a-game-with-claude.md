# Adapt a game with Claude

[Version française](#adapter-un-jeu-avec-claude)

The [board game template](https://github.com/gamepark/board-game-template) includes a skill that teaches [Claude Code](https://claude.com/claude-code) how we adapt games. Run these prompts in order, and use `/clear` between them to consume fewer tokens.

1. "Rename the template to [name of the game]"
2. "Get the rules of the game in French and English from [folder], and optimize them for the web"
3. "Prepare the game images from the files in [folder]"
4. "Use these images for the cover, the box and the icon in /public: [files]"
5. "Code the game setup and its display" => iterate until the table looks right
6. "Code the rules of the game"
7. "Code the headers and the buttons" => iterate on each phase of the game
8. "Code the help dialogs"
9. "Code a strong bot" => play against it, then optimize
10. "Suggest a tutorial script like https://github.com/gamepark/paper-tales/blob/main/script-tuto.txt" => review and refine it
11. "Code the tutorial"
12. "Translate the game into the 5 other languages"
13. "Check the game against the checklist": see the [Checklist](step-by-step-example/checklist.md)

# Adapter un jeu avec Claude

Le [template de jeu](https://github.com/gamepark/board-game-template) inclut un skill qui apprend à [Claude Code](https://claude.com/claude-code) comment nous adaptons les jeux. Lance ces prompts dans l'ordre, avec `/clear` entre chacun pour consommer moins de tokens.

1. « Renomme le template en [nom du jeu] »
2. « Récupère les règles du jeu en FR et EN ici : [dossier], et optimise-les pour le web »
3. « Prépare les images du jeu à partir des fichiers ici : [dossier] »
4. « Utilise ces images pour la cover, la boîte et l'icône dans /public : [fichiers] »
5. « Code le setup du jeu et son affichage » => itère jusqu'à obtenir l'affichage voulu
6. « Code les règles du jeu »
7. « Code les headers et les boutons » => itère sur chaque phase de jeu
8. « Code les popups d'aide du jeu »
9. « Code un bot de bon niveau » => joue contre lui, puis optimise
10. « Propose-moi un scénario de tutoriel comme https://github.com/gamepark/paper-tales/blob/main/script-tuto.txt » => relis et peaufine
11. « Code le tutoriel »
12. « Traduis le jeu dans les 5 autres langues »
13. « Vérifie le jeu avec la checklist » : voir la [Checklist](step-by-step-example/checklist.md)
