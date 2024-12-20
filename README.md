# Discord Bot: チャットボット

このプロジェクトは、LLM（大規模言語モデル）サーバーと連携してリアルタイムで対話型の応答を提供するDiscordボットです。ボットは文字ごとにテキスト応答をストリーミングします。`discord.py`を使用して構築されており、Pythonの`asyncio`を活用しています。

## 機能

- 指定されたチャンネルでユーザーのメッセージをリッスン。
- プロンプトをLLMサーバーに送信し、文字単位で応答をストリーミング。
- 応答が進行中にDiscordメッセージを動的に更新。

## 動作デモ
![スクリーンショット 2024-12-20 172521](https://github.com/user-attachments/assets/adaedb07-3fae-4cf7-bf32-df1d5e545df4)

![スクリーンショット 2024-12-20 172449](https://github.com/user-attachments/assets/b8873c6a-c0b0-4f30-9ad7-cc909afa35a7)

https://github.com/user-attachments/assets/fd6861e7-5377-4d42-a01e-8405685e3db6


## 前提条件

- Python 3.8以上
- アクセス可能なAPIを持つ稼働中のLLMサーバー
- Discordボットトークン

## セットアップ

### 1. リポジトリをクローン
```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. 必要な依存関係をインストール
以下のコマンドでPythonパッケージをインストールしてください：
```bash
pip install -r requirements.txt
```

### 3. 環境変数を設定
`settings.py`ファイルを作成し、以下の変数を定義してください：

```python
# settings.py
TOKEN = 'your-discord-bot-token'
LLM_HOST = 'llm-server-host'
LLM_PORT = 'llm-server-port'
CHATBOT_CH_ID = 123456789012345678  # ボットが応答するチャンネルIDを指定
```

### 4. Ollamaを起動
以下のコマンドでOllamaを起動します(モデルはなんでも大丈夫ですが、settings.pyにも同じモデル名を記載してください。)：
```bash
Ollama run gemma:2b
```

### 5. ボットを起動
以下のコマンドでボットを起動します：
```bash
python3 chatbot.py
```

## 使用方法

### コマンド
- `!chat <prompt>`: メッセージをLLMサーバーに送信し、文字単位で応答をストリーミングします。このコマンドは、`CHATBOT_CH_ID`で指定されたチャンネルでのみ動作します。

### 例
1. 指定されたDiscordチャンネルに移動します。
2. コマンド `!chat Hello! How are you?` を入力します。
3. ボットが応答をストリーミングする様子を観察します。

## コード構成

### 主なコンポーネント

#### `bot.py`
ボットロジックを含むメインスクリプト。主な部分は以下の通りです：
- **イベントリスナー**:
  - `on_ready()`: ボットの準備が完了したときにメッセージをログに記録します。
- **コマンド**:
  - `chat`: ユーザープロンプトを処理し、LLMサーバーとやり取りします。

#### `LLM_Model`
LLMサーバーとの通信を担当するクラス。応答をストリーミングし、非同期キューに文字を配置してリアルタイム処理を行います。

#### `settings.py`
ボットトークン、LLMサーバー、チャンネルIDの設定を含むファイル。

## 注意事項

- LLMサーバーがストリーミング応答をサポートし、期待されるAPI形式に従っていることを確認してください。
- パフォーマンスのため、ボットはチャンク単位で応答を更新します。このチャンクサイズや更新頻度は`chat`関数で調整できます。
- このボットは`CHATBOT_CH_ID`で定義されたチャンネルでのみ応答します。

## 依存関係

- `discord.py`: Python用のDiscord APIラッパー。
- `requests`: LLMサーバーとの通信に使用するHTTPライブラリ。

## トラブルシューティング

1. **ボットが応答しない場合:**
   - ボットが正しく起動し、適切なDiscordサーバーにログインしていることを確認してください。
   - `settings.py`のチャンネルIDが対象のDiscordチャンネルと一致していることを確認してください。

2. **APIエラー:**
   - LLMサーバーが稼働しておりアクセス可能であることを確認してください。
   - `settings.py`内の`LLM_HOST`および`LLM_PORT`の値を確認してください。

3. **ストリーミングの問題:**
   - LLMサーバーが期待される形式でストリーミング応答をサポートしていることを確認してください。

## ライセンス

このプロジェクトはMITライセンスの下でライセンスされています。
