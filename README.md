# Streamlit LLM 専門家Q&Aアプリケーション

このアプリケーションは、ユーザーが質問を入力し、異なる「専門家」の視点から回答を得ることができるStreamlitウェブアプリケーションです。

## 機能

- 歴史学と料理の2つの専門分野から選択可能
- 質問に対して専門家の視点からの回答を生成
- シンプルで使いやすいインターフェース

## インストール方法

1. リポジトリをクローン
```
git clone https://github.com/fjdayo/streamlit-llm-app.git
cd streamlit-llm-app
```

2. 依存関係をインストール
```
pip install -r requirements.txt
```

3. Streamlitのシークレット設定
   - `.streamlit/secrets.toml`ファイルを作成し、OpenAI APIキーを設定
   ```
   [openai]
   api_key = "your-api-key-here"
   ```

## 使用方法

1. アプリケーションを起動
```
streamlit run app.py
```

2. ブラウザで表示されるインターフェースで質問を入力
3. 専門家のタイプ（歴史学または料理）を選択
4. 「質問を送信」ボタンをクリック
5. 専門家の視点からの回答が表示されます

## 技術スタック

- Streamlit: ウェブインターフェース
- Langchain: LLM統合
- OpenAI: 言語モデル
