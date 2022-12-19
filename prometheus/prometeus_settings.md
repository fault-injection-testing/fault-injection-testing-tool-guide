# Prometheusの設定ファイル

[Prometheusの設定ドキュメント](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)を参照して設定ファイル[prometheus.yml](./prometheus.yml)を適切に編集し、`compose.yaml`の`volumes`で設定した`/etc/prometheus`と対応するディレクトリに配置してください。

データ取得先のNode Exporterを追加したい場合は、以下のように`scrape_configs`の`targets`にIPアドレスとポート番号を記入してください。

```yaml
scrape_configs:
  - job_name: "node"
    static_configs:
      - targets:
        - X.X.X.X:9100
```
