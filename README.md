# Utilisation d'Ansible dans le projet
## Prérequis
- **Ansible** : Installer Ansible sur votre machine bastion. [Documentation Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

Ansible est un outil d'automatisation de la configuration, du déploiement et de la gestion des infrastructures.  
Dans ce projet, Ansible est utilisé pour :

- **Déployer** les applications et services via les playbooks YAML (`deploy.yml`, `docker.yml`)
- **Gérer la sécurité** des machines avec des playbooks comme `security.yml`, `ufw.yml`
- **Configurer le monitoring** à l'aide de `monitoring.yml`
- **Gérer les utilisateurs SSH** via `ssh-user.yml`
- **Mettre à jour et maintenir** les serveurs avec `update.yml`
- **Déployer sur AWS EC2** grâce au playbook `aws_ec2.yml`

Tous ces playbooks sont orchestrés par Ansible pour automatiser et standardiser les opérations, facilitant ainsi la gestion de l'infrastructure.

---

## 2. Installation d'Ansible 

Pour installer Ansible, voici les commandes à exécuter dans un terminal Ubuntu :

```bash
sudo apt update
sudo apt install ansible
ansible --version
