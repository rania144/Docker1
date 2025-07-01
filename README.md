##  Résumé global du déploiement et de la supervision du site Arcdata

Le projet **Arcdata** repose sur une infrastructure Docker automatisée via **Ansible**, permettant de déployer et superviser un site web de manière entièrement intégrée.

Le **Dockerfile** joue un rôle central : il permet de créer une image personnalisée contenant le site web **Arcdata**, qui est ensuite utilisée dans le fichier `docker-compose.yml` pour démarrer le service **arcdata-site**. Ce service expose le **port 80 du conteneur** sur le **port 8080 de la machine hôte**, rendant le site accessible à l'adresse [`http://localhost:8080`](http://localhost:8080).

L’infrastructure comprend également des services de supervision essentiels tels que **Prometheus**, **Grafana** et **Blackbox Exporter**, qui surveillent en continu le bon fonctionnement du site :

-  **Prometheus** collecte les métriques.
-  **Grafana** permet de les visualiser sous forme de tableaux de bord.
-  **Blackbox Exporter**, intégré à Prometheus, teste la **disponibilité du site** Arcdata depuis l’extérieur.

Pour orchestrer l’ensemble, **Ansible** est utilisé afin d’automatiser :
- l'installation de Docker,
- la configuration de `docker-compose`,
- et le déploiement des conteneurs via des **playbooks**.

Grâce à cela, tout l’environnement peut être mis en place et configuré rapidement, sans intervention manuelle.

De plus, **Prometheus** utilise des **règles d’alerte**, notamment pour signaler si le site est **inaccessible pendant plus de 30 secondes**, permettant ainsi d’être informé immédiatement d’un incident critique via des canaux de notification (emails, Slack, etc.).

>  En résumé, ce système permet de :
> - **Déployer le site web Arcdata de façon automatisée**
> - **Le superviser en temps réel**
> - **Réagir rapidement en cas d’indisponibilité**, assurant ainsi une **haute fiabilité** du service.
