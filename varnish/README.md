### Start
Let's suppose you want to be able to access Varnish at http://localhost:8099/ and let Varnish serve an app which
is running on your machine at http://localhost:80/.

Get the IP of your machine in the `docker0` network. This IP can be used inside of the Varnish container to
connect to your local host.
```console
user@machine:~$ ip -4 addr show docker0 | grep -Po 'inet \K[\d.]+'
172.17.0.1
```

Copy the IP (in this case `172.17.0.1`) and use it in your `/my-path/default.vcl` like this:
```vcl
vcl 4.0;

backend default {
  .host = "172.17.0.1";
  .port = "80";
}
```

Configure the path to your Varnish configuration in `.env` (use the provided template `cp .env.template .env`):
```dotenv
LOCAL_CONFIG_FILE=/my-path/default.vcl
```

Start the container:
```console
user@machine:~$ sudo docker-compose up -d
```

### Access
Open in a browser http://localhost:8099/

### Use Varnish's tools
```console
user@machine:~$ sudo docker exec -it varnish bash
// varnishlog is used to access request-specific data. It provides information about specific clients and requests.
root@7e831a73c62e:/etc/varnish# varnishlog
// varnishncsa displays Varnish access logs in NCSA Common log format.
root@7e831a73c62e:/etc/varnish# varnishncsa
// varnishtest allows you to display log records and counters from your tests.
root@7e831a73c62e:/etc/varnish# varnishtest
// varnishstat is used to access global counters.
root@7e831a73c62e:/etc/varnish# varnishstat
// varnishtop reads the Varnish log and presents a continuously updated list of the most commonly occurring log entries.
root@7e831a73c62e:/etc/varnish# varnishtop
// varnishhist reads the Varnish log and presents a continuously updated histogram showing the distribution of the last N requests by their processing.
root@7e831a73c62e:/etc/varnish# varnishhist
```
