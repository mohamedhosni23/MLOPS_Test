# MLOPS_Test: TP1 - Maîtrise de Git
*Dr. Salah Gontara MLOps 2025-26*

## Objectifs
- Maîtriser Git pour la gestion de projets ML
- Appliquer Git dans un workflow MLOps

## Partie 1: Préparation de l'environnement Git

### 1. Création de clé SSH
a. Utilisez la commande suivante pour générer votre clé SSH:
   ```bash
   ssh-keygen -t rsa -b 4096 -C votreadresse@email.com
   ```
b. Copiez votre clé publique SSH:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
c. Connectez-vous à votre compte sur le dépôt distant (par exemple, GitHub) et ajoutez votre clé publique SSH dans les paramètres de votre compte.

### 2. Configuration de Git
Configurez votre nom et votre adresse e-mail pour que vos commits soient correctement identifiés:
```bash
git config --global user.name "Votre Nom"
git config --global user.email votre@email.com
```

### 3. Connexion SSH aux dépôts distants
Pour tester votre connexion SSH, exécutez la commande suivante. Assurez-vous de remplacer `git@github.com` par l'adresse du serveur distant où vous hébergerez vos dépôts:
```bash
ssh -T git@github.com
```
ou
```bash
ssh -T git@gitlab.com
```

### 4. Comment vérifier la configuration actuelle de Git sur votre machine, notamment le nom d’utilisateur et l’adresse e-mail?
```bash
git config --list
```

### 5. Comment modifier votre adresse e-mail si vous l'avez mal configurée lors de l'installation de Git?
```bash
git config --global user.email "votre@email.com"
```

## Partie 2: Création d'un nouveau projet

1. Connectez-vous à votre compte GitHub (ou GitLab).
2. Une fois connecté, cliquez sur le bouton "Nouveau" pour créer un nouveau projet. Donnez-lui un nom significatif et choisissez les options appropriées (public ou privé, avec ou sans README, etc.). Ensuite, cliquez sur "Créer le référentiel" (ou "Create Repository").
3. Notez l'URL de votre projet. Elle ressemblera à ceci:
   ```
   git@github.com:mohamedhosni23/MLOPS_Test.git
   ```
   (pour GitHub) ou `git@gitlab.com:votre-nom-utilisateur/fraud-detection-mlops.git` (pour GitLab).
4. Clonez le projet en utilisant l'URL SSH:
   ```bash
   git clone git@github.com:mohamedhosni23/MLOPS_Test.git
   ```
5. Accédez au projet cloné:
   ```bash
   cd MLOPS_Test
   ```

### 6. Si vous avez oublié de créer un fichier README.md lors de l'initialisation du projet, comment pouvez-vous l'ajouter après coup et committer les changements?
Voici les étapes:
```bash
touch README.md
echo "# MLOPS_Test" > README.md
git add README.md
git commit -m "Add README.md to project"
git push origin main
```
*Note*: Je ne l'ai pas fait car j'ai déjà sélectionné README lors de la création du repository 😀

### 7. Comment définir un dépôt distant si vous n'en avez pas configuré un lors de la création du projet?
Voici les étapes:
```bash
git remote add origin git@github.com:mohamedhosni23/MLOPS_Test.git
git remote -v
```
(This should display the fetch and push URLs for origin)
```bash
git push -u origin main
```

## Partie 3: Concepts de base de Git

### 1. Travailler avec les fichiers
Créez un fichier appelé `eda.py` dans votre répertoire de projet et ajoutez-y du contenu:
```bash
touch eda.py
echo "import pandas as pd\ndf = pd.read_csv('data.csv')\nprint(df.head())" > eda.py
```
Ajoutez le fichier à l'index (staging) et validez votre premier commit:
```bash
git add eda.py
git commit -m "Premier commit : ajout de eda.py"
```

### 2. Historique des commits
Visualisez l'historique des commits:
```bash
git log
```
Pour afficher les différences entre deux commits:
```bash
git diff commit_id_1 commit_id_2
```

### 3. Comment annuler les modifications locales d'un fichier avant de les ajouter à l'index?
Voici:
```bash
git restore eda.py
git clean -f eda.py
```
To discard all:
```bash
git restore .
git clean -fd
```

### 4. Comment visualiser les fichiers qui sont prêts à être committés dans Git (staging)?
Voici:
```bash
git status
git diff --cached
```
or
```bash
git diff --staged
```

## Partie 4: Collaborer sur Git

### 1. Créer une nouvelle branche
Créez une nouvelle branche pour travailler sur une fonctionnalité spécifique:
```bash
git branch experiment-eda
git checkout experiment-eda
```

### 2. Effectuer des modifications
Effectuez des modifications, ajoutez-les à l'index, faites un commit, puis poussez vers le dépôt distant:
```bash
git add .
git commit -m "Modification de experiment-eda"
git push origin experiment-eda
```

### 3. Gestion des conflits
Simulez un conflit en modifiant la même ligne dans deux branches différentes, puis fusionnez et résolvez:
```bash
git checkout experiment-eda
git commit -am "Modification dans experiment-eda"
git checkout master
git commit -am "Modification dans master"
git merge experiment-eda
git add .
git commit -m "Résolution du conflit"
```

### 4. Comment suivre (track) un dépôt distant et récupérer toutes les branches de ce dépôt?
[To be answered, as per your previous question response]

### 5. Comment supprimer une branche locale après l'avoir fusionnée dans master?
```bash
git branch -d experiment-eda
```

## Partie 5: Rebase d'une branche sur ‘master’

### Intégrer une branche dans master en utilisant rebase

1. Passer sur la branche master et la mettre à jour:
   ```bash
   git checkout master
   git pull origin master
   ```

2. Changer de branche pour celle à intégrer:
   ```bash
   git checkout experiment-eda
   ```

3. Rebaser la branche `experiment-eda` sur `master`:
   ```bash
   git rebase master
   ```

4. Résoudre les conflits (si nécessaire):
   ```bash
   git add fichier_conflit
   git rebase --continue
   ```

5. Retourner sur la branche master:
   ```bash
   git checkout master
   ```

6. Fusionner sans commit supplémentaire:
   ```bash
   git merge experiment-eda
   ```

7. Pousser les modifications sur le dépôt distant:
   ```bash
   git push origin master
   ```

### 8. Comment interrompre un rebase en cours si vous avez commis une erreur?
```bash
git rebase --abort
```

### 9. Comment lister les commits qui vont être rebasés avant de lancer un rebase?
Voici:
```bash
git log master..experiment-eda
git log --oneline master..experiment-eda
git checkout experiment-eda
git branch
git checkout master
git pull origin master
git checkout experiment-eda
```

## Images
The following images were referenced in the original document but are not embedded here. Add them to the repository and link them in this Markdown file as needed:
- image1.png
- image2.png
- image3.png
- image4.png
- image5.png
- image6.png
- image7.png
- image8.png
- image9.png
- image10.png
- image11.png
- image12.png
- image13.png
- image14.png
- image15.png
- image16.png
- image18.png

To include an image in Markdown (e.g., `image1.png`), add it to your repository and reference it:
```markdown
![Description of image1](image1.png)
```
