# Prise en main de Visual Studio Community pour le C++
 
**Prérequis :** avoir des notions de base en C++ (syntaxe, compilation)
 
---
 
## Objectifs du cours
 
À la fin de ce cours, vous serez capable de :
 
- Naviguer dans l'interface de Visual Studio Community
- Créer, organiser et compiler un projet C++
- Utiliser le débogueur pour trouver des bugs
- **Lier une bibliothèque externe (.lib / .dll) à votre projet**
- Comprendre et résoudre les erreurs de link les plus courantes
---
 
## 1. Qu'est-ce que Visual Studio Community ?
 
Visual Studio Community est un **IDE** (Integrated Development Environment / Environnement de Développement Intégré) gratuit édité par Microsoft. Il regroupe en un seul logiciel :
 
- Un éditeur de code (avec coloration syntaxique, autocomplétion...)
- Un compilateur C++ (MSVC)
- Un débogueur intégré
- Un gestionnaire de projets/solutions
⚠️ Ne pas confondre avec **Visual Studio Code** (VS Code), qui est un simple éditeur de texte léger, sans compilateur intégré. Ici on parle bien de **Visual Studio Community**, la version complète.
 
---
 
## 2. Installation (rappel rapide)
 
Si ce n'est pas déjà fait :
 
1. Télécharger l'installeur sur [visualstudio.microsoft.com](https://visualstudio.microsoft.com/fr/vs/community/)
2. Dans le programme d'installation, cocher la charge de travail :
   **« Développement Desktop en C++ »**
3. Lancer l'installation (plusieurs Go, prévoir du temps)
Sans cette charge de travail, le compilateur C++ n'est pas installé et vous ne pourrez pas créer de projet C++.
 
---
 
## 3. Créer un premier projet C++
 
1. Ouvrir Visual Studio
2. Cliquer sur **« Créer un projet »**
3. Rechercher **« Projet vide »** (Empty Project) — *on part de zéro, sans code généré automatiquement*
4. Choisir un nom de projet et un emplacement
5. Une fois le projet créé : clic droit sur le dossier **Fichiers sources** (Source Files) → **Ajouter** → **Nouvel élément** → `main.cpp`
```cpp
#include <iostream>
 
int main()
{
    std::cout << "Hello Visual Studio !" << std::endl;
    return 0;
}
```
 
6. Lancer avec **Ctrl + F5** (exécuter sans débogage) ou **F5** (exécuter avec débogage)
---
 
## 4. Tour de l'interface
 
### a) La barre d'outils supérieure
- Bouton vert ▶️ (Local Windows Debugger) : compile + exécute en mode debug
- Menu déroulant **Debug / Release** : le mode de compilation
- Menu déroulant **x86 / x64** : l'architecture cible (64 bits par défaut aujourd'hui)
### b) L'explorateur de solutions (Solution Explorer)
Situé à droite par défaut. C'est l'arborescence de votre projet :
- **Fichiers sources** (.cpp)
- **Fichiers d'en-tête** (.h / .hpp)
- **Fichiers de ressources**
C'est ici qu'on ajoute/supprime des fichiers, et qu'on accède aux **propriétés du projet** (clic droit sur le projet → Propriétés), un menu qu'on va beaucoup utiliser pour le link de librairies.
 
### c) L'éditeur de code
Zone centrale. Coloration syntaxique, IntelliSense (autocomplétion), soulignement des erreurs en temps réel (vagues rouges).
 
### d) La fenêtre de sortie (Output)
En bas. Affiche les logs de compilation. **Toujours la regarder en cas d'erreur** : elle indique précisément quelle étape a échoué (compilation ou édition de liens).
 
### e) La liste d'erreurs (Error List)
En bas également (onglet à côté de Output). Liste les erreurs et warnings, avec le fichier et la ligne concernés. Double-clic sur une ligne = saut direct à l'endroit du code concerné.
 
### f) Le Gestionnaire de propriétés (Property Manager)
Accessible via **Affichage → Gestionnaire de propriétés**. Vue plus technique des propriétés du projet, utile pour comprendre comment les configurations (Debug/Release, x86/x64) héritent de propriétés communes.
 
### g) L'intégration Git
 
Vous venez de voir Git en cours, bonne nouvelle : Visual Studio a un suivi Git intégré, pas besoin de tout faire en ligne de commande.
 
- **Git Changes** (`Affichage → Git Changes`) : équivalent d'un `git status` + `git add` + `git commit` visuel. On y voit les fichiers modifiés, on coche ceux à stager, on écrit son message et on commit directement.
- **Git Repository** (`Affichage → Git Repository` ou `Git → Voir l'historique du référentiel`) : vue graphique des branches et de l'historique des commits, pratique pour visualiser un arbre de commits sans taper `git log`.
- Le **menu Git** dans la barre de menus regroupe toutes les actions courantes : Pull, Push, changer de branche, créer une branche, gérer les remotes.
- En bas à droite de la fenêtre, dans la barre d'état, un indicateur affiche la **branche courante** et l'état du dépôt (nombre de commits en avance/retard) — cliquer dessus ouvre rapidement les actions Git.
💡 Vous n'êtes pas obligés de l'utiliser à la place du terminal, mais c'est pratique pour visualiser ce que vous venez d'apprendre en ligne de commande.
 
---
 
## 5. Solution vs Projet
 
Point souvent confus chez les débutants :
 
- Une **Solution** (`.sln`) peut contenir **plusieurs projets**
- Un **Projet** (`.vcxproj`) correspond à un exécutable ou une bibliothèque
Exemple typique en jeu vidéo : une solution "MonJeu" avec un projet "Moteur" (bibliothèque) et un projet "Jeu" (exécutable) qui dépend du moteur.
 
---
 
## 6. Compiler et exécuter
 
### Debug vs Release
| Mode | Utilisation | Caractéristiques |
|---|---|---|
| Debug | Développement | Pas d'optimisations, infos de débogage incluses, plus lent à l'exécution |
| Release | Version finale | Code optimisé, plus rapide, pas d'infos de debug |
 
### x86 vs x64
- **x64** : architecture 64 bits (à utiliser par défaut aujourd'hui)
- **x86** : architecture 32 bits (à utiliser seulement si une lib externe l'exige)
⚠️ Piège fréquent : lier une lib compilée en x86 alors que le projet est en x64 (ou l'inverse) → erreurs de link. Il faut que le mode et l'architecture correspondent des deux côtés.
 
---
 
## 7. Le débogueur
 
### Points d'arrêt (breakpoints)
Cliquer dans la marge grise à gauche d'une ligne de code (un point rouge apparaît). Lancer avec **F5** : l'exécution s'arrêtera à cette ligne.
 
### Pas à pas
- **F10** : pas à pas principal (n'entre pas dans les fonctions appelées)
- **F11** : pas à pas détaillé (entre dans les fonctions)
- **Maj + F11** : sortir de la fonction courante
### Fenêtres utiles pendant le debug
- **Locals** : variables locales et leur valeur en temps réel
- **Watch** : ajouter manuellement des variables/expressions à surveiller
- **Call Stack** : la pile d'appels (utile pour comprendre "qui a appelé quoi")
### Aller plus loin : mesurer les performances (Alt+F2)
 
Le raccourci **Alt+F2** (`Debug → Performance Profiler`) ouvre le **Performance Profiler** de Visual Studio. C'est un premier pas vers le profiling, très utile en jeu vidéo dès qu'on commence à se demander "pourquoi ça rame".
 
À savoir pour bien choisir son outil :
- Les outils du **Performance Profiler** (accessibles via Alt+F2) tournent en dehors du débogueur, en général sur une **build Release** : les résultats sont les plus fidèles à ce que vivra le joueur, car la Release contient les optimisations du compilateur.
- Les outils de la fenêtre **Diagnostic Tools**, eux, s'affichent automatiquement pendant une session de debug classique (F5) et permettent de croiser variables/breakpoints avec l'utilisation CPU/mémoire, mais sur une build Debug donc moins représentative des performances réelles.
Retenez simplement que ce raccourci existe : vous vous en servirez plus tard, quand vous commencerez à optimiser vos jeux. Plus de détails : [Run profiling tools on release or debug builds (Microsoft Learn)](https://learn.microsoft.com/en-us/visualstudio/profiling/running-profiling-tools-with-or-without-the-debugger?view=visualstudio).
 
---
 
## 8. Lier une bibliothèque externe
 
C'est la partie la plus technique, mais absolument essentielle en développement de jeux (SFML, GLFW, GLM, Box2D, etc. sont quasiment incontournables).
 
### a) Comprendre les deux types de bibliothèques
 
| Type | Extension | Fonctionnement |
|---|---|---|
| Statique | `.lib` | Le code de la lib est intégré directement dans votre .exe à la compilation |
| Dynamique | `.dll` (+ un `.lib` d'import) | Le code reste dans un fichier séparé, chargé au lancement du programme |
 
Pour une lib dynamique, il faut généralement **3 éléments** : les headers (`.h`), le `.lib` d'import (pour le link), et le `.dll` (à copier à côté de l'exécutable).
 
### b) Les 3 réglages à connaître
 
Clic droit sur le projet → **Propriétés**. ⚠️ Bien vérifier en haut de la fenêtre que la configuration sélectionnée est cohérente (souvent choisir **Toutes les configurations** / **Toutes les plateformes** pour éviter de devoir répéter les réglages en Debug ET en Release).
 
1. **C/C++ → Général → Répertoires d'inclusion supplémentaires**
   → chemin vers le dossier `include` de la bibliothèque (pour que `#include <SFML/...>` fonctionne)
2. **Éditeur de liens (Linker) → Général → Répertoires de bibliothèques supplémentaires**
   → chemin vers le dossier `lib` de la bibliothèque (là où se trouvent les fichiers `.lib`)
3. **Éditeur de liens → Entrée → Dépendances supplémentaires**
   → nom exact des fichiers `.lib` à lier, par exemple `sfml-graphics.lib`
### c) Exemple concret : lier SFML
 
1. Télécharger SFML (version correspondant à votre version de Visual Studio, et à l'architecture x64) depuis [sfml-dev.org](https://www.sfml-dev.org/download.php)
2. Dézipper dans un dossier connu, ex: `C:\libs\SFML-2.6.1`
3. Dans les propriétés du projet :
   - Répertoires d'inclusion supplémentaires → `C:\libs\SFML-2.6.1\include`
   - Répertoires de bibliothèques supplémentaires → `C:\libs\SFML-2.6.1\lib`
   - Dépendances supplémentaires → ajouter (en plus des valeurs déjà présentes) :
```
     sfml-graphics.lib
     sfml-graphics-d.lib
     sfml-window.lib
     sfml-window-d.lib
     sfml-system.lib
     sfml-system-d.lib
```
     *(en Debug, on met souvent les versions `-d`, ex: `sfml-graphics-d.lib`)*
4. **Copier les `.dll`** correspondants (dossier `bin` de SFML) dans le même dossier que votre `.exe` compilé (généralement `x64/Debug/` ou `x64/Release/`)
Une fois ces 4 étapes faites, direction la dernière section de ce cours pour vérifier que tout est bien branché.
 
---
 
## 9. Erreurs fréquentes de link (et comment les lire)
 
| Erreur | Signification probable |
|---|---|
| `LNK2019: unresolved external symbol` | Le `.lib` n'est pas trouvé/ajouté dans "Dépendances supplémentaires", ou mauvaise architecture (x86/x64) |
| `LNK1104: cannot open file 'xxx.lib'` | Mauvais chemin dans "Répertoires de bibliothèques", ou nom du fichier mal orthographié |
| Le `.exe` se lance puis crash immédiatement / fenêtre qui clignote | Un `.dll` manque à côté de l'exécutable |
| `Cannot open include file` | Mauvais chemin dans "Répertoires d'inclusion" |
 
💡 Réflexe à avoir : **toujours lire la fenêtre Output en entier**, la première erreur en haut est souvent la cause racine des suivantes.
 
---
 
## 10. Bonnes pratiques
 
- Toujours vérifier qu'on configure la bonne combinaison (Debug/Release × x86/x64)
- Utiliser **Toutes les configurations / Toutes les plateformes** quand un réglage doit s'appliquer partout
- Ranger les libs externes dans un dossier fixe (éviter les chemins en `Bureau\nouveau dossier (2)\...`)
- Ne jamais copier-coller un `.dll` "à l'arrache" sans comprendre pourquoi il est nécessaire
- Committer un `.gitignore` qui exclut les dossiers `Debug/`, `Release/`, `.vs/` si vous utilisez Git
---
 
## 11. Test : votre SFML est-il bien lié ?
 
Une fois les étapes de la section 8 terminées, collez ce code dans votre `main.cpp`, puis lancez avec **Ctrl+F5** :
 
```cpp
#include <SFML/Graphics.hpp>
 
int main()
{
    sf::RenderWindow window(sf::VideoMode(800, 600), "Ma fenetre SFML");
 
    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
                window.close();
        }
        window.clear();
        window.display();
    }
    return 0;
}
```
 
- ✅ Une fenêtre noire de 800x600 s'ouvre sans erreur : votre lib est correctement liée.
- ❌ Erreur de compilation (`Cannot open include file`) : vérifiez le chemin des **Répertoires d'inclusion supplémentaires**.
- ❌ Erreur de link (`LNK2019` / `LNK1104`) : vérifiez les **Répertoires de bibliothèques** et les **Dépendances supplémentaires**.
- ❌ La fenêtre plante à l'ouverture ou clignote : il manque un `.dll` à côté de votre `.exe`.
