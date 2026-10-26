1/ Question & Answer:

Quelle est la différence entre le modèle pull (Prometheus) et push ?

La difference est que le modele pull de Prometheus lui permet de recupere directement les metrics, il n'a pas besoin de les recevoir contrairement a un modèle push qui lui doit attendre de recevoir les metrics pour les traiter

Quels sont les 4 types de métriques (counter, gauge, histogram, summary) ? Donne un exemple concret de chacun pour une app comme 
Transcendence (ex. : nombre de parties jouées, joueurs connectés…).

Counter: cette metric est un compteur qui ne peut augmenter ou être reset à 0. (On pourrait par exemple regarder le nombre de gens inscrit sur le site)

Gauge: C'est comme un une jauge sur le tableau de bord d'une voiture. Elle peut aller de haut en bas comme l'aiguille d'un indicateur de vitesse.(On pourrait regarder la mémoire utilisé en cours par exemple)

Histogram: enregistre les observations (généralement des éléments comme la durée des requêtes ou la taille des réponses) en les comptant dans des buckets configurables.

Summary : À l'instar d'un Histogram, un Summary échantillonne les observations (généralement des éléments comme la durée des requêtes et la taille des réponses). Bien qu'il fournisse également un compte total des observations et la somme de toutes les valeurs observées, il calcule des quantiles configurables sur une fenêtre temporelle glissante.

Qu’est-ce qu’un label et pourquoi une forte cardinalité est dangereuse ?

Ne sais pas.

À quoi sert un exporter ?

Les exporters sont des binaires qui permettent de collecter la donnée au niveau local et de les afficher sur les routes de prometheus.

2/ Launch Prometheus with docker:

To launch Prometheus with Docker i need to first pull Docker Image of prometheus from Docker Hub:
`docker pull prom/prometheus:[tag]`

In order to get my /prometheus/prometheus.yml in Promtheus container i need to bind mont a volume to the directory file where the default config is in the container: 
`--volume [absolute_path_of_prometheus.yml]:/etc/prometheus/prometheus.yml`

Final command:

`docker run -d -p 9090:9090 --name prometheus -v $PWD/prometheus/:/etc/prometheus/ prom/prometheus`

3/ Explore UI & metrics's format:

Metrics:

up: up is a gauge because we can see that it can be eather from 0 to 2.

prometheus_http_request_total: is a counter. It start from 0 and can only go up.

process_resident_memory_bytes: is a gauge. It can eather go up and go down depending of the value of the current memory bytes of the process resident.


