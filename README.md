# Utilisation d'Ansible dans le projet


Ansible est un outil d'automatisation de la configuration, du déploiement et de la gestion des infrastructures.  
Dans ce projet, Ansible est utilisé pour :

- **Déployer** les applications et services via les playbooks YAML (`deploy.yml`, `docker.yml`, etc.)
- **Gérer la sécurité** des machines avec des playbooks comme `security.yml`, `ufw.yml`
- **Configurer le monitoring** à l'aide de `monitoring.yml`
- **Gérer les utilisateurs SSH** via `ssh-user.yml`
- **Mettre à jour et maintenir** les serveurs avec `update.yml`
- **Déployer sur AWS EC2** grâce au playbook `aws_ec2.yml`

Tous ces playbooks sont orchestrés par Ansible pour automatiser et standardiser les opérations, facilitant ainsi la gestion de l'infrastructure.

---

## Comment installer Ansible ?

Ansible peut être installé facilement via `pip` (Python package manager) ou via le gestionnaire de paquets de votre système.

### Installation avec pip (recommandée)

```bash
pip install ansible
