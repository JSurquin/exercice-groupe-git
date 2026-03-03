TP Git – Travail en équipe avec Git Flow (Merge, Rebase, Squash)

Objectif

Vous allez simuler un vrai projet d’équipe avec :
	•	1 repository GitHub par groupe
	•	Branches main / develop
	•	Branches features individuelles
	•	Commits concurrents
	•	Merge classique pour certains
	•	Rebase obligatoire pour d’autres
	•	Rebase interactif + squash en fin d’exercice

Groupes : 3 ou 4 personnes

⸻

Contexte projet

Mini application CLI en Node.js : TaskBoard

Fonctionnalités
	•	Ajouter une tâche
	•	Lister les tâches
	•	Marquer une tâche comme terminée
	•	Supprimer une tâche

⸻

ÉTAPE 1 – Base du projet (à copier/coller)

Un membre crée le repository GitHub :

taskboard-groupe-X

En local :

git init
git checkout -b main


⸻

📄 package.json

{
  "name": "taskboard",
  "version": "1.0.0",
  "description": "Mini CLI Task Manager",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  }
}


⸻

📄 tasks.json

[]


⸻

📄 utils.js

const fs = require("fs");

const FILE = "./tasks.json";

function readTasks() {
  if (!fs.existsSync(FILE)) {
    return [];
  }
  const data = fs.readFileSync(FILE);
  return JSON.parse(data);
}

function writeTasks(tasks) {
  fs.writeFileSync(FILE, JSON.stringify(tasks, null, 2));
}

module.exports = {
  readTasks,
  writeTasks
};


⸻

📄 index.js

const { readTasks, writeTasks } = require("./utils");

const command = process.argv[2];
const argument = process.argv[3];

function addTask(title) {
  const tasks = readTasks();
  tasks.push({
    id: Date.now(),
    title,
    completed: false
  });
  writeTasks(tasks);
  console.log("Tâche ajoutée.");
}

function listTasks() {
  const tasks = readTasks();
  if (tasks.length === 0) {
    console.log("Aucune tâche.");
    return;
  }

  tasks.forEach(task => {
    console.log(
      `${task.id} - ${task.title} - ${task.completed ? "✔" : "✘"}`
    );
  });
}

switch (command) {
  case "add":
    addTask(argument);
    break;
  case "list":
    listTasks();
    break;
  default:
    console.log("Commandes disponibles : add, list");
}


⸻

Commit initial

git add .
git commit -m "init: base taskboard cli"
git remote add origin <url>
git push -u origin main


⸻

ÉTAPE 2 – Mise en place Git Flow simplifié

Créer develop :

git checkout -b develop
git push -u origin develop


⸻

Répartition des rôles

Dans chaque groupe :
	•	1 mainteneur de la branche develop
	•	2 développeurs qui feront un merge classique
	•	1 développeur (ou 2 si groupe de 4) qui devra faire un rebase

⸻

ÉTAPE 3 – Création des branches features

Chaque développeur :

git checkout develop
git pull
git checkout -b feature/nom-feature

Features possibles
	•	feature/delete-task
	•	feature/complete-task
	•	feature/refactor-utils
	•	feature/improve-display

Chaque développeur doit faire au minimum 3 commits distincts.

⸻

ÉTAPE 4 – Bifurcations obligatoires

Pendant que les développeurs travaillent :

Le mainteneur de develop doit faire 2 commits minimum directement sur develop.

Exemples :
	•	Modifier README
	•	Ajouter un console.log global
	•	Refactor léger dans utils.js

Cela doit provoquer une divergence entre develop et les branches features.

⸻

ÉTAPE 5 – Merge classique (2 développeurs)

Ces développeurs doivent :

git checkout develop
git pull
git merge feature/xxx

Contraintes
	•	Résoudre au moins 1 conflit
	•	Créer un commit de merge
	•	Vérifier l’historique avec :

git log --oneline --graph --all --decorate


⸻

ÉTAPE 6 – Rebase obligatoire (1 ou 2 développeurs)

Ces développeurs doivent :

git checkout feature/xxx
git fetch origin
git rebase origin/develop

Résoudre les conflits.

Puis :

git checkout develop
git merge feature/xxx

Le merge doit être fast-forward.

⸻

ÉTAPE 7 – Nettoyage final (Rebase interactif)

Chaque feature doit finir avec un seul commit propre.

Sur la branche feature :

git rebase -i develop

Transformer :

pick 1234 ajout logique
pick 4567 fix
pick 8910 debug

En :

pick 1234 feature complète
squash 4567 fix
squash 8910 debug

Puis pousser la branche mise à jour.

⸻

Livrables obligatoires
	1.	URL du repository
	2.	Capture du graph avant rebase
	3.	Capture du graph après rebase
	4.	Explication écrite :
	•	Ce qu’est une bifurcation
	•	Différence merge vs rebase
	•	Différence merge commit vs fast-forward
	•	Pourquoi le rebase réécrit l’historique

⸻

Objectif pédagogique

Comprendre visuellement et concrètement l’impact des stratégies d’intégration sur l’historique Git.
