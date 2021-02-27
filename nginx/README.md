### Start
```console
michael@lenovo:~$ sudo docker-compose up -d
```
Now visit the demo app at http://127.0.0.1:3005/.

### Reload nginx (e.g. after config changes)
```console
michael@lenovo:~$ sudo docker exec nginx nginx -s reload

// Or login to your Docker instance via docker exec -it [YOUR CONTAINER NAME] /bin/sh ...
// michael@lenovo:~$ sudo docker exec -it nginx /bin/sh
// ... and reload nginx
// root@c0a33c5aed72:/# nginx -s reload
```
