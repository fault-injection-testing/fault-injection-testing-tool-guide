# Grafana Loki の設定ファイル

デフォルト以外の設定を行いたい場合は[Configuring Grafana Loki](https://grafana.com/docs/loki/latest/configuration/)を参考に設定ファイルを作成し、`compose.yaml`に`volumes`の設定を追加する。

```yaml
 loki:
   ...
   volumes: <your file path>:/etc/loki/local-config.yaml
   ...
```
