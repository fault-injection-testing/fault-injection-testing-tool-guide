# GrafanaとGrafana Lokiの連携

1. GrafanaのUIで"Configuration"->"Data Sources"に移動し、"Add data source"を選択
2. "Loki"を検索し選択
3. "URL"に`http://<ホスト名>:3100`を入力し"Save & test"を押下
   - Grafana Lokiにデータが入ってない場合、押下時に"Data source connected, but no labels received. Verify that Loki and Promtail is configured properly."という警告が表示されますが、連携は成功しているため問題ありません。
