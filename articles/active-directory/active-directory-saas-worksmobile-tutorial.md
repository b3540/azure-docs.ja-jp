﻿---
title: "チュートリアル: Azure Active Directory と LINE WORKS の統合 | Microsoft Docs"
description: "Azure Active Directory と LINE WORKS の間でシングル サインオンを構成する方法について説明します。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8d1b3a49f15861d886822fa1a7328301e97ce37e
ms.contentlocale: ja-jp
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a>チュートリアル: Azure Active Directory と LINE WORKS の統合

このチュートリアルでは、LINE WORKS と Azure Active Directory (Azure AD) を統合する方法について説明します。

LINE WORKS と Azure AD の統合には、次の利点があります。

- LINE WORKS にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが各自の Azure AD アカウントで LINE WORKS に自動的にシングル サインオン (SSO) できるようにすることが可能です。
- 1 つの中央サイト (Microsoft Azure 管理ポータル) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「 [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)」を参照してください。

## <a name="prerequisites"></a>前提条件

Azure AD と LINE WORKS の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- LINE WORKS での SSO が有効なサブスクリプション

>[!NOTE]
>このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。
>

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[1 か月の試用版](https://azure.microsoft.com/pricing/free-trial/)を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD SSO をテストします。 

このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの LINE WORKS の追加
2. Azure AD SSO の構成とテスト

## <a name="add-works-mobile-from-the-gallery"></a>ギャラリーからの LINE WORKS の追加
Azure AD への WORKS MOBILE の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に WORKS MOBILE を追加する必要があります。

**ギャラリーから WORKS MOBILE を追加するには、次の手順を実行します。**

1. **[Azure 管理ポータル](https://portal.azure.com)**の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]
2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![アプリケーション][2]
3. ダイアログの上部にある **[追加]** をクリックします。

    ![アプリケーション][3]
4. 検索ボックスに、「**WORKS MOBILE**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_001.png)

5. 結果ウィンドウで **[WORKS MOBILE]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_0001.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、WORKS MOBILE で Azure AD の SSO を構成し、テストします。

SSO を機能させるには、Azure AD ユーザーに対応する WORKS MOBILE ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと WORKS MOBILE の関連ユーザーの間で、リンク関係が確立されている必要があります。

このリンク関係は、Azure AD の **[ユーザー名]** の値を、WORKS MOBILE の **[Username]** の値として割り当てることで確立されます。

WORKS MOBILE で Azure AD の SSO を構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[WORKS MOBILE のテスト ユーザーの作成](#creating-a-works-mobile-test-user)** - WORKS MOBILE で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure 管理ポータルで Azure AD の SSO を有効にして、WORKS MOBILE アプリケーションで SSO を構成します。

**WORKS MOBILE で Azure AD SSO を構成するには、次の手順に従います。**

1. Azure 管理ポータルの **LINE WORKS** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![Configure Single Sign-On][4]
2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_01.png)
3. **[WORKS MOBILE のドメインと URL]** セクションで、次の手順を実行します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_02.png)
  1. **[サインオン URL]** ボックスに、`https://<your-subdomain>.worksmobile.com/jp/myservice` のパターンを使用して URL を入力します。
  2. **[識別子]** ボックスに、値として「`worksmobile.com`」と入力します。

    >[!NOTE] 
    >これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新する必要があります。 ここでは、識別子に一意の文字列値を使用することをお勧めします。 これらの値を取得するには、[WORKS MOBILE サポート チーム](mailto:dl_ssoinfo@worksmobile.com)に連絡してください。 
    >
    >

4. **[SAML 署名証明書]** セクションで、**[新しい証明書の作成]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_03.png)     
5. **[新しい証明書の作成]** ダイアログで、カレンダー アイコンをクリックし、**期限日**を選択します。 **[保存]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_300.png)
6. **[SAML 署名証明書]** セクションで、**[Make new certificate active (新しい証明書を有効にする)]** をクリックし、**[保存]** をクリックします。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_04.png)
7. ポップアップ表示される **[Rollover certificate (ロール オーバー証明書)]** ウィンドウで、**[OK]** をクリックします。

    ![Configure Single Sign-On](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)
8. **[SAML 署名証明書]** セクションで、**[Certificate (Raw) (証明書 (Raw))]** をクリックし、コンピューターに証明書ファイルを保存します。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_05.png) 

9. **[WORKS MOBILE Configuration (WORKS MOBILE 構成)]** セクションで、**[Configure WORKS MOBILE (WORKS MOBILE を構成する)]** をクリックして、**[サインオンの構成]** ウィンドウを開きます。

    ![[シングル サインオンの構成]](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_06.png) 

    ![Configure Single Sign-On](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_07.png)

10. アプリケーション用に構成された SSO を取得するには、[WORKS MOBILE サポート チーム](mailto:dl_ssoinfo@worksmobile.com)に連絡し、次の項目を情報として提供してください。• ダウンロードした**証明書ファイル**、• **SAML シングル サインオン サービス URL**、• **SAML エンティティ ID**、• **サインアウト URL**

  
### <a name="create-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Microsoft Azure 管理ポータルで Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Microsoft Azure 管理ポータル**の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 
2. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 
3. ダイアログの上部にある **[追加]** をクリックして **[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 
4. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 
  1. **[名前]** ボックスに「**BrittaSimon**」と入力します。
  2. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。
  3. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。
  4. **[作成]**をクリックします。 

### <a name="create-a-works-mobile-test-user"></a>WORKS MOBILE のテスト ユーザーの作成

このセクションでは、WORKS MOBILE で Britta Simon というユーザーを作成します。 [WORKS MOBILE サポート チーム](mailto:dl_ssoinfo@worksmobile.com)と連携して、WORKS MOBILE プラットフォームにユーザーを追加してください。

### <a name="assign-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に WORKS MOBILE へのアクセスを許可することで、このユーザーが Azure SSO を使用できるようにします。

![ユーザーの割り当て][200] 

**Britta Simon を WORKS MOBILE に割り当てるには、次の手順を実行します。**

1. Azure 管理ポータルでアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で **[WORKS MOBILE]** を選択します。

    ![Configure Single Sign-On](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_50.png) 

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。
6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。
7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="test-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD の SSO 構成をテストします。

アクセス パネルで [WORKS MOBILE] タイルをクリックすると、WORKS MOBILE アプリケーションに自動的にサインオンします。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](active-directory-saas-tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png
