---
title: 「Ollama+没入型翻訳」で高品質（？）無料翻訳サービスを手に入れる
date: 2024-07-05 17:30:00 +0900
categories: [AI]
tags: [AI,効率化,ツール,Selfhosted]
pin: false
media_subpath: /../assets/media/ollama-translate
mermaid: true
---

> Ollamaを使用する際には、高性能GPUの使用は必須ではありませんが、翻訳速度が遅いとユーザーエクスペリエンスに大きく影響します。少なくとも12GBのビデオメモリを搭載したコンピューターを使用することを強く推奨します。
{: .prompt-tip }

## TL;DR

- 「[没入型翻訳](https://immersivetranslate.com/ja/download/)」をインストールする
- 「[Ollama](https://ollama.com/)」をインストールする
- `ollama pull gemma2:9b`でモデルダウンロード
  - 好きなModelどれでもいい、PCスペックと相談
- `OLLAMA_ORIGINS="*" ollama serve`を実行する
- 「`没入型翻訳`」>「`設定`」>「`翻訳サービス`」> 一番下にスクロール>「`OpenAIインターフェースと互換性のあるカスタムAI翻訳サービスを追加しますか？`」クリック
- アドレスに`http://127.0.0.1:11434/v1/chat/completions`を入れて、`APIKEY`に任意文字列、`モデル`に`gemma2:9b`
- Enjoy

## 実際の効果

![Ollama HP](traefik.png)  

TraefikのGithub README.md  

![Ollama Gemma2](ollama-gemma2.png)  

Ollama Gemma2の説明ページ  

{%
  include embed/video.html
  src='evil-neuro.mp4'
  autoplay=false
  loop=false
  muted=true
%}

Evil Neuro-sama(AI VTber)  
  
個人的な感想として、Google翻訳は60/100なら、Gemma2による翻訳は75/100点といったところですね。  
特にYoutubeの自動生成英語字幕の翻訳だといまいちのところが多いです。まぁ自動生成字幕の方の問題もありますけど。  
もちろん、普通のGoogle翻訳と同じく、書面的な言葉（ドキュメント、論文など）であればあるほど、精度が上がっていく傾向があります。  

---
> ここからの内容は、全部`Gemma2:9b`で中国語から翻訳したものになります。  
> ※少しだけ手直しの部分あります。
{: .prompt-tip }

---

## Immersive Translate

[Immersive Translate](https://immersivetranslate.com/ja/download/)  
  
中国企業が開発したブラウザ拡張機能です。Edge、Chrome、Firefox、Safariなど主流のブラウザに対応しており、Tampermonkeyでもインストールできます。  
  
デフォルトでは、Google翻訳やMicrosoft翻訳を選択できます。また、設定でさまざまなAIサービスのAPIを追加して、AI翻訳を使用することもできます。内部には、GitHubやRedditなどの特定のケースを強化したAI用のプロンプトが多数含まれています。
  
私自身、この拡張機能を最も活用しているのは、GitHubや開発ドキュメント、APIリファレンスなどの専門的な用語が頻繁に登場する場面です。この拡張機能は、そのような場面で非常に正確な翻訳を提供してくれるため、私にとって最も信頼できる効率化ツールの一つとなっています。  
  
課題として、翻訳対象となる文章量が膨大であることが挙げられます。OpenAIなどのAIサービスプロバイダーのAPIを利用した場合、コスト面で負担が大きくなってしまいます。
そこで、私自身の使用状況では、ローカルにデプロイ可能なAIを使用することを優先しています。幸いなことに、AIに関する広告が溢れる現代において、簡単にセットアップできるローカルデプロイ型のAIサービスを見つけることは容易です。  
  
ここに登場するサービスをご紹介します：Ollama。  

## Ollama

[Ollama](https://ollama.com/)  
  
完全にローカルでインストールしたらあとは使うだけのAIのデプロイコマンドラインツールです。  
インストール後、`ollama pull gemma2:9b`を使用するだけでGemma2 9Bをダウンロードできます。  
その後、`ollama run gemma2:9b`を使用してチャットモードを起動できます。  
また、APIサーバーとしてデプロイすることもでき、`ollama serve`を実行するだけでローカルのポート`11434`でAPIサーバーが自動的に起動します。  
このAPIインターフェースは、OpenAIのAPIインターフェースと完全に互換性があり、これは非常に重要です。  
これは、OpenAI APIを使用できるほとんどのサービスが、ローカルのOllamaにアクセスするように変更できることを意味します。

### Ollamaの利用上の注意点

- [Ollamaは、デフォルトで127.0.0.1と0.0.0.0からのcross-originリクエストを許可しています。追加のoriginは`OLLAMA_ORIGINS`で構成できます。](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-can-i-allow-additional-web-origins-to-access-ollama)
  - 環境変数`OLLAMA_ORIGINS=*`を設定することで、この問題を解決できます。（ネットワークセキュリティにご注意ください）
- GPUの限界を超えるモデルを使用しないでください。参考として、4080(16GB)を使用して9Bのパラメータを持つGemma2を実行すると、約9GBのVRAMが消費されます。
- OllamaのAPIサーバーが起動していても、Ollamaはすぐにモデルを読み込みません。APIリクエストが来ると、対応するモデルが起動され、しばらくすると（約数分）自動的に解放されます。
  - つまり、複数のモデルのパフォーマンスをテストしたい場合は、コマンドラインでテストすることをお勧めします。API（たとえば、没入型翻訳のモデル設定を変更する）を使用しないようにしてください。これにより、複数のモデルが同時に読み込まれ、VRAM不足が発生する可能性があります。

## その他

- 会社内でOllamaを使用する場合、Ollamaを実行するための専用高性能PCを用意し、APIを社内ネットワークに公開するといった選択肢があります。これにより、低性能PCでのOllama利用の問題が解決されます。
- Ollamaは、上記で述べたようにOpenAIのAPIと完全に互換性があるため、多くの他のサービスと連携して使用することができます。そのため、拡張性の高いシステム構築が可能です。
