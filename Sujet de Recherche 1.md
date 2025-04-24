# I. Conteneurisation avec Docker
## 1. Définition d’un conteneur Docker
### Docker est une plateforme(PaaS) ouverte(Open Sources) pour développer, livrer et exécuter des applications.

Docker permet de **séparer vos applications de votre infrastructure**, ce qui vous permet de livrer des logiciels plus rapidement.

Avec Docker, vous pouvez **gérer votre infrastructure de la même manière que vous gérez vos applications**.

(*selon le site officiel de docker [https://docs.docker.com/get-started/docker-overview/*](https://docs.docker.com/get-started/docker-overview/*))

La grande innovation de Docker est de permettre de “packager” une application ou un ensemble de services dans des conteneurs. Un conteneur Docker est une instance d’une application qui contient toutes les bibliothèques et composants nécessaires au fonctionnement d’une application. D’un point de vue pratique, un conteneur est une sorte de machine virtuelle limitée qui fonctionne indépendamment du système d’exploitation dans lequel une application ou un service spécifique est exécuté.

Un conteneur Docker est généré à partir d’une image qui est le résultat de l’application ou du service packagé. Il peut contenir un système d’exploitation complet ou des applications pré-installées. C’est-à-dire qu’à partir d’une image, le conteneur se met à fonctionner.

_([https://www.hostinger.com/fr/tutoriels/installer-docker-sur-ubuntu](https://www.hostinger.com/fr/tutoriels/installer-docker-sur-ubuntu))_

## 🧱 Architecture de Docker – Vue d’ensemble
### ⚙️ 1. **Docker Engine** (le cœur de Docker)

C’est le moteur principal de Docker. Il se compose de deux parties :

- **Docker Daemon (`dockerd`)** : C’est un service qui tourne en arrière-plan sur la machine. Il exécute les commandes Docker, gère les **conteneurs, images, réseaux, volumes**, etc.
    
- **Docker Client (`docker`)** : C’est ce que tu utilises dans le terminal pour parler au daemon. Quand tu tapes une commande comme `docker run`, c’est le client qui l’envoie au daemon.
    

---

### 🖼️ 2. **Docker Images**

- Une **image Docker** est comme un **modèle** ou une **photo figée** d’un environnement d’application (ex : une appli Node.js avec ses dépendances).
    
- Elles sont **créées à partir de fichiers `Dockerfile`**, qui contiennent des instructions (comme "installe Node", "copie les fichiers", etc.).
    
- Les images sont **en lecture seule**. Pour les utiliser, on les transforme en conteneurs.
    

---

### 📦 3. **Docker Containers**

- Un **conteneur** est une **instance vivante** d’une image.
    
- C’est là que l’application tourne, de manière **isolée**.
    
- Tu peux **créer, démarrer, arrêter, déplacer ou supprimer** des conteneurs facilement avec Docker.
    

---

### 🗂️ 4. **Docker Registry**

- Un **registry** est un endroit pour **stocker et partager des images**.
    
- Le plus connu est **Docker Hub** (public), mais tu peux aussi avoir ton propre **registry privé** pour tes images internes à ton entreprise.
    

---

### 🧩 5. **Docker Compose**

- **Docker Compose** est un outil qui te permet de **gérer plusieurs conteneurs** ensemble.
    
- Tu décris toute ton appli (les services, réseaux, volumes...) dans un **fichier YAML (`docker-compose.yml`)**.
    
- Très utile pour les applis complexes avec plusieurs composants (ex : une app web + une base de données + un cache).
    

---

### 💾 6. **Docker Volumes**

- Les **volumes** servent à **garder les données même quand un conteneur est supprimé**.
    
- Par exemple : si ta base de données est dans un conteneur, tu ne veux pas perdre les données à chaque redémarrage → tu utilises un volume.
    

---

### 🌐 7. **Docker Networking**

- Docker crée des **réseaux virtuels** pour que les conteneurs puissent **communiquer entre eux** ou avec l’extérieur.
    
- Tu peux avoir des **réseaux isolés**, des **règles de communication**, etc.
    
- Tu peux même connecter plusieurs conteneurs ensemble via Compose ou en ligne de commande.
-Architecture de docker ![[Architecture Docker 1.png]]

*(https://medium.com/@ravipatel.it/understanding-docker-architecture-a-comprehensive-guide-5ce9129df1a4)*
## 2. Différences entre conteneur et machine virtuelle

- **Conteneur :**
    - Léger et rapide
    - Ne contient que l’application et ses dépendances
    - Partage le système d’exploitation de la machine hôte
    - Démarre en quelques secondes
- **Machine virtuelle (VM) :**
    - Plus lourde
    - Contient un système d’exploitation complet en plus de l’application
    - Nécessite plus de ressources (RAM, CPU)
    - Démarre en plusieurs minutes

|Aspect|Conteneur Docker|Machine Virtuelle (VM)|
|---|---|---|
|**Isolation**|Partage le noyau de l’OS hôte, mais est isolé|Complètement isolée, y compris OS invité|
|**Poids**|Très léger|Lourd (OS complet dans chaque VM)|
|**Temps de démarrage**|Quelques secondes|Plusieurs dizaines de secondes à minutes|
|**Performance**|Plus rapide et proche des performances natives|Moins performant à cause de l’hyperviseur|
|**Consommation de ressources**|Moins de RAM et CPU requis|Plus gourmand en ressources|
|**Portabilité**|Très portable (fonctionne partout où Docker est installé)|Portabilité plus limitée|
|**Cas d’usage typique**|Développement, microservices, CI/CD|Virtualisation d’environnements complets|

_(selon ChatGPT)_

## 3. Principe d’un Dockerfile

### Qu’est-ce qu’un Dockerfile ?

Un **Dockerfile** est un fichier texte qui contient les **instructions nécessaires à la création d’une image Docker personnalisée**. Chaque ligne du Dockerfile représente une **étape de construction** que Docker exécutera pour générer l'image.

### 🔧 Exemple simple :

```yaml
# Utiliser une image de base
FROM node:18

# Créer un dossier de travail
WORKDIR /app

# Copier les fichiers du projet
COPY . .

# Installer les dépendances
RUN npm install

# Démarrer l’application
CMD ["npm", "start"]
```

|Instruction|Rôle|
|---|---|
|`FROM`|Spécifie l’image de base (ex. `ubuntu`, `node`, etc.)|
|`WORKDIR`|Définit le dossier de travail à l’intérieur du conteneur|
|`COPY`|Copie des fichiers du système hôte vers l’image|
|`RUN`|Exécute une commande (ex : installation de paquets)|
|`CMD`|Définit la commande à exécuter quand le conteneur démarre|
|`EXPOSE`|Indique le port sur lequel l’application tourne (facultatif)|

### 1. 🧾 **Dockerfile → Image**

Le **Dockerfile** est un **fichier texte** contenant les **instructions** pour construire une **image Docker**.

```
docker build -t mon-image .
```

### 2. 📦 **Image → Conteneur**

Une **image Docker** est un **modèle statique** (un peu comme un template ou une photo figée) de ce que sera un environnement d’exécution.

### 3. 🧱 **Conteneur → Noeud**

Un **nœud (ou node)** est une **machine (physique ou virtuelle)** qui **exécute Docker**. C’est l’environnement qui héberge et fait tourner les conteneurs.

- Un **conteneur s’exécute toujours sur un nœud Docker**.
- Si tu utilises **Kubernetes**, un nœud est une **machine du cluster** capable d’héberger plusieurs conteneurs orchestrés.

```
Dockerfile ➜ Image ➜ Conteneur ➜ Noeud
    |          |         |          |
 Recette   Modèle    Instance   Machine
```

En resume

A partir du **Dockefile** on contruit un **Image** et de ce dernier on construit **Conteneur** celui ci tourne dans un **Noeud**

_(ChatGPT)_

# Une révolution pour le cloud ?

Venons-en au fait : est-ce que Docker va révolutionner le cloud ? Oui… et non ! En fait, tout dépend de la façon dont il évoluera. En théorie, cela pourrait être le cas puisqu’il permet deux choses cruciales.

Tout d’abord, la réduction des ressources nécessaires au déploiement d’un cloud. Grâce à l’utilisation des conteneurs pouvant faire tourner des applications sans système d’exploitation embarqué et à l’absence d’hyperviseur, on peut envisager un cloud tournant sur des infrastructures serveurs moins puissantes, plus petites, et donc moins coûteuses.

De plus, étant donné que Docker a été conçu pour faciliter la portabilité des conteneurs, on peut envisager la migration entre différentes infrastructures (ex : de serveurs Windows à des serveurs Linux), voire même la redondance en temps réel entre ces infrastructures.

Mais nous n’en sommes, pour le moment, qu’à la théorie. Docker est une application qui, dans sa forme actuelle, est surtout destinée aux développeurs. Ainsi, elle s’avère idéale pour tester, en quelques secondes via un serveur de développement peu performant, la qualité d’un développement sur plusieurs systèmes. Au-delà, Docker n’est pas encore assez performant et ne fait pas le poids face aux systèmes de virtualisation qui ont plusieurs années d’expériences. De plus, actuellement, Docker est clairement utilisé dans des machines virtuelles classiques pour permettre à plus d’applications de tourner sans consommer plus de ressources. Il est donc perçu comme une brique complémentaire de la virtualisation classique, et non comme un concurrent.

Ainsi, et bien qu’il n’en soit qu’à ses débuts, il est important de suivre le développement de ce petit poucet qui pourrait bien, prochainement, devenir aussi gros que l’ogre.

_Sources: ([https://www.diatem.net/cloud-docker/](https://www.diatem.net/cloud-docker/))_

# Docker, seule solution de conteneurs ?

Pour finir, il est important de signaler que Docker n’est pas la seule solution à envisager les conteneurs pour remplacer, ou compléter, la virtualisation par hyperviseur.

Et même plus, Docker fait aujourd’hui face à un concurrent de taille qui critique sèchement son modèle, après l’avoir pourtant officiellement soutenu. Ce concurrent, c’est CoreOS, la société éditrice de la distribution Linux du même nom. CoreOS a décidé, suite à des divergences d’opinions sur ce que devait être les conteneurs et leur façon de fonctionner, de développer son propre standard et son environnement.

Ainsi, CoreOS a créé App Container, un standard qui s’appuie sur Rocket, la technologie permettant la construction et la gestion des conteneurs.

Il n’est pas question ici de départager les deux solutions, ni même de résumer le débat qui les oppose. Mais il est important de souligner que la solution de CoreOS a reçu, officiellement, des soutiens de poids en mai 2015. Ces soutiens ce sont des employés de Google, de Red Hat (distribution Linux) et de Twitter. Mais, en annonçant un soutien officiel, ces employés indique officieusement que leur employeur soutient aussi App Container. Et ce n’est pas tout puisque VMware et Apcera, deux gros acteurs du cloud, ont aussi apporté leur soutien.

Autant dire que Docker, même s’il fait de plus en plus parler de lui, n’est pas encore sûr de s’imposer sur le marché. Il ne faut donc pas négliger App Container, ainsi que d’autres solutions qui peuvent émerger, et continuer à suivre l’évolution des technologies de conteneurs qui n’en sont, pour le moment, qu’à leurs débuts.

## 4. Décrire une application avec Docker

Pour décrire une application avec Docker, vous devez créer un fichier appelé Dockerfile qui contient une série d'instructions permettant de définir l'environnement dans lequel une application sera exécutée.Ces instructions peuvent inclure l'installation de logiciels, la copie de fichiers, la définition de variables d'environnement, etc. Le Dockerfile est essentiel pour automatiser la création d'une image Docker, en spécifiant les dépendances, les fichiers à copier, les commandes à exécuter, etc.Chaque ligne du Dockerfile correspond à une étape dans le processus de construction de l'image.

Une fois le Dockerfile créé, vous pouvez utiliser la commande docker build pour construire une image Docker à partir de ce fichier.

Cette image contiendra tout ce dont votre application a besoin pour fonctionner : le code, les bibliothèques, les dépendances et les configurations système.

Une fois l'image construite, vous pouvez utiliser la commande docker run pour créer et démarrer un conteneur à partir de cette image.

[*](https://search.brave.com/search?q=Comment+D%C3%A9crire+une+application+avec+Docker&source=desktop&conversation=d1109d60b7fc86bcb78f1f&summary=1)[https://search.brave.com/search?q=Comment+Décrire+une+application+avec+Docker&source=desktop&conversation=d1109d60b7fc86bcb78f1f&summary=1*](https://search.brave.com/search?q=Comment+D%C3%A9crire+une+application+avec+Docker&source=desktop&conversation=d1109d60b7fc86bcb78f1f&summary=1*)

## **1. Dockerfile (pour une application simple)**

![[Docker.png]]

Le **`Dockerfile`** décrit les étapes pour construire une image Docker contenant votre application.

### **Exemple pour une application Python :**

```yaml
# Étape 1 : Utiliser une image de base officielle
FROM python:3.9-slim

# Étape 2 : Définir le répertoire de travail dans le conteneur
WORKDIR /app

# Étape 3 : Copier les fichiers nécessaires
COPY requirements.txt .
COPY app.py .

# Étape 4 : Installer les dépendances
RUN pip install --no-cache-dir -r requirements.txt

# Étape 5 : Exposer le port utilisé par l'application
EXPOSE 5000

# Étape 6 : Commande à exécuter au lancement du conteneur
CMD ["python", "app.py"]
```

### **Explications :**

- **`FROM`** : L'image de base (Python, Node.js, Java, etc.).
- **`WORKDIR`** : Le répertoire où les commandes suivantes seront exécutées.
- **`COPY`** : Copie des fichiers locaux vers l'image.
- **`RUN`** : Exécute une commande pendant la construction (installation de dépendances).
- **`EXPOSE`** : Indique le port à exposer.
- **`CMD`** : La commande qui démarre l'application.

## **2. Docker Compose (pour une application multi-conteneurs)**

Si votre application utilise plusieurs services (ex. : base de données, backend, frontend), utilisez **`docker-compose.yml`**.

### **Exemple avec une API Flask + PostgreSQL :**

```yaml
version: '3.8'

services:
  # Service pour l'application Flask
  web:
    build: .  # Construit l'image à partir du Dockerfile
    ports:
      - "5000:5000"
    environment:
      - FLASK_ENV=development
    depends_on:
      - db

  # Service pour PostgreSQL
  db:
    image: postgres:13
    environment:
      - POSTGRES_USER=myuser
      - POSTGRES_PASSWORD=mypassword
    volumes:
      - postgres_data:/var/lib/postgresql/data

# Volume pour persister les données de la base
volumes:
  postgres_data:
```

### **Explications :**

- **`services`** : Définit les différents conteneurs.
- **`build`** : Construit l'image depuis le **`Dockerfile`**.
- **`ports`** : Mappe les ports (host:conteneur).
- **`environment`** : Définit des variables d'environnement.
- **`depends_on`** : Indique les dépendances entre services.
- **`volumes`** : Persiste les données (ici pour PostgreSQL).

## **3. Commandes Utiles**

### **Construire et lancer avec Docker :**

```bash
docker build -t mon-app .          # Construit l'image
docker run -p 5000:5000 mon-app    # Lance le conteneur
```

### **Lancer avec Docker Compose :**

```bash
docker-compose up -d  # Lance les services en arrière-plan
docker-compose down   # Arrête et supprime les conteneurs
```

## 5. Fonctionnement de Docker Compose

### 🔷 Qu’est-ce que Docker Compose ?

Docker Compose est un outil qui te permet de **décrire, configurer et exécuter plusieurs conteneurs Docker** à l’aide d’un **fichier YAML** (`docker-compose.yml`).

> ⚙️ Il agit comme un orchestrateur léger pour démarrer tous tes services avec une seule commande.

---

### 📄 Exemple simple : une app web + base de données

```yaml
version: "3.8"
services:
  web: #Partie pour service WEB
    image: my-web-app
    ports:
      - "8080:80" #port 
    depends_on:
      - db

  db:# Partie pour le Service database
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb #nom de la base de donnees
```

 Fonctionnement global de Docker Compose

| Étape                  | Ce que fait Docker Compose                                        |
| ---------------------- | ----------------------------------------------------------------- |
| `docker-compose.yml`   | Décrit tous les conteneurs, images, ports, volumes, réseaux, etc. |
| `docker compose up`    | Crée et démarre tous les services à partir du fichier YAML        |
| `docker compose down`  | Arrête et supprime tous les conteneurs, réseaux, etc.             |
| `docker compose build` | Construit les images Docker si tu as un `Dockerfile`              |
| `docker compose ps`    | Affiche les services en cours d’exécution                         |

## Pourquoi utiliser Kubernetes alors qu’on a déjà Docker Compose ?

Docker Compose est super pour **un développeur ou une petite équipe**, mais quand tu passes à **une application à grande échelle**, tu as besoin de beaucoup plus de contrôle, de résilience, d’automatisation... C’est là que **Kubernetes** entre en jeu.

## Docker Compose vs Kubernetes – Comparaison complète

| Fonctionnalité / Besoin                | **Docker Compose** | **Kubernetes**                          |
| -------------------------------------- | ------------------ | --------------------------------------- |
| Définir plusieurs conteneurs           | ✅ Oui              | ✅ Oui                                   |
| Lancer un projet multi-services        | ✅ Simple et rapide | ✅ Plus complexe à configurer            |
| Portabilité environnement dev          | ✅ Très bien adapté | ⚠️ Moins adapté (plus lourd)            |
| Scalabilité automatique (auto-scaling) | ❌ Non              | ✅ Oui                                   |
| Redémarrage automatique (self-healing) | ❌ Non              | ✅ Oui                                   |
| Gestion d'état du cluster              | ❌ Non              | ✅ Kubernetes gère les nœuds, pods, etc. |
| Déploiement dans le cloud              | ⚠️ Peu adapté      | ✅ Intégration native (AWS, GCP, Azure)  |
| Rolling updates, rollback              | ❌ Non              | ✅ Oui                                   |
| Load balancing intégré                 | ❌ (manuel)         | ✅ Oui (service, ingress…)               |
| Gestion des secrets                    | ❌ Basique          | ✅ Avancée (Secrets, ConfigMaps)         |

## 6. Orchestration de plusieurs conteneurs

## **Orchestration de plusieurs conteneurs** : c’est quoi ?

C’est le **processus d’automatisation** de la gestion de plusieurs conteneurs, pour qu’ils fonctionnent ensemble **de manière fluide, efficace et stable**, surtout quand ça devient gros ou complexe.

Imagine une application composée de :

- un serveur web (Nginx),
- une API Node.js,
- une base de données PostgreSQL,
- un service de cache Redis.

Tu pourrais les lancer à la main avec `docker run`, un par un… mais ce serait vite **ingérable**.

>  L’orchestration s’en occupe pour toi

# II. Orchestration avec Kubernetes

##  **ARCHITECTURE ET COMPOSANTS DE KUBERNETES**

*image venant de* :https://beopenit.com/qu-est-ce-que-kubernetes/
![[Screenshot from 2025-04-21 16-05-12.png]]
### —  PLAN DE CONTRÔLE (CONTROL PLANE)

Le **plan de contrôle**, ou **nœud maître**(Kubernetes Master)**, est l’unité centrale de gestion d’un cluster Kubernetes. Il supervise l’ensemble des opérations du cluster et coordonne son fonctionnement. Il prend des décisions de haut niveau sur le déploiement des applications et veille à ce que l’état réel du cluster corresponde à l’état souhaité.

- **API Server (Serveur API)** :  
    C’est le **point d’entrée** pour interagir avec le cluster Kubernetes. Il expose l’API Kubernetes, que les utilisateurs, administrateurs et autres composants peuvent utiliser pour communiquer avec le cluster.
    
- **etcd** :  
    C’est une base de données **clé-valeur distribuée**. Elle sert de **stockage central** pour toutes les données du cluster (Pods, Services, configurations de réplication, etc.).
    
- **Controller Manager (Gestionnaire de contrôleurs)** :  
    Il surveille l’état des objets du cluster et prend des **actions correctives** pour maintenir l’état désiré. Il comprend plusieurs contrôleurs intégrés comme le **Replication Controller**, le **Deployment Controller** ou le **StatefulSet Controller**.
    
- **Scheduler (Planificateur)** :  
    Il choisit sur quel nœud **exécuter les Pods**, en fonction de la disponibilité des ressources, des contraintes et des objectifs d’optimisation.
    

 Ensemble, ces composants forment le **cerveau** du cluster.

---

### —  NŒUDS TRAVAILLEURS (WORKER NODES)

Les **nœuds de travail**, aussi appelés **machines de travail**, exécutent les conteneurs et les charges de travail applicatives.

- **Kubelet** :  
    C’est un **agent** qui s’exécute sur chaque nœud. Il communique avec le plan de contrôle et s’assure que les conteneurs sont bien lancés et fonctionnels.
    
- **Kube Proxy** :  
    Il s’occupe du **routage réseau** et de l’**équilibrage de charge**, pour permettre la communication entre les applications.
    
- **Container Runtime (Moteur de conteneurisation)** :  
    C’est le logiciel chargé d’**exécuter les conteneurs** sur chaque nœud.
    
- **CSI (Container Storage Interface)** :  
    Interface standardisée permettant aux nœuds d’accéder à des solutions de stockage **persistant**.
    

---

### —  PODS

Les _Pods_ sont les plus petites unités informatiques déployables qui peuvent être créées et gérées dans Kubernetes.
*selon le site officielles de kubernetes : (https://kubernetes.io/fr/docs/concepts/workloads/pods/pod/)*

Les **Pods** sont les unités fondamentales de Kubernetes. Ils regroupent un ou plusieurs conteneurs qui partagent le même réseau et les mêmes volumes de stockage.

En terme de concepts [Docker](https://www.docker.com/), un pod est modélisé par un groupe de conteneurs Docker ayant des namespaces et des [volumes](https://kubernetes.io/fr/docs/concepts/storage/volumes/) partagés.

Un Pods est exposé en tant que primitive afin de faciliter :

- la connexion du scheduler et du contrôleur
- la possibilité d'opérations au niveau du pod sans besoin de passer par des APIs au niveau du contrôleur
- le découplage du cycle de fin d'un pod de celui d'un contrôleur, comme pour l'amorçage (bootstrapping)
- le découplage des contrôleurs et des services — le contrôleur d'endpoints examine uniquement des pods
- la composition claire des fonctionnalités niveau Kubelet et des fonctionnalités niveau cluster — concrètement, Kubelet est le "contrôleur de pods"
- les applications hautement disponibles, qui attendront que les pods soient remplacés avant leur arrêt et au moins avant leur suppression, comme dans les cas d'éviction programmée ou de pré-chargement d'image.




- **Co-localisation des conteneurs** :  
    Les conteneurs d’un Pod peuvent **communiquer via localhost** et partagent IP et ports.
    
- **Stockage partagé** :  
    Les volumes montés dans un Pod peuvent être utilisés par tous les conteneurs de celui-ci.
    
- **Unité de déploiement** :  
    Kubernetes déploie et gère les **Pods**, pas les conteneurs individuellement.
    
- **Init Containers** :  
    Ce sont des conteneurs spéciaux qui s’exécutent **avant** les conteneurs principaux.
    

---

### —  CONTRÔLEURS

Les **contrôleurs** veillent à ce que l’état réel des ressources corresponde à l’état désiré. Ils automatisent la **scalabilité**, la **guérison automatique** et la **gestion des applications**.

- **Replication Controller** :  
    Garantit qu’un **nombre exact de Pods** est toujours en fonctionnement.
    
- **Deployment** :  
    Gère les **mises à jour progressives** (rolling updates) des applications et permet les **rollbacks**.
    
- **StatefulSet** :  
    Gère les applications **avec état** (base de données, etc.) en leur attribuant une **identité stable**.
    
- **DaemonSet** :  
    Fait en sorte qu’un **Pod spécifique tourne sur chaque nœud** (utile pour les logs, la surveillance...).
    
- **Horizontal Pod Autoscaler (HPA)** :  
    Ajuste automatiquement le nombre de Pods en fonction de la **charge CPU** ou d’autres métriques.
    
- **Vertical Pod Autoscaler (VPA)** :  
    Modifie les **ressources allouées** aux Pods selon leur consommation réelle.
    

---

### —  SERVICES

Les **Services** facilitent la communication entre Pods et assurent un **équilibrage de charge**.

1. **Types de services** :
    
    - **ClusterIP** : IP interne accessible uniquement dans le cluster.
        
    - **NodePort** : Expose le service sur un port fixe de chaque nœud.
        
    - **LoadBalancer** : Crée un load balancer externe pour exposer le service.
        
    - **ExternalName** : Redirige vers un **nom DNS externe**.
        
2. **Sélecteurs et étiquettes (labels)** :  
    Les Services ciblent les Pods via des **labels**.
    
3. **Load Balancing** :  
    Les requêtes envoyées à un Service sont **réparties entre les Pods** associés.
    
4. **Services Headless** :  
    Ne fournit **pas d’IP stable** mais permet un **accès direct aux Pods**.
    

---

### —  VOLUMES

Les **volumes** offrent un **stockage persistant** aux conteneurs.

1. **Types de volumes** :
    
    - **EmptyDir** : Temporaire, supprimé avec le Pod.
        
    - **HostPath** : Monte un fichier/dossier du nœud (non recommandé en production).
        
    - **PersistentVolumeClaim (PVC)** : Requête pour un volume persistant, dynamique.
        
    - **ConfigMap / Secret** : Injecte des fichiers de config ou des secrets.
        
    - **NFS** : Utilise un stockage réseau partagé.
        
    - **CSI** : Permet l’intégration de solutions de stockage tierces.
        

---

### — 🔐 CONFIGMAPS ET SECRETS

- **ConfigMaps** :  
    Stockent des **données de configuration non sensibles** sous forme de paires clé-valeur.
    
- **Secrets** :  
    Stockent des **informations sensibles** comme mots de passe, clés API, certificats… en toute sécurité.
    

---

### — 🧱 NAMESPACES

Les **namespaces** permettent d’organiser et d’isoler les ressources dans un cluster.

1. **Objectifs** :
    
    - **Isolation** des ressources par projet, équipe ou environnement.
        
    - **Gestion des accès** (RBAC).
        
    - **Quota de ressources**.
        
2. **Namespace par défaut** :  
    Si aucun namespace n’est précisé, les ressources sont créées dans le **namespace `default`**.
    
3. **Portée** :  
    Certaines ressources (Nodes, Volumes) sont **globales**, d'autres sont **propres à un namespace** (Pods, Services…).
    

---

### — 🌐 INGRESS

L’**Ingress** permet de **gérer l’accès externe** aux services du cluster, souvent via HTTP/HTTPS.

1. **Objectif** :  
    Agit comme **point d’entrée** du cluster pour exposer les applications.
    
2. **Fonctionnement** :  
    L’Ingress définit des **règles de routage** basées sur les noms de domaine ou chemins.
    
3. **Ingress Controller** :  
    C’est un composant (comme NGINX ou Traefik) qui **applique les règles** définies dans les Ingress.
    
Source :(https://medium.com/@himanshusangshetty/kubernetes-architecture-and-components-explained-e489e98db15d)

![[Kubernetes-Architecture.webp]]

*source :(https://www.opsramp.com/guides/why-kubernetes/kubernetes-architecture/)*
## 1. Concepts de base : Pod, Service, Deployment, Cluster

### [Kubernetes (K8s)](https://kubernetes.io/fr/docs/concepts/overview/) 


***est un système open-source permettant d'automatiser le déploiement, la mise à l'échelle et la gestion des applications conteneurisées***
Les conteneurs qui composent une application sont regroupés dans des unités logiques pour en faciliter la gestion et la découverte. Kubernetes s’appuie sur [15 années d’expérience dans la gestion de charges de travail de production (workloads) chez Google](https://queue.acm.org/detail.cfm?id=2898444), associé aux meilleures idées et pratiques de la communauté.

#### Quel que soit le nombre[](https://kubernetes.io/fr/#quel-que-soit-le-nombre)

Conçu selon les mêmes principes qui permettent à Google de gérer des milliards de conteneurs par semaine, Kubernetes peut évoluer sans augmenter votre équipe d'opérations.

#### Quelle que soit la complexité [](https://kubernetes.io/fr/#quelle-que-soit-la-complexit%C3%A9)

Qu'il s'agisse de tester localement ou d'une implémentation globale, Kubernetes est suffisamment flexible pour fournir vos applications de manière cohérente et simple, quelle que soit la complexité de vos besoins.

#### Quel que soit l'endroit[](https://kubernetes.io/fr/#quel-que-soit-l-endroit)

Kubernetes est une solution open-source qui vous permet de tirer parti de vos infrastructures qu'elles soient sur site (on-premises), hybride ou en Cloud publique. Vous pourrez ainsi répartir sans effort vos workloads là où vous le souhaitez.

Source Site Officielles :(https://kubernetes.io/fr/)
# **Orchestration avec Kubernetes : Concepts de base**

---

## 🔹 1. **Pod** – _L’unité de base dans Kubernetes_

- Un **Pod** est la plus petite unité déployable dans Kubernetes.
- Il représente **un ou plusieurs conteneurs** qui partagent :
    - Le **réseau** (même IP, même port space)
    - Le **stockage** (volumes partagés)
    - Et sont gérés ensemble.
- Les conteneurs d’un même Pod sont **fortement liés** (ex : un conteneur d’app + un conteneur de monitoring).

> 🎯 But : Grouper des conteneurs qui doivent fonctionner ensemble dans un même environnement isolé.

---

## 🔹 2. **Service** – _La couche réseau et de communication_

- Un **Service** est un **abstrait réseau** qui **expose un ou plusieurs Pods** via une adresse IP stable et un nom DNS.
- Il permet :
    - La **découverte de services** dans le cluster.
    - Le **load balancing** automatique entre Pods similaires.
    - L’**exposition vers l’extérieur** (si besoin).

> 🎯 But : Garantir que les applications puissent se parler même si les Pods changent ou redémarrent.

### Types de Services :

| Type             | Description                                     |
| ---------------- | ----------------------------------------------- |
| **ClusterIP**    | Accessible uniquement dans le cluster           |
| **NodePort**     | Ouvert sur un port des nœuds pour accès externe |
| **LoadBalancer** | Utilise un load balancer cloud (AWS, GCP…)      |

---

## 🔹 3. **Deployment** – _La stratégie de déploiement et de mise à jour_

- Un **Deployment** décrit **comment déployer, gérer et mettre à jour** un ensemble de Pods.
- Il spécifie :
    - Le **nombre de réplicas** (copies du Pod).
    - L’**image Docker** à utiliser.
    - Les **mises à jour sans interruption** (rolling updates).
    - Le **rollback automatique** en cas d’échec.

> 🎯 But : Assurer un déploiement fiable, scalable et maintenable de l’application.

En resume
```
Cluster
├── Nodes (machines)
│   └── Pods (exécutent les conteneurs)
│       └── Containers (ton app)
│
├── Deployment (gère combien de Pods doivent exister)
│
├── Service (expose les Pods via le réseau)
```
---

## 🔹 4. **Cluster** – _L’ensemble de l’environnement Kubernetes_

- Un **Cluster Kubernetes** est un ensemble de **machines (nœuds)** qui travaillent ensemble :
    - Le **Control Plane** (ou Master) gère la logique de l’orchestration.
    - Les **Worker Nodes** exécutent les Pods et les conteneurs.
- Le cluster est l’**environnement dans lequel tout tourne**.

> 🎯 But : Fournir une plateforme stable, automatisée et résiliente pour exécuter des applications conteneurisées.

![[Pasted image 20250423125658.png]]
## 2. Qu’est-ce que Minikube ?

**Minikube** est un outil qui permet de créer et de gérer un cluster Kubernetes local à nœud unique sur votre ordinateur personnel. Il est principalement utilisé pour l'apprentissage, le développement et les tests de configurations Kubernetes sans nécessiter une infrastructure complexe.

[*](https://webpick.info/minikube-mise-en-place-dun-cluster-kubernetes-a-noeud-unique/?utm_source=chatgpt.com)[https://webpick.info/minikube-mise-en-place-dun-cluster-kubernetes-a-noeud-unique/?utm_source=chatgpt.com*](https://webpick.info/minikube-mise-en-place-dun-cluster-kubernetes-a-noeud-unique/?utm_source=chatgpt.com*)

### 🧩 Fonctionnalités principales de Minikube

- **Cluster local à nœud unique** : Minikube exécute un cluster Kubernetes dans une machine virtuelle ou un conteneur sur votre machine locale.
- **Support multiplateforme** : Compatible avec Windows, macOS et Linux.
- **Facilité d'utilisation** : Installation simple et rapide, idéale pour les débutants souhaitant se familiariser avec Kubernetes.
- **Intégration avec kubectl** : Permet d'utiliser les commandes Kubernetes standard pour gérer le cluster.
- **Fonctionnalités Kubernetes complètes** : Prise en charge de DNS, NodePorts, ConfigMaps, Secrets, Ingress, et plus encore.

*source :[https://kubernetes.io/fr/docs/setup/learning-environment/minikube/*](https://kubernetes.io/fr/docs/setup/learning-environment/minikube/*)

*source : [https://minikube.sigs.k8s.io/?utm_source=chatgpt.com*](https://minikube.sigs.k8s.io/?utm_source=chatgpt.com*)

### ⚙️ Prérequis pour l'installation

- **Système d'exploitation** : Windows, macOS ou Linux.
- **Processeur** : 2 cœurs ou plus.
- **Mémoire** : 2 Go de RAM minimum.
- **Espace disque** : 20 Go d'espace libre.
- **Connexion Internet** : Requise pour télécharger les images nécessaires.
- **Gestionnaire de machines virtuelles ou de conteneurs** : Docker, VirtualBox, Hyper-V, KVM, etc.

*Sources : [https://minikube.sigs.k8s.io/docs/start/?utm_source=chatgpt.com*](https://minikube.sigs.k8s.io/docs/start/?utm_source=chatgpt.com*)

## 3. Simuler un cluster Kubernetes avec Minikube

#### 🔧 **1. Prérequis**


Avant d’installer Minikube, assure-toi d’avoir :

- **Une machine avec au moins 2 CPU, 2 Go RAM**
    
- **Un hyperviseur** : VirtualBox, Docker, Hyper-V, etc.
    
- **kubectl** : l'outil en ligne de commande pour Kubernetes

➤ Installer `kubectl` :
```
sudo apt update
sudo apt install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubectl

```

#### 🚀 **2. Installer Minikube**


```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 sudo install minikube-linux-amd64 /usr/local/bin/minikube
```


#### ▶️ **3. Démarrer un cluster Kubernetes local**

```
minikube start
```
Par défaut, Minikube utilise **Docker** comme moteur. Si tu veux utiliser un autre hyperviseur :
```
minikube start --driver=virtualbox
```

#### 🧪 **4. Tester que tout fonctionne**
```
kubectl get nodes
```

#### 🧰 **5. Déployer une application de test**

```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080
minikube service hello-minikube
```

#### 📊 **6. Accéder au tableau de bord Kubernetes**
```
minikube dashboard
```



# III. Authentification et sécurité

## 1. Rôle de Keycloak : gestion des identités.

### ➤ Qu’est-ce que Keycloak ?

**Keycloak**, développé par Red Hat, a pour objectif principal est d’assurer la sécurité et la facilité d’utilisation des applications en fournissant des fonctionnalités robustes d’authentification et d’autorisation.

*selon le site(https://medium.com/@n.benamor97/comprendre-les-principes-fondamentaux-de-keycloak-50f66267d446)*

**Keycloak** est une solution **open source** de gestion des identités (IAM - Identity and Access Management). 

Il gère :
- L’authentification des utilisateurs (login/password, SSO…)
- L’autorisation d’accès aux applications
- La fédération avec d’autres providers (LDAP, Active Directory, Google, etc.)
- L’émission de **tokens OAuth2 / OpenID Connect**

### ➤ Rôles principaux :

- **SSO (Single Sign-On)** : un seul login pour accéder à plusieurs services.
- **Fédération d'identité** : centralisation des identités provenant de plusieurs sources.
- **Gestion fine des rôles et permissions**.
- **Intégration OAuth2 / OpenID Connect (OIDC)**.
    

### ➤ Pourquoi l’utiliser dans un cloud privé ?

- Sécurise l'accès aux applications internes (Dashboard, API…)
- Permet de déléguer l’authentification à un service robuste
- S’intègre bien avec Kubernetes, Docker, Spring Boot, etc.

## 2. Mécanisme OAuth2 et JWT

### ➤ OAuth2

**OAuth 2.0**, successeur du protocole OAuth 1.0 a, est **un framework d’autorisation** permettant à une application tierce d’accéder à un service web.

**OAuth2** est un **protocole d’autorisation** permettant à une application d’accéder aux ressources d’un utilisateur sans connaître son mot de passe.

#### Les rôles principaux :

- **Propriétaire de la ressource(Resource Owner)** : l’utilisateur
- **Client** : l’application qui demande l’accès
- **Serveur d’autorisation(Authorization Server)** : Keycloak dans ton cas
- **Serveur de ressource(Resource Server)** : l’API ou le service protégé


![[Pasted image 20250424094310.png]]
*image de simulation*

### 🔍 Décryptons étape par étape :
---

### 👤 **1. Le Propriétaire de la ressource (Resource Owner)**
C’est la **personne qui possède la photo**. Il est celui qui donne l’autorisation à une application (le client) pour y accéder.

---

### 📱 **2. Le Client (Application tierce)**
C’est par exemple une app mobile ou un site web qui **veut accéder à la photo** du propriétaire, stockée sur un serveur tiers (ex: Google Photos).

---

### 🖥️ **3. Les Serveurs**
Ils sont divisés en deux parties :
- **Serveur d’autorisation** : celui qui gère les autorisations (OAuth).
- **Serveur de ressources** : celui qui héberge la photo ou la ressource demandée.

---
#### Flows courants :

- **Authorization Code Flow** : recommandé pour les apps web
- **Client Credentials Flow** : utilisé pour la communication entre services
- **Password Grant** (déconseillé sauf cas très spécifiques)
    

### ➤ JWT (JSON Web Token)

Les **JWT** sont des **tokens** (jetons) générés par Keycloak (ou tout serveur OAuth2) pour prouver l’identité de l’utilisateur.

#### Composition d’un JWT :

1. **Header** : algorithme de signature
2. **Payload** : les claims (identité, rôles, durée…)
3. **Signature** : permet de vérifier que le token n’a pas été modifié.
Exemple:
```
{
  "sub": "user123",
  "name": "Alice",
  "roles": ["admin"],
  "exp": 1682542800
}
```
## 3. Fonctionnement des tokens d’authentification.

### ➤ Cycle typique :

1. L’utilisateur se connecte via une page gérée par Keycloak.
2. Keycloak authentifie l’utilisateur et génère :
    
    - un **access token (JWT)** : pour accéder aux API
    - un **refresh token** : pour renouveler le token sans reconnecter
3. Le token est envoyé au client (navigateur, appli, etc.)
4. À chaque requête, le token est envoyé dans l’entête HTTP :

    ```
     Authorization: Bearer eyJhbGciOi...
    ```
5. Le serveur (API ou service) vérifie la signature et les permissions dans le token.

### ➤ Avantages :

- Sans état (stateless) : tout est dans le token
- Facile à intégrer avec des middlewares ou des API Gateway
- Sécurité renforcée via HTTPS + expiration automatique

| Élément      | Rôle principal                                   |
| ------------ | ------------------------------------------------ |
| **Keycloak** | Authentifie les utilisateurs, délivre des tokens |
| **OAuth2**   | Protocole d’autorisation                         |
| **JWT**      | Format de token sécurisé et vérifiable           |

# IV. API et réseaux
![[Pasted image 20250423160953.png]]
## 1. Architecture d’une API REST

L'architecture d'une **API REST** (REpresentational State Transfer) est basée sur un ensemble de contraintes qui visent à créer des services web ou des applications. Ces contraintes incluent une architecture client-serveur, une communication sans état, la possibilité de stocker des données en cache, une interface uniforme entre les composants et des messages auto-déscriptifs.24

Une **API REST** doit respecter ces critères pour être considérée comme **RESTful** : elle doit avoir une architecture client-serveur composée de clients, de serveurs et de ressources, avec des requêtes gérées par HTTP. La communication client-serveur doit être sans état, ce qui signifie que les informations du client ne sont pas stockées entre les requêtes GET et que chaque requête est indépendante et non connectée.4

L'interface uniforme entre les composants est essentielle car elle permet la communication entre le client et le serveur dans un langage unique, indépendamment de l'architecture backend. Cela inclut l'utilisation d'HTTP avec des ressources URI, **CRUD (Create, Read, Update, Delete)** et JSON.6

Les contraintes REST permettent de créer une API qui n'est pas dictée par son architecture, mais par les représentations qu'elle retourne. Bien que l'API soit architecturalement sans état, elle dépend des représentations pour définir l'état de l'application.6

Une API REST peut être utilisée pour développer des services web ou pour se connecter à des applications cloud. Par exemple, l'API Graph de Facebook est un bon exemple d'API REST, permettant aux logiciels de communiquer avec l'application Facebook pour publier des messages, recueillir des données et gérer des annonces.5

Les API REST sont les plus utilisées dans les architectures distribuées ou basées sur des services, car elles sont indépendantes du type de plateforme ou des langages utilisés entre le client et le serveur.5

Les contraintes REST incluent également la possibilité de stocker des données en cache pour réduire le nombre d'interactions avec l'API, réduire l'utilisation interne du serveur et fournir aux utilisateurs de l'API les outils nécessaires pour fournir les applications les plus rapides et les plus efficaces possibles.
#### **Principes clés** :

- **Client-Serveur** : Séparation claire entre le client (frontend) et le serveur (backend).
- **Sans état (Stateless)** : Chaque requête contient toutes les informations nécessaires (ex : token d’authentification).
- **Ressources** : Les données sont exposées comme des **ressources** identifiées par des URLs (ex : `/users`, `/products`).
- **Verbes HTTP** : Utilisation des méthodes HTTP pour les opérations :
    
    - `GET` : Lire une ressource.
    - `POST` : Créer une ressource.
    - `PUT`/`PATCH` : Mettre à jour.
    - `DELETE` : Supprimer.
        
- **Représentations** : Les réponses sont souvent en JSON ou XML.
    
```
GET /users → liste tous les utilisateurs
POST /users → ajoute un nouvel utilisateur
GET /users/1 → affiche l’utilisateur avec l’ID 1
PUT /users/1 → modifie l’utilisateur 1
DELETE /users/1 → supprime l’utilisateur 1
```
#### **Exemple d’endpoint REST** :


## 2. Modèle CRUD (Create, Read, Update, Delete)

### 📌 Définition :

CRUD est un acronyme qui décrit les **4 opérations de base** pour manipuler des données.

| Opération  | Méthode HTTP     | Action              | Exemple URL   |
| ---------- | ---------------- | ------------------- | ------------- |
| **Create** | `POST`           | Créer une ressource | `/articles`   |
| **Read**   | `GET`            | Lire une ressource  | `/articles/1` |
| **Update** | `PUT` ou `PATCH` | Mettre à jour       | `/articles/1` |
| **Delete** | `DELETE`         | Supprimer           | `/articles/1` |
#### Demandes de **GET**

Une requête **GET** récupère une représentation de la ressource au niveau de l’URI spécifié. Le corps du message de réponse contient les détails de la ressource demandée.

Une requête **GET** doit retourner l’un des codes d’état HTTP suivants :

| Code d’état HTTP     | Motif                                                                                                                                                 |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| 200 (OK)             | La méthode a retourné la ressource avec succès.                                                                                                       |
| 204 (Pas de contenu) | Le corps de la réponse ne contient aucun contenu, par exemple lorsqu’une requête de recherche ne retourne aucune correspondance dans la réponse HTTP. |
| 404 (Introuvable)    | La ressource demandée est introuvable.                                                                                                                |
#### Demandes de **POST**

Une requête **POST** doit créer une ressource. Le serveur affecte un URI pour la nouvelle ressource et retourne cet URI au client.

Importante

Pour les requêtes **POST**, un client ne doit pas tenter de créer son propre URI. Le client doit soumettre la demande à l’URI de la collection, et le serveur doit affecter un URI à la nouvelle ressource. Si un client tente cette opération et émet une requête **POST** vers un URI spécifique, le serveur retourne le code d’état HTTP 400 (DEMANDE INCORRECTE) pour indiquer que la méthode n’est pas prise en charge.

Dans un modèle RESTful, les requêtes **POST** sont généralement utilisées pour ajouter une nouvelle ressource à la collection identifiée par l’URI. Toutefois, une demande **POST** peut également être utilisée pour envoyer des données à des fins de traitement à une ressource existante, sans aucune nouvelle ressource créée.

Une requête **POST** doit retourner l’un des codes d’état HTTP suivants :

|Code d’état HTTP|Motif|
|---|---|
|200 (OK)|La méthode a effectué un traitement, mais ne crée pas de ressource. Le résultat de l’opération peut être inclus dans le corps de la réponse.|
|201 (créé)|La ressource a été créée avec succès. L’URI de la nouvelle ressource est inclus dans l’en-tête Location de la réponse. Le corps de la réponse contient une représentation de la ressource.|
|204 (Pas de contenu)|Le corps de la réponse ne contient aucun contenu.|
|400 (Requête incorrecte)|Le client a placé des données non valides dans la requête. Le corps de la réponse peut contenir plus d’informations sur l’erreur ou un lien vers un URI qui fournit plus de détails.|
|405 (Méthode non autorisée)|Le client a tenté d’envoyer une requête POST à un URI qui ne prend pas en charge ces demandes.|

#### Requête de **PUT**

Une demande **PUT** doit mettre à jour une ressource existante s’il existe ou, dans certains cas, créer une ressource si elle n’existe pas. Le processus d’établissement d’une demande **PUT** est le suivant :

1. Le client spécifie l’URI de la ressource et inclut le corps de la demande qui contient une représentation complète de la ressource.
2. Le client effectue la requête.
3. Si une ressource avec cet URI existe déjà, elle est remplacée. Sinon, une nouvelle ressource est créée si l’itinéraire prend en charge cette opération.

Les méthodes **PUT** sont généralement appliquées aux ressources qui sont des éléments individuels, tels qu’un client spécifique, plutôt que des collections. Un serveur peut prendre en charge les mises à jour, mais pas la création via **PUT**. La prise en charge de la création via **PUT** dépend du fait que le client peut affecter de manière significative et fiable un URI à une ressource avant que celle-ci n'existe. Si ce n’est pas le cas, utilisez **POST** pour créer des ressources et demandez au serveur d’affecter l’URI, puis utilisez **PUT** ou **PATCH** pour mettre à jour.

Importante

Les requêtes PUT doivent être idempotentes. Si un client envoie la même requête **PUT** plusieurs fois, les résultats doivent toujours être identiques (la même ressource sera modifiée avec les mêmes valeurs). Les requêtes **POST** et **PATCH** ne sont pas garanties d’être idempotentes.

Une requête **PUT** doit retourner l’un des codes d’état HTTP suivants :

|Code d’état HTTP|Motif|
|---|---|
|200 (OK)|La ressource a été mise à jour avec succès.|
|201 (créé)|La ressource a été créée avec succès. Le corps de la réponse peut contenir une représentation de la ressource.|
|204 (Pas de contenu)|La ressource a été mise à jour avec succès, mais le corps de la réponse ne contient aucun contenu.|
|409 (Conflit)|Impossible de terminer la requête en raison d’un conflit avec l’état actuel de la ressource.|

Conseil / Astuce

Envisagez d’implémenter des opérations HTTP PUT en bloc qui peuvent effectuer des mises à jour par lots vers plusieurs ressources d’une collection. La requête PUT doit spécifier l’URI de la collection, et le corps de la demande doit spécifier les détails des ressources à modifier. Cette approche peut aider à réduire les chattines et à améliorer les performances.

#### requêtes **PATCH**

Une requête PATCH effectue une _mise à jour partielle_ vers une ressource existante. Le client spécifie l’URI de la ressource. Le corps de la demande spécifie un ensemble de _modifications_ à appliquer à la ressource. Cela peut être plus efficace que l’utilisation de PUT, car le client envoie uniquement les modifications, et non la représentation entière de la ressource. Techniquement, PATCH peut également créer une ressource (en spécifiant un ensemble de mises à jour vers une ressource « null »), si le serveur prend en charge cette opération.

Avec une requête PATCH, le client envoie un ensemble de mises à jour à une ressource existante, sous la forme d’un _document patch_. Le serveur traite le document de correctif pour effectuer la mise à jour. Le document patch ne décrit pas l’ensemble de la ressource, seul un ensemble de modifications à appliquer. La spécification de la méthode PATCH ([RFC 5789](https://tools.ietf.org/html/rfc5789)) ne définit pas un format particulier pour les documents patch. Le format doit être déduit du type de média dans la requête.

JSON est probablement le format de données le plus courant pour les API web. Il existe deux formats de correctifs JSON principaux, appelés _correctifs JSON_ et _correctif de fusion JSON_.

Le correctif de fusion JSON est un peu plus simple. Le document patch a la même structure que la ressource JSON d’origine, mais inclut uniquement le sous-ensemble de champs qui doivent être modifiés ou ajoutés. En outre, un champ peut être supprimé en spécifiant `null` la valeur du champ dans le document patch. (Cela signifie que le correctif de fusion ne convient pas si la ressource d’origine peut avoir des valeurs null explicites.)

Par exemple, supposons que la ressource d’origine a la représentation JSON suivante :

```
{ 
  "name":"gizmo", 
  "category":"widgets", 
  "color":"blue", 
  "price":10 
}
```

Voici un correctif de fusion JSON possible pour cette ressource :

```
{
    "price":12,
    "color":null,
    "size":"small"
}
```
Cela indique au serveur de mettre à jour `price`, supprimer `color`et ajouter `size`, pendant `name` et `category` ne sont pas modifiés. Pour obtenir les détails exacts du correctif de fusion JSON, consultez [RFC 7396](https://tools.ietf.org/html/rfc7396). Le type de média pour le correctif de fusion JSON est `application/merge-patch+json`.

Le patch de fusion ne convient pas si la ressource d'origine peut contenir des valeurs nulles explicites, en raison de la signification spéciale du `null` dans le document patch. En outre, le document de correctif ne spécifie pas l’ordre dans lequel le serveur doit appliquer les mises à jour, ce qui peut ne pas être important, en fonction des données et du domaine. Le correctif JSON, défini dans [RFC 6902](https://tools.ietf.org/html/rfc6902), est plus flexible, car il spécifie les modifications en tant que séquence d’opérations à appliquer. Les opérations incluent l’ajout, la suppression, le remplacement, la copie et le test (pour valider les valeurs). Le type de média pour le correctif JSON est `application/json-patch+json`.

Une requête PATCH doit retourner l’un des codes d’état HTTP suivants :

|Code d’état HTTP|Motif|
|---|---|
|200 (OK)|La ressource a été mise à jour avec succès.|
|400 (Requête incorrecte)|Document de correctif incorrect.|
|409 (Conflit)|Le document patch est valide, mais les modifications ne peuvent pas être appliquées à la ressource dans son état actuel.|
|415 (type de média non pris en charge)|Le format de document de correctif n’est pas pris en charge.|

#### Demandes de suppression

Une demande DELETE supprime la ressource à l’URI spécifié.

Une requête DELETE doit retourner l’un des codes d’état HTTP suivants :

|Code d’état HTTP|Motif|
|---|---|
|204 (AUCUN CONTENU)|La ressource a été supprimée avec succès. Le processus a été géré avec succès et le corps de la réponse ne contient aucune information supplémentaire.|
|404 (INTROUVABLE)|La ressource n’existe pas.|

source:https://learn.microsoft.com/fr-fr/azure/architecture/best-practices/api-design
## 3. Fonctionnement d’un Reverse Proxy (ex : NGINX)

### 📌 Définition :

Un **reverse proxy** est un **intermédiaire entre les clients et les serveurs**. Contrairement à un **proxy classique (forward proxy)** qui cache les utilisateurs, un **reverse proxy cache les serveurs**.

👉 Il reçoit les requêtes des clients et les redirige vers le bon service interne (ex : une API, un site web, un conteneur Docker).
![[1_DhHPPuJov8kN2mjiX0WCMw.webp]]

*selon Medium*
> Un proxy inverse (reverse proxy) est un type de serveur, habituellement placé en frontal de serveurs web. Contrairement au serveur proxy qui permet à un utilisateur d’accéder au réseau Internet, le proxy inverse permet à un utilisateur d’Internet d’accéder à des serveurs internes. Une des applications courantes du proxy inverse est la répartition de charge (load-balancing).
### 🔧 Il sert à :

- **Rediriger les requêtes** vers le bon service (load balancing)
    
- **Cacher certaines ressources** pour les charger plus vite (cache)
    
- **Sécuriser les accès** (SSL, firewall, authentification)
    
- **Masquer l’architecture interne** au client

## 🔍 Pourquoi utiliser un reverse proxy ?

Voici les **fonctions clés** :

|Fonction|Description|
|---|---|
|🎯 **Routage intelligent**|Dirige les requêtes selon le chemin, le domaine ou l’URL|
|📦 **Répartition de charge**|Distribue les requêtes vers plusieurs serveurs (load balancing)|
|🔐 **SSL/HTTPS**|Gère les certificats SSL pour sécuriser le trafic|
|🚪 **Accès restreint**|Bloque ou filtre les connexions (firewall applicatif)|
|🚀 **Cache**|Stocke des réponses pour accélérer les réponses (reverse caching)|
|🛠️ **Simplifie la config**|Un seul point d’entrée, même si tu as 10 apps derrière|
## 🧠 Fonctionnement schématique

```
    Navigateur client (Internet)
                |
                ↓
      🔐 NGINX Reverse Proxy (443/80)
      ┌────────────────────────────┐
      │     Routage, SSL, cache    │
      └────────────────────────────┘
           |           |         |
      ↙︎        ↘︎       ↘︎
 App 1     API REST     Interface Admin
(3000)     (4000)         (5000)

```



---
### 📊 Exemple simple avec NGINX :

Imagine un utilisateur qui tape `www.toncloud.com`. Voici ce que fait NGINX :
```
Navigateur → NGINX (port 443)
                   ↓
        Redirige vers ton app (port 3000)
```

```
server {
    listen 443 ssl;
    server_name moncloud.local;

    location / {
        proxy_pass http://localhost:3000;
    }
}

```
*source:(https://search.brave.com/search?q=Architecture+d%E2%80%99une+API+REST&source=desktop&conversation=1b9db0d6df3a79089fef2a&summary=1)*

Un reverse proxy NGINX transmet les requêtes des clients aux serveurs et vice-versa. Il agit comme un pont entre les appareils clients et les serveurs dorsaux, tels que LiteSpeed ou Apache, en gérant les requêtes entrantes dans une configuration de proxy inverse. Lorsqu’un appareil client envoie des requêtes HTTP à votre application web, ces requêtes atteignent d’abord le serveur reverse proxy NGINX, qui examine ensuite les détails de la requête, tels que l’URL et les en-têtes, afin de déterminer le traitement approprié.23

Pour configurer NGINX en tant que reverse proxy, vous devez créer un nouveau fichier de configuration. Ce fichier contiendra les blocs du serveur et les directives nécessaires au routage des requêtes. Voici un exemple de configuration de base :

```
server {
    listen 80;
    server_name example.com;
    location / {
        proxy_pass http://your_backend_server_ip;
        proxy_set_header Host $host; # Forwarded host
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_redirect off;
    }
}
```

Dans cet exemple, remplacez `your_backend_server_ip` par l’adresse IP réelle de votre serveur back-end.2

Pour mettre en place un équilibreur de charge, définissez un bloc `upstream` et utilisez `proxy_pass` dans votre bloc `serveur` pour répartir le trafic entre plusieurs serveurs.

Les avantages d'utiliser NGINX comme reverse proxy incluent la réduction de la charge sur les serveurs d'applications, la navigation à travers les pare-feu qui protègent les ressources en arrière-plan, la possibilité de servir du contenu statique pour réduire la charge sur les serveurs d'applications, et la simplification de la gestion de l'accès utilisateur avec un point d'accès unique à votre site.

source:(https://search.brave.com/search?q=3.+Fonctionnement+d%E2%80%99un+Reverse+Proxy+%28ex+%3A+NGINX%29&source=desktop&conversation=00a54a3b7afe2b909b8ad2&summary=1)

# V. Gestion de code source avec Git

## 1. Principes de Git : dépôt, commit, push, pull, merge

## 2. GitFlow Workflow : organisation des branches

# VI. Stockage et fonctions serverless

## 1. Qu’est-ce que MinIO : stockage d’objets compatible S3

## 2. Introduction à OpenFaaS et Knative : fonctions serverless sur Kubernetes

# VII. Supervision et journalisation

## 1. Centralisation des logs avec Grafana et Loki

## 2. Collecte et visualisation des logs applicatifs

# VIII. Système d’exploitation pour les serveurs

## 1. Comparaison : Ubuntu Server, CentOS, Debian, Rocky Linux, etc.


### 🏆 Classement des distributions Linux pour serveurs

| Rang | Distribution                        | Points forts                                                                                                                                                                                                    | Cas d'utilisation typique                                                                                |
| ---- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| 1️⃣  | **Ubuntu Server (LTS)**             | - Très populaire et largement utilisée- Facilité d'installation et d'utilisation- Intégration fluide avec les plateformes cloud comme AWS et Azure- Support LTS de 5 ans, idéal pour les environnements stables | Cloud privé/public, DevOps, hébergement web, conteneurs, serveurs d'applications                         |
| 2️⃣  | **Debian**                          | - Réputée pour sa stabilité et sa robustesse- Gestion efficace des paquets avec APT- Base de nombreuses autres distributions- Documentation complète                                                            | Serveurs critiques, infrastructures cloud, conteneurs, environnements nécessitant une grande stabilité   |
| 3️⃣  | **Red Hat Enterprise Linux (RHEL)** | - Support professionnel avec mises à jour régulières- Sécurité renforcée et conformité aux normes- Intégration avec diverses solutions logicielles- Support étendu de 10 ans sur le système de base             | Entreprises, data centers, cloud hybride, environnements nécessitant un support professionnel            |
| 4️⃣  | **AlmaLinux**                       | - Successeur communautaire de CentOS- 100% compatible avec RHEL- Support jusqu'en 2029- Soutenu par des institutions comme le CERN et le Fermilab                                                               | Environnements d'entreprise, cloud, hébergeurs, utilisateurs recherchant une alternative gratuite à RHEL |
| 5️⃣  | **Rocky Linux**                     | - Alternative communautaire à CentOS- Compatibilité totale avec RHEL- Cycle de vie de support de 10 ans- Développement intensif par la communauté                                                               | Entreprises, calcul haute performance (HPC), environnements critiques nécessitant stabilité et support   |
| 6️⃣  | **CentOS Stream**                   | - Version rolling release de RHEL- Reçoit les mises à jour avant RHEL- Bon pour les tests et le développement                                                                                                   | Développement, pré-production, environnements nécessitant des mises à jour fréquentes                    |
