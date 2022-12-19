# 障害テスト実行手順

このドキュメントでは障害シミュレーションツールを利用した障害テストの実行手順を例示します。
[障害テストの環境構築](./build_environment.md)と[障害シミュレーションツールの導入](https://github.com/fault-injection-testing/fault-injection-tool/blob/main/README.md)がおこなわれていることが前提です。

Chaos ToolkitのシナリオはJSONまたはYAML形式で記述が可能で、このドキュメントでは`experiment.json`ファイルにシナリオを記述することを前提とします。

Chaos Toolkitのシナリオの記述例をJSONの[サンプルシナリオ](experiment.json)で示しています。
サンプルシナリオは環境変数で指定したサーバに対して外向きの通信に遅延を発生させJMeterシナリオのリクエスト結果がどう変化するかを検証するものになっています。

## 事前準備

サンプルではChaos Toolkitの実行ログをGrafana Lokiに送信する設定を行っています。同様の機能を利用する場合は、chaostoolkit-grafanaの[利用方法](https://github.com/chaostoolkit-incubator/chaostoolkit-grafana#sending-chaos-toolkit-logs-to-loki)を参考にして`experiment.json`の[`secrets`](https://chaostoolkit.org/reference/api/experiment/#secrets)ブロックと[`controls`](https://chaostoolkit.org/reference/api/experiment/#controls)ブロックにGrafana Lokiの接続情報を記述します。

また、サンプルではChaos Toolkitの[Json Path機能](https://github.com/chaostoolkit/chaostoolkit-lib/tree/782f927e338077a055a628fffe07e4b7d83baa6e#json-path)を利用するため、以下のコマンドを実行して必要なライブラリをインストールする必要があります。

```bash
pip install -U chaostoolkit-lib[jsonpath]
```

## シナリオ作成

### 定常状態の定義

障害をシミュレーションした際に対象のシステムが正常に動作していることを確認するための指標（定常状態）を定義して、定常状態が継続しているかを確認するための記述を`experiment.json`の[`steady-state-hypothesis`](https://chaostoolkit.org/reference/api/experiment/#steady-state-hypothesis)ブロックにします。

サンプルでは定常状態が破られたことを検知できるようGrafanaで[アラート](https://grafana.com/docs/grafana/latest/alerting/)設定をおこない、[Grafana API](https://grafana.com/docs/grafana/latest/developers/http_api/)でアラート発生有無を確認します。アラートが発生した場合は定常状態の仮説が破られ、アラートが発生しなかった場合は定常状態の仮説が正しいと判断できます。

### テスト内容の記述

`experiment.json`の[`method`](https://chaostoolkit.org/reference/api/experiment/#method)ブロックでテスト中におこなう任意の操作を記述します。

サンプルでは任意のJMeterシナリオを実行し、その最中に[障害シミュレーションツール](https://github.com/fault-injection-testing/fault-injection-tool/blob/main/README.md)を利用してネットワーク遅延をシミュレーションをおこなう記述がされています。

### ロールバック

必要があれば、`experiment.json`の[`rollbacks`](https://chaostoolkit.org/reference/api/experiment/#rollbacks)ブロックにテストシナリオで変更を加えた内容を初期状態に戻すための記述をします。

サンプルでは障害シミュレーションツールでネットワーク遅延をシミュレーションするために加えた変更をロールバックするための記述をしています。

## Chaos Toolkitの実行

Chaos Toolkitの`chaos run`コマンドで記述したシナリオの実行を行えます。`chaos run`コマンドの詳細については[Chaos Toolkitのドキュメント](https://chaostoolkit.org/reference/usage/run/)を参照してください。

サンプルでは障害シミュレーション対象のサーバ、そのサーバにSSH接続するための情報とJMeterシナリオのパスを以下の環境変数で設定します。

- `ATTACK_TARGET`: 障害をシミュレーションするサーバを設定
- `SSH_USER`: SSH接続するユーザを設定
- `SSH_KEY_PATH`: SSH接続で利用する鍵のパスを設定
- `JMETER_SCRIPT_PATH`: JMeterシナリオのパス

サンプルシナリオを実行するためのコマンドは以下のようになります。

```bash
ATTACK_TARGET=address SSH_USER=user SSH_KEY_PATH=key_path JMETER_SCRIPT_PATH=scenario.jmx chaos run experiment.json
```

## Grafanaでメトリクス確認

実行中や実行完了後にGrafanaでメトリクス状況を確認します。

メトリクスの可視化方法に関しては以下を参照してください:

- [アプリケーション監視](./grafana/visualize_red_method.md)
- [リソース監視](./grafana/visualize_saturation.md)
