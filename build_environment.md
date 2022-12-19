# 環境構築手順

## 前提条件

ツール実行環境を構築するサーバに以下のツールがインストールされていることを前提とします。

- [Docker](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

環境を構築する各ソフトウェアは以下のバージョンで動作確認をおこなっています。

- Grafana: 9.1.8
- Grafana Loki: 2.6.1
- InfluxDB: 2.4.0
- Prometheus: v2.39.1
- Python: 3.10.0
- Chaos Toolkit: 1.12.0
- JMeter: 5.5
  - Java 8+ が必要
- Node Exporter: 1.4.0

## 手順

### Grafana, Grafana Loki, InfluxDB, Prometheusの設定

[compose-sample.yaml](compose-sample.yaml)をコピーして`compose.yaml`を作成します。

```sh
cp compose-sample.yaml compose.yaml
```

以下を参考にして必要な設定ファイルの作成と`compose.yaml`の編集を行います。

- [Grafana Lokiの設定](./loki/loki_settings.md)
- [InfluxDBの設定](./influxdb/influxdb_settings.md)
- [Prometheusの設定](./prometheus/prometeus_settings.md)

ツール実行環境を構築するサーバ上に`compose.yaml`を配置し以下のコマンドを実行します。

```sh
docker compose up
```

起動後に`Grafana`のUIにWebブラウザでアクセスし（デフォルトだとURLは`http://ホスト名:3000`、ログインに必要なusernameとPasswordはどちらもadmin）、以下を参考にして`Grafana`に各データソースを連携します。

- [Grafana Lokiの設定](./loki/connect_grafana_to_loki.md)
- [InfluxDBの設定](./influxdb/connect_grafana_to_influxdb.md)
- [Prometheusの設定](./prometheus/connect_grafana_to_prometheus.md)

### Node Exporterの設定

以下の手順にしたがって実験対象システムのリソース情報を取得したいサーバにNode Exporterを導入します。

- [MONITORING LINUX HOST METRICS WITH THE NODE EXPORTER](https://prometheus.io/docs/guides/node-exporter/)

### Chaos Toolkitの設定

ツール実行環境を構築するサーバに`Chaos Toolkit`をインストールします。

Chaos Toolkitのインストールは[How to Install the Chaos Toolkit](https://chaostoolkit.org/reference/usage/install/)を参照してください。Chaos Toolkitの実行ログをGrafana Lokiに送信する場合は、chaostoolkit-grafanaの[インストール手順](https://github.com/chaostoolkit-incubator/chaostoolkit-grafana#install)にしたがいchaostoolkit-grafanaのインストールも行ってください。

#### 障害シミュレーションツールの導入

[障害シミュレーションツールのGitHubリポジトリ](https://github.com/fault-injection-testing/fault-injection-tool/blob/main/README.md)を参照して、Chaos Toolkitをインストールしたサーバに障害シミュレーションツールの導入をおこなってください。

### JMeterの設定

ツール実行環境を構築するサーバに`JMeter`をインストールします。
`JMeter`をインストールする環境に`Java`がインストールされていない場合は、Java 8以上のバージョンのJavaのインストールも行ってください。

JMeterは <https://jmeter.apache.org/download_jmeter.cgi> からダウンロードして解凍を行ってください。
解凍後に以下のコマンドを参考にして`~/.bashrc`などでJMeterの実行ファイルにパスを通してください。

```bash
export JMETER_HOME=/home/centos/apache-jmeter-5.5
export PATH=$JMETER_HOME/bin:$PATH
```

`JMeter`の実行結果を`Grafana`で可視化するために、[JMeterの設定](./jmeter/jmeter_settings.md)を参考にしてJMeterシナリオファイルに[Backend Listener](https://jmeter.apache.org/usermanual/component_reference.html#Backend_Listener)の設定を行ってください。

### メトリクス収集・監視

本ツールではGrafanaをInfluxDBやPrometheusと連携して、HTTPリクエスト実行結果を用いたアプリケーションの監視や、Node Exporterから取得した情報を用いたリソースの監視がおこなえます。

それぞれ以下の手順にしたがって設定をおこなってください。

- [アプリケーション監視](./grafana/visualize_red_method.md)
- [リソース監視](./grafana/visualize_saturation.md)
