---
title: "チュートリアル: Azure Active Directory と Zscaler Two の統合 | Microsoft Docs"
description: "Azure Active Directory で Zscaler Two を使用して、シングル サインオンや自動プロビジョニングなどを有効にする方法について説明します。"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 579fe0647b9eb9664492b549d416d90b9fb0fada
ms.openlocfilehash: ec1b058446dc54049240bea5ce8ebd64a5826dbf
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>チュートリアル: Azure Active Directory と Zscaler Two の統合
このチュートリアルでは、Azure と ZScaler Two の統合について説明します。  
このチュートリアルで説明するシナリオでは、次の項目があることを前提としています。

* 有効な Azure サブスクリプション
* ZScaler Two でのシングル サインオンが有効なサブスクリプション

このチュートリアルを完了すると、ZScaler Two に割り当てた Azure AD ユーザーは、ZScaler Two 企業サイト (サービス プロバイダーが開始したサインオン) で、または「 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)

このチュートリアルで説明するシナリオは、次の要素で構成されています。

1. ZScaler Two のアプリケーション統合の有効化
2. シングル サインオンの構成
3. プロキシ設定の構成
4. ユーザー プロビジョニングの構成
5. ユーザーの割り当て

![シングル サインオンの構成](./media/active-directory-saas-zscaler-two-tutorial/IC800199.png "Configure Single Sign-On")

## <a name="enabling-the-application-integration-for-zscaler-two"></a>ZScaler Two のアプリケーション統合の有効化
このセクションでは、ZScaler Two のアプリケーション統合を有効にする方法を説明します。

### <a name="to-enable-the-application-integration-for-zscaler-two-perform-the-following-steps"></a>ZScaler Two のアプリケーション統合を有効にするには、次の手順を実行します。
1. Azure クラシック ポータルの左側のナビゲーション ウィンドウで、 **[Active Directory]**をクリックします。
   
   ![Active Directory](./media/active-directory-saas-zscaler-two-tutorial/IC700993.png "Active Directory")
2. **[ディレクトリ]** の一覧から、ディレクトリ統合を有効にするディレクトリを選択します。
3. アプリケーション ビューを開くには、ディレクトリ ビューでトップ メニューの **[アプリケーション]** をクリックします。
   
   ![アプリケーション](./media/active-directory-saas-zscaler-two-tutorial/IC700994.png "Applications")
4. ページの下部にある **[追加]** をクリックします。
   
   ![アプリケーションの追加](./media/active-directory-saas-zscaler-two-tutorial/IC749321.png "Add application")
5. **[実行する内容]** ダイアログで、**[ギャラリーからアプリケーションを追加します]** をクリックします。
   
   ![ギャラリーからのアプリケーションの追加](./media/active-directory-saas-zscaler-two-tutorial/IC749322.png "Add an application from gallerry")
6. **検索ボックス**に、「**ZScaler Two**」と入力します。
   
   ![アプリケーション ギャラリー](./media/active-directory-saas-zscaler-two-tutorial/IC800200.png "Application Gallery")
7. 結果ウィンドウで **[ZScaler Two]** を選択し、**[完了]** をクリックしてアプリケーションを追加します。
   
   ![ZScaler Two](./media/active-directory-saas-zscaler-two-tutorial/IC800201.png "ZScaler Two")

## <a name="configuring-single-sign-on"></a>シングル サインオンの構成
このセクションでは、ユーザーが SAML プロトコルに基づくフェデレーションを使用して、Azure AD でのユーザーのアカウントで ZScaler Two に対する認証を行うことができるようにする方法を説明します。  
この手順の途中で、base-64 でエンコードされた証明書を ZScaler Two テナントにアップロードする必要があります。  
この手順に慣れていない場合は、「 [How to convert a binary certificate into a text file (バイナリ証明書をテキスト ファイルに変換する方法)](http://youtu.be/PlgrzUZ-Y1o)

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>シングル サインオンを構成するには、次の手順を実行します。
1. Azure クラシック ポータルの **ZScaler Two** アプリケーション統合ページで、**[シングル サインオンの構成]** をクリックして、**[シングル サインオンの構成]** ダイアログを開きます。
   
   ![シングル サインオンの構成](./media/active-directory-saas-zscaler-two-tutorial/IC800202.png "Configure Single Sign-On")
2. **[ユーザーの ZScaler Two へのアクセスを設定してください]** ページで、**[Microsoft Azure AD のシングル サインオン]** を選択し、**[次へ]** をクリックします。
   
   ![シングル サインオンの構成](./media/active-directory-saas-zscaler-two-tutorial/IC800203.png "Configure Single Sign-On")
3. **[アプリの URL を構成する]** ページの **[ZScaler Two サインオン URL]** テキスト ボックスに、ユーザーが ZScaler Two アプリケーションのサインオンに使用する URL を入力して、**[次へ]** をクリックします。
   
   ![アプリケーション URL の構成](./media/active-directory-saas-zscaler-two-tutorial/IC800204.png "Configure App URL")
   
   > [!NOTE]
   > ご使用の環境の実際の値は、ZScaler Two サポート チームから入手できます。
   > 
   > 
4. **[ZScaler Two でのシングル サインオンの構成]** ページで、**[証明書のダウンロード]** をクリックして証明書をダウンロードし、証明書ファイルをコンピューターに保存します。
   
   ![シングル サインオンの構成](./media/active-directory-saas-zscaler-two-tutorial/IC800205.png "Configure Single Sign-On")
5. 別の Web ブラウザー ウィンドウで、ZScaler Two 企業サイトに管理者としてログインします。
6. 上部のメニューで **[管理]**をクリックします。
   
   ![Administration](./media/active-directory-saas-zscaler-two-tutorial/IC800206.png "Administration")
7. **[管理者とロールの管理]** をクリックし、**[ユーザーと認証の管理]** をクリックします。
   
   ![ユーザーと認証の管理](./media/active-directory-saas-zscaler-two-tutorial/IC800207.png "Manage Users & Authentication")
8. **[組織の認証オプションの選択]** セクションで、次の手順を実行します。
   
   ![Authentication](./media/active-directory-saas-zscaler-two-tutorial/IC800208.png "Authentication")
   
   1. **[SAML シングル サインオンを使用した認証]**を選択します。
   2. **[SAML シングル サインオン パラメーターの構成]**をクリックします。
9. **[SAML シングル サインオン パラメーターの構成]** ダイアログ ページで、次の手順に従い、**[完了]** をクリックします。
   
   ![シングル サインオン](./media/active-directory-saas-zscaler-two-tutorial/IC800209.png "Single Sign-On")
   
   1. Azure クラシック ポータルの **[ZScaler Two でのシングル サインオンの構成]** ダイアログ ページで、**[認証要求 URL]** の値をコピーし、**[URL of the SAML Portal to which users are sent for authentication (認証のためにユーザーに表示される SAML ポータルの URL)]** ボックスに貼り付けます。
   2. **[ログイン名を含む属性]** ボックスに「**NameID**」と入力します。
   3. ダウンロードした証明書をアップロードするには、 **[Zscaler pem]**をクリックします。
   4. **[SAML 自動プロビジョニングを有効にする]**を選択します。
10. **[ユーザー認証の構成]** ダイアログ ページで、次の手順に従います。
    
    ![Administration](./media/active-directory-saas-zscaler-two-tutorial/IC800210.png "Administration")
    
    1. **[保存]**をクリックします。
    2. **[今すぐ認証する]**をクリックします。
11. Azure クラシック ポータルの **[ZScaler Two でのシングル サインオンの構成]** ダイアログ ページで、シングル サインオンの構成確認を選択し、**[完了]** をクリックします。
    
    ![シングル サインオンの構成](./media/active-directory-saas-zscaler-two-tutorial/IC800211.png "Configure Single Sign-On")

## <a name="configuring-proxy-settings"></a>プロキシ設定の構成
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer でプロキシ設定を構成するには
1. **Internet Explorer**を開始します。
2. **[ツール]** メニューの **[インターネット オプション]** を選択し、**[インターネット オプション]** ダイアログを開きます。
   
   ![インターネット オプション](./media/active-directory-saas-zscaler-two-tutorial/IC769492.png "Internet Options")
3. **[接続]** タブをクリックします。
   
   ![接続](./media/active-directory-saas-zscaler-two-tutorial/IC769493.png "Connections")
4. **[LAN の設定]** をクリックして **[LAN の設定]** ダイアログを開きます。
5. [プロキシ サーバー] セクションで、次の手順を実行します。
   
   ![プロキシ サーバー](./media/active-directory-saas-zscaler-two-tutorial/IC769494.png "Proxy server")
   
   1. [LAN にプロキシ サーバーを使用する] をオンにします。
   2. [アドレス] ボックスに「 **gateway.zscalerone.net**」と入力します。
   3. [ポート] ボックスに「 **80**」と入力します。
   4. **[ローカル アドレスにはプロキシ サーバーを使用しない]**を選択します。
   5. **[OK]** をクリックして **[ローカル エリア ネットワーク (LAN) の設定]** ダイアログを閉じます。
6. **[OK]** をクリックして **[インターネット オプション]** ダイアログを閉じます。

## <a name="configuring-user-provisioning"></a>ユーザー プロビジョニングの構成
Azure AD ユーザーが ZScaler Two にログインできるようにするには、そのユーザーを ZScaler Two にプロビジョニングする必要があります。  
ZScaler Two の場合、プロビジョニングは手動で行います。

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>ユーザー プロビジョニングを構成するには、次の手順に従います。
1. **Zscaler** テナントにログインします。
2. **[管理]**をクリックします。
   
   ![Administration](./media/active-directory-saas-zscaler-two-tutorial/IC781035.png "Administration")
3. **[ユーザー管理]**をクリックします。
   
   ![Add](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Add")
4. **[ユーザー]** タブで、**[追加]** をクリックします。
   
   ![Add](./media/active-directory-saas-zscaler-two-tutorial/IC781037.png "Add")
5. [ユーザーの追加] セクションで、次の手順を実行します。
   
   ![ユーザーの追加](./media/active-directory-saas-zscaler-two-tutorial/IC781038.png "Add User")
   
   1. プロビジョニングする有効な AAD アカウントの **[ユーザー ID]**、**[ユーザー表示名]**、**[パスワード]**、**[パスワードの確認]** に入力し、**[グループ]** と **[部署]** を選択します。
   2. [ **Save**] をクリックします。

> [!NOTE]
> ZScaler Two から提供されている他の ZScaler Two ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。
> 
> 

## <a name="assigning-users"></a>ユーザーの割り当て
構成をテストするには、アプリケーションの使用を許可する Azure AD ユーザーを割り当てて、そのユーザーに、アプリケーションへのアクセス権を付与する必要があります。

### <a name="to-assign-users-to-zscaler-two-perform-the-following-steps"></a>ユーザーを ZScaler Two に割り当てるには、次の手順を実行します。
1. Azure クラシック ポータルで、テスト アカウントを作成します。
2. **ZScaler Two** アプリケーション統合ページで、**[ユーザーの割り当て]** をクリックします。
   
   ![ユーザーの割り当て](./media/active-directory-saas-zscaler-two-tutorial/IC800212.png "Assign Users")
3. テスト ユーザーを選択して、**[割り当て]** をクリックし、**[はい]** をクリックして割り当てを確定します。
   
   ![はい](./media/active-directory-saas-zscaler-two-tutorial/IC767830.png "Yes")

シングル サインオンの設定をテストする場合は、アクセス パネルを開きます。 アクセス パネルの詳細については、 [アクセス パネルの概要](active-directory-saas-access-panel-introduction.md)を参照してください。


