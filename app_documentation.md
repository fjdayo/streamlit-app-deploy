# app.py ドキュメント

このドキュメントは、Streamlit LLM 専門家Q&Aアプリケーションの中核となる`app.py`ファイルの機能と構造を説明します。

## 概要

`app.py`は、OpenAIのLLM（大規模言語モデル）を使用して、ユーザーの質問に対して専門家の視点から回答を生成するStreamlitウェブアプリケーションのメインファイルです。

## 主要コンポーネント

### 1. インポートと初期設定

```python
import streamlit as st
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# Streamlit Secrets から API キーを取得
openai_api_key = st.secrets["openai"]["api_key"]
```

このセクションでは、必要なライブラリをインポートし、Streamlitのシークレット機能からOpenAI APIキーを取得しています。

### 2. 専門家回答生成関数 (`get_expert_answer`)

```python
def get_expert_answer(user_input, expert_type):
    # LLMの初期化
    llm = OpenAI(temperature=0.7, openai_api_key=openai_api_key, max_tokens=500)

    # 専門家に応じたプロンプトのシステムメッセージ
    if expert_type == 'A：歴史学':
        expert_message = "あなたは歴史学の専門家です。次の質問に答えてください:"
    elif expert_type == 'B：料理':
        expert_message = "あなたは料理の専門家です。次の質問に答えてください:"
    else:
        expert_message = "あなたは一般的なアシスタントです。次の質問に答えてください:"

    # プロンプトテンプレートを定義
    prompt = PromptTemplate(input_variables=["user_input"], template=f"{expert_message} {{user_input}}")

    # プロンプトを使ってLLMチェーンを作成
    chain = LLMChain(llm=llm, prompt=prompt)

    # LLMを使って回答を取得
    result = chain.run(user_input)
    
    return result
```

この関数は以下の処理を行います：
- OpenAI LLMを初期化（temperature=0.7, max_tokens=500）
- 選択された専門家タイプに基づいてプロンプトメッセージを設定
- LangchainのPromptTemplateとLLMChainを使用して回答を生成
- 生成された回答を返す

### 3. メイン関数 (`main`)

```python
def main():
    # アプリケーションタイトル
    st.title("専門家への質問")

    # アプリケーションの説明
    st.write("このWebアプリケーションでは、専門家に質問をすることができます。以下の入力フォームに質問を入力し、専門家を選んで送信してください。")

    # ユーザーからの入力を取得
    user_input = st.text_input("質問内容を入力してください:")

    # 専門家の選択肢を提供
    expert_type = st.radio(
        "専門家を選んでください",
        ('A：歴史学', 'B：料理'),
        index=0
    )

    # 質問が送信されたときの処理
    if st.button('質問を送信'):
        if user_input:
            # 入力された内容と選択された専門家に基づいてLLMから回答を取得
            answer = get_expert_answer(user_input, expert_type)
            st.write("### 回答:")
            st.write(answer)
        else:
            st.error("質問を入力してください。")
```

この関数は以下の処理を行います：
- Streamlitを使用してウェブインターフェースを構築
- ユーザーが質問を入力するテキスト入力フィールドを表示
- 専門家タイプを選択するラジオボタンを表示
- 「質問を送信」ボタンが押されたときの処理を定義
- 入力が空でない場合、`get_expert_answer`関数を呼び出して回答を取得し表示
- 入力が空の場合、エラーメッセージを表示

### 4. エントリーポイント

```python
if __name__ == "__main__":
    main()
```

このセクションは、スクリプトが直接実行された場合に`main`関数を呼び出します。

## パラメータの説明

- **temperature (0.7)**: LLMの出力のランダム性を制御するパラメータ。高い値ほど創造的な回答になります。
- **max_tokens (500)**: 生成される回答の最大トークン数（長さ）を制限します。
- **expert_type**: 「A：歴史学」または「B：料理」の専門家タイプを指定します。

## アプリケーションフロー

1. ユーザーがアプリケーションにアクセス
2. 質問を入力フィールドに入力
3. 専門家タイプ（歴史学または料理）をラジオボタンで選択
4. 「質問を送信」ボタンをクリック
5. アプリケーションが入力を検証
   - 入力が空の場合、エラーメッセージを表示
   - 入力がある場合、選択された専門家タイプに基づいてLLMから回答を取得
6. 生成された回答をウェブページに表示

## 技術的な詳細

- **Streamlit**: ウェブインターフェースの構築に使用
- **Langchain**: LLMとの統合を簡素化するフレームワーク
- **OpenAI API**: テキスト生成のためのLLMを提供
- **PromptTemplate**: ユーザー入力と専門家コンテキストを組み合わせたプロンプトを作成
- **LLMChain**: プロンプトとLLMを組み合わせてテキスト生成を実行

## セキュリティ上の考慮事項

- OpenAI APIキーはStreamlitのシークレット機能を使用して安全に管理
- APIキーはコードに直接記述されていない
- `.streamlit/secrets.toml`ファイルを使用してAPIキーを設定する必要がある
