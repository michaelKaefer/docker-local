### Start
```console
// Either ...
michael@lenovo:~$ echo "SHARED_DIRECTORY_PATH=$HOME" > .env
// ... or copy the template and edit your new .env manually
michael@lenovo:~$ cp .env.template .env
// Then you can start the containers
michael@lenovo:~$ sudo docker-compose up -d
```

### Access MinIO's embedded web based object browser
Open in a browser http://localhost:9001/

### Links
- https://github.com/minio/minio/blob/master/docs/orchestration/docker-compose/docker-compose.yaml
