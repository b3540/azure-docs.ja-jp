---
title: "Azure AD v2 の ASP.NET Web サーバーの概要 - 構成 | Microsoft Docs"
description: "OpenID Connect 標準を使用する従来の Web ブラウザー ベースのアプリケーションで、ASP.NET ソリューションに Microsoft のサインインを実装する"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: 6c6a44cbbd79cf6422515a6d70d48ac12cba4f9f
ms.contentlocale: ja-jp
ms.lasthandoff: 07/06/2017


---
## アプリケーションの登録情報を使用した ASP.NET Web アプリの構成
<a id="configure-your-aspnet-web-app-with-the-applications-registration-information" class="xliff"></a>

この手順では、SSL を使用するようにプロジェクトを構成した後、SSL URL を使用してアプリケーションの登録情報を構成します。 その後、*web.config* を介して、アプリケーションの登録情報をソリューションに追加します。

1.  ソリューション エクスプローラーで、プロジェクト選択して [`Properties`] \(プロパティ) ウィンドウを確認します ([プロパティ] ウィンドウが表示されない場合は F4 キーを押します)。
2.  以下のように [`SSL Enabled`] \(SSL 有効) を `True` に変えます。<br/>
![プロジェクトのプロパティ](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />
3.  上の画像にある [`SSL URL`] の値をコピーしてこのページの最上部にある [`Redirect URL`] \(リダイレクト URL) フィールドに貼り付け、*[更新]* をクリックします。
4.  `configuration\appSettings` セクションのルート フォルダーにあるファイル `web.config` に、以下のコードを追加します。

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```

