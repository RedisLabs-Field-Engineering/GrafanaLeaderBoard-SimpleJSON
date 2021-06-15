# Grafana Leader Board

This creates a way to build a leaderboard into Grafana from a Redis Graph data source using Simple JSON

To run:

1) Startup a Grafana Docker container

```
docker run --rm  -p 3000:3000 --name=grafana -e "GF_INSTALL_PLUGINS=redis-datasource,grafana-simple-json-datasource" grafana/grafana

```

2) Startup this application

```
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
# Set any env vars - see below for more information
python3 app.py
```

3) Add a simple JSON datasource

The URL will look like

```
http://<MY_IP_ADDRESS>:5000/
```

![DataSource](docs/datasource.png?raw=true "Data Source")

4) Add a New Table panel

Be sure to set the Table in the top right and bottom left

![Panel](docs/panel.png?raw=true "Panel Setup")


## Environment variables

| Variable | Setting | Default |
|----------|----------|--------|
| REDIS_SERVER | Name or IP of Redis Instance|localhost|
| REDIS_PORT | Redis Instance port|6379|
| REDIS_PASSWORD | Redis Instance password|""|
|REDIS_LEADERBOARDS| Comma separated list (optional)| "" |
|REDIS_SCANPREFIX | Prefix of keys to scan for possible leaderboards | "" |
|REDIS_TOPK | The number of entries in the leaderboard | 10 |

Note: if REDIS_LEADERBOARDS, is not set then the application will scan all redis keys with the SCANPREFIX looking for sorted sets.  This is an expensive operation if you have lots of keys, so it's recommended that you set the REDIS_LEADERBOARDS if possible
