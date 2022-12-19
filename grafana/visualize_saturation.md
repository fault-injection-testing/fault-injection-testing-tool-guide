# リソース監視

このドキュメントでは[Grafanaの公式ドキュメント](https://grafana.com/oss/prometheus/exporters/node-exporter/)にしたがってリソース監視をおこなう方法を紹介します。

1. Grafana公式ドキュメントから[Recording rulesのYAMLファイル](https://grafana.com/oss/prometheus/exporters/node-exporter/?tab=recording-rules)を取得します。
2. [環境構築手順](../build_environment.md)の`compose.yaml`で設定したPrometheusの設定ファイルディレクトリ(`./prometheus/conf`)配下にrecording rulesのYAMLを配置します。
3. 以下のようにPrometheus設定ファイル[prometheus.yml](../prometheus/prometheus.yml)にrecording rulesのファイルパスを設定し、`docker compose`コマンドなどでPrometheusを再起動させます。

    ```yaml
    rule_files:
      - "/etc/prometheus/node_exporter_recording_rules.yml"
    ```

4. [こちら](https://grafana.com/oss/prometheus/exporters/node-exporter/?tab=dashboards)から用途に適したダッシュボードを[Grafanaにインポート](https://grafana.com/docs/grafana/latest/dashboards/manage-dashboards/#import-a-dashboard)してください。
