### Start
```console
michael@lenovo:~$ sudo docker-compose up -d
```

### Reload nginx (e.g. after config changes)
```console
michael@lenovo:~$ sudo docker exec nginx nginx -s reload

// Or login to your Docker instance via docker exec -it [YOUR CONTAINER NAME] bin/bash ...
michael@lenovo:~$ sudo docker exec -it nginx bin/bash
// ... and reload nginx
root@c0a33c5aed72:/# nginx -s reload
```
