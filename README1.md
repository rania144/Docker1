# Utilisation d'Ansible dans le projet

## Prérequis
- **Posséder un compte AWS**  
- **Installer AWS-CLI** : Pour gérer vos ressources AWS via la ligne de commande, installez AWS CLI et configurez-le avec aws configure. Lors de la configuration, entrez votre AWS Access Key ID, AWS Secret Access Key que vous allez trouver dans Vous pouvez installer AWS CLI , puis le configurer avec vos identifiants AWS, disponibles dans la section "AWS details" de votre compte AW, la région et le format de sortie. Une fois configuré, vous pouvez l'utiliser pour gérer vos instances EC2.
- - **Informations d'identification AWS** : Configurer les informations d'identification AWS dans `./aws/credentials` car elle sera utilisée par la suite avec Ansible.

- **Ansible** : Installer Ansible sur votre machine .  
  Documentation officielle : [https://docs.ansible.com/ansible/latest/installation_guide/index.html](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

---

## À quoi sert Ansible dans ce projet ?

Ansible est un outil d'automatisation de la configuration, du déploiement et de la gestion des infrastructures.  
Dans ce projet, Ansible est utilisé pour :

# Explications des playbooks

---

### 1. Playbook principal d’importation (security.yml)

Ce playbook principal importe plusieurs autres playbooks qui couvrent différentes tâches d’administration système : mise à jour des serveurs (`update.yml`), gestion des utilisateurs SSH (`ssh-user.yml`), configuration du pare-feu UFW (`ufw.yml`), installation et gestion de Docker (`docker.yml`), et mise en place du monitoring (`monitoring.yml`). Cela permet d’organiser et d’exécuter toutes ces opérations dans un ordre structuré.

---

### 2. Installation et activation de fail2ban et auditd (monitoring.yml)

Ce playbook installe deux outils importants pour la sécurité : **fail2ban** (qui protège contre les tentatives d’intrusion en bloquant les adresses IP suspectes) et **auditd** (qui collecte des logs détaillés du système pour la surveillance et les audits). Après l’installation, il démarre et active ces services afin qu’ils fonctionnent automatiquement au démarrage du système.

---

### 3. Désactivation de l’accès SSH root (user.yml)

Ce playbook renforce la sécurité SSH en modifiant la configuration pour interdire la connexion directe en tant que `root` via SSH. Il modifie le fichier `sshd_config` pour mettre `PermitRootLogin no`, puis redémarre le service SSH afin que ce changement soit pris en compte immédiatement.

---

### 4. Configuration du pare-feu UFW (ufw.yml)

Ce playbook configure le pare-feu **UFW** (Uncomplicated Firewall) pour autoriser les connexions sur les ports essentiels : SSH (22), HTTP (80) et HTTPS (443). Après avoir ajouté ces règles, il active le pare-feu pour que la protection soit effective.

---

### 5. Mise à jour des paquets sur tous les serveurs (update.yml)

Ce playbook met à jour les serveurs en actualisant la liste des paquets disponibles (`update_cache: yes`) puis en lançant une mise à niveau complète des paquets installés (`upgrade: dist`). Cette maintenance régulière est essentielle pour la sécurité et la stabilité des serveurs.
### 6.Explication du playbook de déploiement Docker pour Arcdata (deploy.yml)

Ce playbook automatise le déploiement de la stack Docker pour le projet Arcdata sur tous les serveurs ciblés (`hosts: all`) avec les droits administrateur (`become: yes`).

## Explication rapide du playbook de déploiement Docker

- Crée le dossier du projet si besoin  
- Copie les fichiers `docker-compose.yml` et `prometheus.yml` dans ce dossier  
- Ajoute l’utilisateur `ubuntu` au groupe `docker`  
- Lance la stack Docker Compose en arrière-plan dans le dossier projet  

Ce playbook assure que la configuration nécessaire est en place et que la stack Docker fonctionne correctement sur la machine distante.

Tous ces playbooks sont orchestrés par Ansible pour automatiser et standardiser les opérations, facilitant ainsi la gestion de l'infrastructure.

---

## Configuration du plugin `aws_ec2`

Ce fichier permet à Ansible de récupérer automatiquement les instances EC2 en cours d’exécution dans la région `us-east-1`.  
Les hôtes seront nommés par leur IP privée, mais Ansible se connectera via leur IP publique.

---

## Commandes utiles

### Tester la connexion (ping) aux instances AWS EC2

```bash
ansible -i aws_ec2.yml all -m ping
 ```

---
### Execution des playbooks
```bash
ansible-playbook -i aws_ec2.yml <nom_du_playbook>.yml
 ```

---

## Résumé
Ansible automatise la gestion des serveurs pour le projet Arcdata en installant, configurant et sécurisant tout automatiquement. Plusieurs playbooks sont utilisés pour mettre à jour les serveurs, gérer les utilisateurs SSH, configurer le pare-feu, installer Docker, surveiller les services, et renforcer la sécurité. Le fichier aws_ec2.yml récupère les instances AWS actives, et on lance les playbooks avec la commande ansible-playbook. Cela rend l’administration des serveurs Arcdata plus simple, rapide et sécurisée.








