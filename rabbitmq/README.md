### Start
```console
// Either ...
michael@lenovo:~$ echo "SHARED_DIRECTORY_PATH=$HOME" > .env
// ... or copy the template and edit your new .env manually
michael@lenovo:~$ cp .env.template .env
// Then you can start the containers
michael@lenovo:~$ sudo docker-compose up -d
```

### Access the web UI of the management plugin
Open in a browser http://localhost:15672/ and use these credentials:
- User: `user`
- Password: `password`
