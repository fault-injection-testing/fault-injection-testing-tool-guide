# GrafanaとInfluxDBの連携

1. GrafanaのUIで"Configuration"->"Data Sources"に移動し、"Add data source"を選択
2. "InfluxDB"を検索し選択
3. "Query Language"を`Flux`に設定
4. "URL"に`http://<ホスト名>:8086`を入力
5. "Auth"の設定はすべてオフ
6. "InfluxDB Details"の"Organization", "Token", "Default Bucket"にそれぞれ`compose.yaml`で設定した`DOCKER_INFLUXDB_INIT_ORG`, `DOCKER_INFLUXDB_INIT_ADMIN_TOKEN`, `DOCKER_INFLUXDB_INIT_BUCKET`と同様の値を設定
7. "Save & test"を押下
