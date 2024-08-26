# はじめに

* このツールの設計書は以下に保管しています。  
    [TOMONI Portal ＞ 開発者サイト ＞ Application Documents ＞ 0111_MHI-MS向けソフトウェア設計業務支援システム ＞ 98_利用手順](https://dxncloud.sharepoint.com/:f:/r/sites/CMSPISPS001/forDev/Application%20Document/0111_MHI-MS%E5%90%91%E3%81%91%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E8%A8%AD%E8%A8%88%E6%A5%AD%E5%8B%99%E6%94%AF%E6%8F%B4%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0/98_%E5%88%A9%E7%94%A8%E6%89%8B%E9%A0%86?csf=1&web=1&e=9paLFK)

# 0. 事前準備  
    
以下Azureリソースを作成してください。
* Azure AI Search
* Azure OpenAI  
    Azure OpenAIでは、リソース作成に加えてGPTモデルおよび埋め込みモデルのデプロイが必要です。  
    GPTモデルは任意のモデル、埋め込みモデルは"text-embedding-ada-002"をデプロイしてください。

検証したいシステム仕様書をAzureリソースにアップロードしてください。

# 1. 起動方法

1.  AVD2番環境から以下にアクセスし，CPFScenarioGenEXE.zipを自分のフォルダにコピーする。  
    \\\cpffileserver04\Project\CPFScenarioGenEXE 
    コピー先の例）\\\cpffileserver04\Public\{Your Name}
1. Zipファイルを解凍する。
1. .streamlit/secret.toml　ファイルに、「事前準備」にて作成した各リソースのAPIおよびエンドポイントを記載する 
　　![secret.toml内容](img/02.png)
    ※"model_name"、"model_deploy_name"は、以下例のようにコンマ区切りで記載してください。  
        model_name = ["GPT3.5","GPT4.0"]  
        model_deploy_name = ["xxxx_gpt-35-turbo","xxxx_gpt-4"]
1. CPFScenarioGenEXE.exe を起動する。  
下図の画面が出た場合，キャンセルをクリックする。  
![警告画面](img/01.png)!

# 2. 操作方法

## ① 要求事項抽出
1. システム仕様書選択 
    [ファイルリスト取得]ボタンをクリックし、対象ファイルを1つ選択してください。  
    **【注意】システム仕様書がTOMONI AI Studioにアップロードされていることが前提となる。**
1. 要求事項検索
    要求事項を検索する機能名（検索キーワード）を入力し、[実行]ボタン押下してください。
1. 要求事項抽出
    以下の手順で要求事項を抽出してください。
    * プロンプトの設定
        以下入力形式から1つ選択して、プロンプトを設定してください。
        * 画面上から直接入力
        * JSONファイルをアップロードする
    * GPTモデルの選択
    * 要求事項抽出結果のファイル出力有無のチェック  
    　　ファイル出力する場合は、出力先パスを指定してください。
1. [実行]ボタンの押下
    実行ボタンを押して、要求事項抽出処理が完了するまでお待ちください。

## ② テストケース作成
1. プロンプトの設定
    以下入力形式から1つ選択して、プロンプトを設定してください。
    * 画面上から直接入力
    * JSONファイルをアップロードする
1. GPTモデルの選択
1. テストケース生成結果のファイル出力有無のチェック
    ファイル出力する場合は、出力先パスを指定してください。
1. [実行]ボタンの押下
    実行ボタンを押して、テストケース作成処理が完了するまでお待ちください。

### （参考）プロント設定ファイルのフォーマット

* ファイル形式：JSON
* エンコーディング：Shift JIS

![ファイル例](img/03.png)

### （参考）要求事項抽出結果ファイルのフォーマット

* ファイル形式：TXT
* エンコーディング：UTF-8(BOM付き)
* ファイル名：output_yyyyMMdd_HHmmss_Requirements.txt　

![ファイル例](img/04.png)

### （参考）テストケース作成結果ファイルのフォーマット

* ファイル形式：Excel
* ファイル名: ファイル名：output_yyyyMMdd_HHmmss_TestCase.xlsx

![ファイル例](img/05.png)

# Q&A
## ①不要になったIndexを削除するにはどうすればいいですか？
### Azure Portal上で削除してください。
1. Azure Portalから対象のAI Searchにアクセスする。
1. 左のメニューから"インデックス"を押下する。
1. 削除するインデックスを押下し、上部メニューから"削除"を選択する。  

![Indexの削除](img/04.png)


## ②AI Searchへ作成できるIndex数の上限を超えた場合はどうすればいいですか？
### 「①不要になったIndexを削除するにはどうすればいいですか？」の手順に沿って削除してください。
なお、上限についてはMicrosoftの公式サイトをご確認ください。  
[> Azure AI Search のサービス制限](https://learn.microsoft.com/ja-jp/azure/search/search-limits-quotas-capacity)

## ③インデクシングボタンを押下した際このようなエラーが出ました。　　
## ![Indexer error](img/05.png)  
### 「インデクシング完了」と表示されている場合、無視してください。
「インデクシング完了」と表示されると問題なくIndexへ登録されています。  
「インデクシング失敗」となった場合は、再度インデクシングボタンを押下してください。　　

## ④AI Searchへの検索実行回数を確認したいです。
### Azure Portalのメトリックで確認できます。
1. Azure Portalから対象のAI Searchにアクセスする。
1. 左のメニューから"メトリック"を押下する。
1. 以下設定して検索数を確認する。
    * スコープ：利用中のAI Searchリソース名
    * メトリック名前空間：Search Service 標準的なメトリック
    * メトリック：Search queries per second
    * 集計：カウント  
![AIsearchメトリック](img/06.png) 




