---
title: "Azure Log Analytics で変更を追跡する | Microsoft Docs"
description: "Log Analytics の変更の追跡ソリューションは、ユーザーの環境で起こるソフトウェアと Windows サービスの変更を特定するために役立ちます。"
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 7e0fa9a83c3c83145a4813422bf73a0e711d0ecc
ms.contentlocale: ja-jp
ms.lasthandoff: 07/06/2017


---
# 変更の追跡ソリューションを使用してユーザーの環境内のソフトウェアの変更を追跡する
<a id="track-software-changes-in-your-environment-with-the-change-tracking-solution" class="xliff"></a>

![変更の追跡シンボル](./media/log-analytics-change-tracking/change-tracking-symbol.png)

この記事では、Log Analytics の変更の追跡ソリューションを使用して、環境の変更箇所を簡単に識別する方法を説明します。 このソリューションは、Windows および Linux ソフトウェア、Windows ファイルおよびレジストリ、Windows サービス、および Linux デーモンに対する変更を追跡します。 構成の変更を識別することで、運用上の問題を特定できるようになります。

このソリューションをインストールすると、インストールしたエージェントの種類が更新されます。 監視対象サーバーにインストールされているソフトウェア、Windows サービス、および Linux デーモンの変更が読み取られた後、そのデータがクラウドの Log Analytics サービスに送信され、処理されます。 受信したデータにロジックが適用され、クラウド サービスによってそのデータが記録されます。 [変更の追跡] ダッシュボードの情報を使用して、サーバー インフラストラクチャで行われた変更を簡単に確認できます。

## ソリューションのインストールと構成
<a id="installing-and-configuring-the-solution" class="xliff"></a>
次の情報を使用して、ソリューションをインストールおよび構成します。

* 変更を監視する各コンピューターに、[Windows](log-analytics-windows-agents.md)、[Operations Manager](log-analytics-om-agents.md)、または [Linux](log-analytics-linux-agents.md) エージェントが必要です。
* 変更の追跡ソリューションを OMS ワークスペースに追加します。[Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview) から追加するか、[ソリューション ギャラリーからの Log Analytics ソリューションの追加](log-analytics-add-solutions.md)に説明されている手順に従って追加してください。  さらに手動で構成する必要はありません。

### 追跡する Linux ファイルを構成する
<a id="configure-linux-files-to-track" class="xliff"></a>
次の手順を使用して、Linux コンピューター上で追跡するファイルを構成します。

1. OMS ポータルで、**[設定]** (歯車アイコン) をクリックします。
2. **[設定]** ページで **[データ]** をクリックし、**[Linux ファイルの追跡]** をクリックします。
3. [Linux ファイルの変更追跡] の下に、追跡するファイルのファイル名を含むパス全体を入力し、**[追加]** シンボルをクリックします。 例: "/etc/*.conf"
4. **[保存]** をクリックします。  
  
> [!NOTE]
> Linux ファイルの追跡には、ディレクトリの追跡、ディレクトリの再帰、およびワイルドカード追跡などの追加機能があります。

### 追跡する Windows ファイルの構成
<a id="configure-windows-files-to-track" class="xliff"></a>
次の手順を使用して、Windows コンピューター上で追跡するファイルを構成します。

1. OMS ポータルで、**[設定]** (歯車アイコン) をクリックします。
2. **[設定]** ページで **[データ]** をクリックし、**[Windows ファイルの追跡]** をクリックします。
3. [Windows ファイルの変更追跡] の下に、追跡するファイルのファイル名を含むパス全体を入力して、**[追加]** の記号をクリックします。 例えば、C:\Program Files (x86)\Internet Explorer\iexplore.exe または C:\Windows\System32\drivers\etc\hosts のように入力します。
4. [ **Save**] をクリックします。  
   ![Windows ファイルの変更追跡](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### 追跡する Windows レジストリ キーを構成する
<a id="configure-windows-registry-keys-to-track" class="xliff"></a>
次の手順を使用して、Windows コンピューターで追跡するレジストリ キーを構成します。

1. OMS ポータルで、**[設定]** (歯車アイコン) をクリックします。
2. **[設定]** ページで **[データ]** をクリックし、**[Windows レジストリの追跡]** をクリックします。
3. [Windows レジストリの変更追跡] の下に、追跡するキー全体を入力し、**[追加]** 記号をクリックします。
4. **[保存]** をクリックします。  
   ![Windows レジストリの変更追跡](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### Linux ファイル コレクションのプロパティの説明
<a id="explanation-of-linux-file-collection-properties" class="xliff"></a>
1. **種類**
   * **ファイル** (ファイルのメタデータを報告: サイズ、変更日、ハッシュなど)
   * **ディレクトリ** (ディレクトリのメタデータを報告: サイズ、変更日など)
2. **リンク** (他のファイルまたはディレクトリへの Linux のシンボリック リンク参照の処理)
   * **無視** (再帰中にシンボリック リンクを無視して、参照先のファイル/ディレクトリを含めないようにする)
   * **フォロー** (再帰中にシンボリック リンクをフォローして、参照先のファイル/ディレクトリも含めるようにする)
   * **管理** (シンボリック リンクをフォローし、返されたコンテンツの処理を変更する) 
   > [!NOTE]   
   > ファイルのコンテンツの取得は現時点ではサポートされていないため、[管理] リンク オプションは推奨されません。
3. **再帰** (フォルダー レベルで再帰し、パス ステートメントに適合するすべてのファイルを追跡する)
4. **Sudo** (sudo 特権を必要とするファイルまたはディレクトリへのアクセスを有効にする)

### 制限事項
<a id="limitations" class="xliff"></a>
現在、変更の追跡ソリューションでは以下に対応していません。

* Windows ファイル追跡用のフォルダー (ディレクトリ)
* Windows ファイル追跡用の再帰
* Windows ファイル追跡用のワイルドカード
* パス変数
* ネットワーク ファイル システム
* ファイル コンテンツ

その他の制限事項:

* **[最大ファイル サイズ]** 列と値は現在の実装では使用されません。
* 30 分間の収集サイクルで収集するファイル数が 2500 を超えると、ソリューションのパフォーマンスが低下する可能性があります。
* ネットワーク トラフィックが高いとき、変更レコードが表示されるまでに最長で 6 時間かかることがあります。
* コンピューターのシャットダウン中に構成を変更した場合、そのコンピューターからは前の構成に対応するファイル変更が送信される可能性があります。

## 変更の追跡データ収集の詳細
<a id="change-tracking-data-collection-details" class="xliff"></a>
変更の追跡では、有効になっているエージェントを使用して、ソフトウェア インベントリと Windows サービス メタデータを収集します。

次の表は、変更の追跡におけるデータの収集手段と、データ収集方法に関する各種情報をまとめたものです。

| プラットフォーム | 直接エージェント | SCOM エージェント | Linux エージェント | Azure Storage (Azure Storage) | SCOM の要否 | 管理グループによって送信される SCOM エージェントのデータ | 収集の頻度 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Windows および Linux |![あり](./media/log-analytics-change-tracking/oms-bullet-green.png) |![あり](./media/log-analytics-change-tracking/oms-bullet-green.png) |![あり](./media/log-analytics-change-tracking/oms-bullet-green.png) |![なし](./media/log-analytics-change-tracking/oms-bullet-red.png) |![いいえ](./media/log-analytics-change-tracking/oms-bullet-red.png) |![はい](./media/log-analytics-change-tracking/oms-bullet-green.png) | 5 ～ 50 分 (変更の種類に応じて) 詳細については、以下を参照してください。 |


次の表は、変更の種類ごとにデータ収集の頻度を示したものです。

| **変更の種類** | **frequency** | **変更が検出されたときに****エージェント****から相違点が送信されるか** |
| --- | --- | --- |
| Windows レジストリ | 50 分 | なし |
| Windows ファイル | 30 分 | はい。 24 時間以内に変更がない場合は、スナップショットが送信されます。 |
| Linux ファイル | 約 15 分 | はい。 24 時間以内に変更がない場合は、スナップショットが送信されます。 |
| Windows サービス | 30 分 | はい。変更が検出されたときに 30 分ごとに送信されます。 スナップショットは、変更の有無に関係なく 24 時間おきに送信されます。 つまり変更が生じていなくてもスナップショットが送信されます。 |
| Linux デーモン | 5 分 | はい。 24 時間以内に変更がない場合は、スナップショットが送信されます。 |
| Windows ソフトウェア | 30 分 | はい。変更が検出されたときに 30 分ごとに送信されます。 スナップショットは、変更の有無に関係なく 24 時間おきに送信されます。 つまり変更が生じていなくてもスナップショットが送信されます。 |
| Linux ソフトウェア | 5 分 | はい。 24 時間以内に変更がない場合は、スナップショットが送信されます。 |

### レジストリ キーの変更追跡
<a id="registry-key-change-tracking" class="xliff"></a>

Log Analytics は、変更の追跡ソリューションを使用して、Windows レジストリの監視と追跡を実行します。 レジストリ キーへの変更を監視する目的は、サード パーティのコードやマルウェアがアクティブ化できる拡張性のポイントを正確に特定することです。 次の一覧は、ソリューションが追跡する既定のレジストリ キーとそれぞれが追跡される理由を示しています。

- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - スタートアップ時に実行されるスクリプトを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - シャットアップ時に実行されるスクリプトを監視します。
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - 64 ビット コンピューターで実行される 32 ビット プログラム用の Windows アカウントでユーザーがサインインする前に読み込まれるキーを監視します。
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed Components
    - アプリケーションの設定の変更を監視します。
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Windows Explorer に直接フックされ、通常は Explorer.exe でインプロセスで実行される共通自動開始エントリを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Windows Explorer に直接フックされ、通常は Explorer.exe でインプロセスで実行される共通自動開始エントリを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Windows Explorer に直接フックされ、通常は Explorer.exe でインプロセスで実行される共通自動開始エントリを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - アイコン オーバーレイ ハンドラーの登録を監視します。
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - 64 ビット コンピューターで実行される 32 ビット プログラムのアイコン オーバーレイ ハンドラーの登録を監視します。
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - 現在のページのドキュメント オブジェクト モデル (DOM) にアクセスし、ナビゲーションを制御するために使用できる、Internet Explorer の新しいブラウザー ヘルパー オブジェクト プラグインを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser Helper Objects
    - 64 ビットコンピューターで実行される 32 ビット プログラムの現在のページのドキュメント オブジェクト モデル (DOM) にアクセスし、ナビゲーションを制御するために使用できる、Internet Explorer の新しいブラウザー ヘルパー オブジェクト プラグインを監視します。
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - カスタム ツール メニューやカスタム ツール バー ボタンなどの新しい Internet Explorer の拡張機能を監視します。
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - 64 ビットコンピューターで実行される 32 ビット プログラムのカスタム ツールのメニューや 64 ビット コンピューター上で実行する 32 ビット プログラムのカスタム ツール バー ボタンなどの新しい Internet Explorer の拡張機能を監視します。
- HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - wavemapper、wave1、wave2、msacm.imaadpcm、.msadpcm、.msgsm610、および vidc に関連付けられている 32 ビット ドライバーを監視します。 SYSTEM.INI ファイルの [drivers] セクションに似ています。
- HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - 64 ビットコンピューターで実行される 32 ビット プログラムの wavemapper、wave1、wave2、msacm.imaadpcm、.msadpcm、.msgsm610、および vidc に関連付けられている 32 ビット ドライバーを監視します。 SYSTEM.INI ファイルの [drivers] セクションに似ています。
- HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - 既知のまたは一般的に使用されるシステムの DLL の一覧を監視します。このシステムは、システム DLL のトロイの木馬バージョンを削除することで、弱いアプリケーション ディレクトリのアクセス許可が悪用されることを防止します。
- HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Windows オペレーティング システムの対話型ログオン サポート モデルである Winlogon からイベント通知を受信できるパッケージの一覧を監視します。


## 変更の追跡を使用する
<a id="use-change-tracking" class="xliff"></a>
ソリューションをインストールした後で、OMS の **[概要]** ページにある **[変更の追跡]** タイルを使用すると、監視対象サーバーの変更の概要を確認できます。

![[変更の追跡] タイルの画像](./media/log-analytics-change-tracking/change-tracking-tile.png)

インフラストラクチャに対する変更を確認し、変更の詳細を次のカテゴリ別に表示することができます。

* ソフトウェアや Windows サービスに対する構成の種類ごとの変更
* アプリケーションに対するソフトウェアの変更および個々のサーバーに対する更新
* 各アプリケーションのソフトウェア変更の合計数
* Linux パッケージ
* 個々のサーバーでの Windows サービスの変更
* Linux デーモンの変更

![[変更の追跡] ダッシュボードの画像](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![[変更の追跡] ダッシュボードの画像](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### 変更の種類別に変更を表示するには
<a id="to-view-changes-for-any-change-type" class="xliff"></a>
1. **[概要]** ページで、**[変更の追跡]** タイルをクリックします。
2. **[変更の追跡]** ダッシュボードにあるいずれかの変更の種類ブレードで概要情報を確認し、**ログの検索**ページで、詳細情報を表示する変更の 1 つをクリックします。
3. どのログの検索ページでも、時間、詳細結果、ログ検索履歴を表示することができます。 結果を絞り込むファセットを使用してフィルター処理することもできます。

## 次のステップ
<a id="next-steps" class="xliff"></a>
* [Log Analytics のログ検索機能](log-analytics-log-searches.md) を使用して、詳細な変更追跡データを確認してください。

