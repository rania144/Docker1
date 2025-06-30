# Utilisation d'Ansible dans le projet

## Prérequis

- **Ansible** : Installer Ansible sur votre machine bastion.  
  Documentation officielle : [https://docs.ansible.com/ansible/latest/installation_guide/index.html](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

---

## À quoi sert Ansible dans ce projet ?

Ansible est un outil d'automatisation de la configuration, du déploiement et de la gestion des infrastructures.  
Dans ce projet, Ansible est utilisé pour :

- **Déployer** les applications et services via les playbooks YAML (`deploy.yml`, `docker.yml`)
- **Gérer la sécurité** des machines avec des playbooks comme `security.yml`, `ufw.yml`
- **Configurer le monitoring** à l'aide de `monitoring.yml`
- **Gérer les utilisateurs SSH** via `ssh-user.yml`
- **Mettre à jour et maintenir** les serveurs avec `update.yml`

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






