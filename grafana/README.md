### Initial setup
Create your `.env` file:
```console
michael@lenovo:~$ cp .env.template .env
```

Populate your `.env` file with the path where you want the container's data to be stored:
```dotenv
SHARED_DIRECTORY_PATH=/home/michael
```

Create a file which will hold your Grafana configuration:
```console
michael@lenovo:~$ mkdir -p /home/michael/.docker-grafana-local/grafana/config/
michael@lenovo:~$ cp grafana.ini.template /home/michael/.docker-grafana-local/grafana/config/grafana.ini
```

In your Grafana configuration file `grafana.ini` set the admin password. This will prevent Grafana 
from forcing you to reset the password after you log in (if you do not use the word "admin" for your 
password):
```ini
[security]
admin_password="password"
```

Create a file which will hold your Promtail configuration:
```console
michael@lenovo:~$ mkdir -p /home/michael/.docker-grafana-local/promtail/config/
michael@lenovo:~$ cp docker-config.yaml.template /home/michael/.docker-grafana-local/promtail/config/docker-config.yaml
```

Create a directory which is mounted to the container and will hold all your local log files:
```console
michael@lenovo:~$ mkdir -p /home/michael/.docker-grafana-local/promtail/log/my-app
```

Start the container:
```console
michael@lenovo:~$ sudo docker-compose up -d
```

Now access Grafana in your browser (see the following sections) and add a Loki data source:
- In the main menu on the left search for "Explore" and click it.
- On http://localhost:3000/explore click on "Add data source".
- On http://localhost:3000/datasources/new click on "Loki".
- For the new data source enter the URL `http://loki:3100/` and save the new data source.
- In the left main menu again go to "Explore" and now use Loki.

### Access in browser
Go to http://localhost:3000/login and login with:
- Username `admin` 
- Password `password` (the password is defined in your local `grafana.ini`)

### Add a local app's log files
Create a symbolic link for the app's log directory:
```console
// Delete the app's old log files
michael@lenovo:~$ rm -rf /var/www/html/my-app/my-app/var/log
// Create links to your new log files
michael@lenovo:~$ ln -s /home/michael/.docker-grafana-local/promtail/log/my-app /var/www/html/my-app/my-app/var/log
```

Then add it to your `docker-config.yaml` (which holds your Promtail config):
```yaml
scrape_configs:
  - job_name: my-app.dev
    static_configs:
      - targets:
          - localhost
        labels:
          app: my-app.dev
          __path__: /var/promtail/log/my-app/dev.log
  - job_name: my-app.prod
    static_configs:
      - targets:
          - localhost
        labels:
          app: my-app.prod
          __path__: /var/promtail/log/my-app/prod.log
  - job_name: my-app.test
    static_configs:
      - targets:
          - localhost
        labels:
          app: my-app.test
          __path__: /var/promtail/log/my-app/test.log
```

Now you can find [my-app.dev](http://localhost:3000/explore?orgId=1&left=%5B%22now-1h%22,%22now%22,%22Loki%22,%7B%22expr%22:%22%7Bapp%3D%5C%22my-app.dev%5C%22%7D%22%7D,%7B%22ui%22:%5Btrue,true,true,%22none%22%5D%7D%5D),
[my-app.test](http://localhost:3000/explore?orgId=1&left=%5B%22now-1h%22,%22now%22,%22Loki%22,%7B%22expr%22:%22%7Bapp%3D%5C%22my-app.test%5C%22%7D%22%7D,%7B%22ui%22:%5Btrue,true,true,%22none%22%5D%7D%5D)
and [my-app.prod](http://localhost:3000/explore?orgId=1&left=%5B%22now-1h%22,%22now%22,%22Loki%22,%7B%22expr%22:%22%7Bapp%3D%5C%22my-app.prod%5C%22%7D%22%7D,%7B%22ui%22:%5Btrue,true,true,%22none%22%5D%7D%5D).
