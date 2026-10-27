1/ Sélecteurs et labels

In query we can do research with regex.

For exemple: 
`up{job=~"prom.*"}` is going to look for every query that has "prom" in it.
`up{job!~"prom.*"}` look for a every query that doesn't have "prom" in it.
`up{job="prometheus"}` look for every query that is the same a "prometheus".
`up{job!="prometheus"}` look for a every query that isn't "prometheus".

node_cpu_seconds_total{mode="idle"}

In the result we can see cpu=0, that represent which cpu is analyzed.
And the mode=idle.

Difference beetween instant vector (up) vs range vector (up[5m]) is that instant vector are the states of the query and with graph we can see from 1 hours or more or less.
Range vector are like a screenshot of the % last minutes in the exemple.

prometheus_http_requests_total: is hard to read and understand because there is to many series.

rate(v range-vector) calculates the per-second average rate of increase of the time series in the range vector.

Difference entre irate et rate: 

rate() donne une indication sur un counter.
Par exemple si on veut avoir le nombre de requettes du server dans un temps donné on va utliser la metric du nombre total de requette sur le serveur et lui appliquer la fonction rate.
Cette fonction permet d'avoir une visualisation lissé mais moins pour les pics et la reactivité.

irate() quand a lui v faire ses calcules sur les dernière data recupéré, donc va donner une meilleur indication sur les pics(va etre plus réactif)

Agregation:

Difference entre "sum by" et "sum without" est qu'avec by on va choisir toutes les métrics qui passe par ce qu'on a précisé entre parenthese.

Par exemple:
`sum by (handler) (rate(prometheus_http_requests_total[5m]))`
On récupere le nombre total de requettes du serveur via les routes(handler)
`sum by (code) (rate(prometheus_http_requests_total[5m]))`
On récupere le nombre total de requettes du serveur via les codes.

`sum by (handler) (rate(prometheus_http_requests_total[5m]))`
On récupere le nombre total de requettes du serveur via ce qui ne passe pas par les routes.

Opérateurs et métriques dérivées avec node_exporter (60 min)

Écris les requêtes suivantes :

1 - % de mémoire utilisée : à partir de node_memory_MemAvailable_bytes et node_memory_MemTotal_bytes:

On fait le calcul : `(node_memory_MemAvailable_bytes * 100) / node_memory_MemTotal_bytes`
Apres calcul on utilise environ = 29% de la memoire total.

2 - % d’utilisation CPU : 100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100). Comprends chaque morceau, puis explique pourquoi on utilise mode="idle" et pas les autres modes.

On calcule d'abord le rate de l'idle (état du processeur lorsqu'il n'est pas utilisé) et on fait une moyen des 4 route restante filtré. Ensuite on multiplie par 100 pour avoir un pourcentage qu'on soustrait a 100.

3 - % d’espace disque utilisé sur / avec node_filesystem_avail_bytes et node_filesystem_size_bytes (filtre sur mountpoint="/" ; si rien ne sort, regarde quels mountpoints existent).


4 - Débit réseau entrant en octets/s : node_network_receive_bytes_total avec rate(), sans l’interface lo.


5 - Génère de la charge (par exemple yes > /dev/null quelques secondes dans un terminal, puis Ctrl+C) et observe le CPU monter sur ton graphe.
