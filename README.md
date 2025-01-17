# Online Audio Kit
**Version 1.0.5**

シンプルな音声キット

**注意：インターネット環境が必要です。またAIを使用する場合はOpenAIのKeyの設定を必ず行って下さい。**

## 機能
+ Google APIを使用したシンプルな音声キット
+ 文字起こし、発話に対応
+ VOSKを用いた音声認識
+ 言語指定可能
+ OpenAIのLangChainを用いて高精度な文章解析

# 環境構築
Debian系ではPyAudioの使用に以下のライブラリが必要です。
```shell:ubuntu
sudo apt install portaudio19-dev
```
# インストール
```shell
pip install git+https://github.com/rionehome/online_audio_kit
```

## パッケージの更新
```shell
pip install -U git+https://github.com/rionehome/online_audio_kit
```

# 使用方法
```python
from online_audio_kit import AudioKit

audio = AudioKit() # Option : AudioKit(language= 'ja' | 'en', openai_api_key=str, vosk_model_name=str | None, vosk_model_path=str | None)

# Google SpeechRecognitionで音声認識
recognized_text = audio.stt()

# gTTSで文章発話 第1引数: 発話文書(str) 第2引数: モード= "default" | "gen" ,第3引数: 音声ファイルのパス (output.mp3)
audio.tts("Hello! What drink do you like?")

# audio.tts("Hello What drink do you like?", mode="gen", path="AskDrink.mp3")

# 音声ファイルの再生
audio.play("AskDrink.mp3")

# OpenAIで文章解析 第1引数: AIへの入力文章(str), 第2引数: AIへのシステムプロンプト(str)
llm_response = audio.llm("I like orange juice.", "You are an analytical AI.  Extract only your favorite drinks from the input text and output the names of the drinks as an array. Example, Human: I like orange juice but I don't like coffee. You: ['orange juice'], Human: My favorite drink is grape juice and apple juice. You: ['grape juice','apple juice']")

# VOSKを用いた音声認識 ジェネレータ関数のためループで実行 例として終了条件を10文字以上の認識としている
for text in audio.vosk()
  print(text)
  if (len(text) > 10):
    break
```

# セットアップ
+ .envファイルの入力
OpenAIから発行したAPIキーを入力
```sh:.env
OPENAI_API_KEY="xxxxxxxxxxxxxx" // 変更
```
+ .envを使用しない場合
インスタンスを生成するときに指定できます。
```python
audio = AudioKit(openai_api_key="YOUR API KEY")
```
