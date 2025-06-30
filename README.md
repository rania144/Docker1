# Utilisation d'Ansible dans le projet

## Prérequis

- **Ansible** : Installer Ansible sur votre machine bastion.  
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

### Execution des playbooks
```bash
ansible-playbook -i aws_ec2.yml <nom_du_playbook>.yml







