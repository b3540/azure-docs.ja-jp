---
title: "Azure Service Bus WCF Relay のチュートリアル | Microsoft Docs"
description: "Service Bus WCF Relay を使用して、Service Bus クライアント アプリケーションとサービスをビルドします。"
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: fee43f1e3fa50758870ffbe5b70c20fba517485e
ms.openlocfilehash: 70fc05f57b66fd14f047fe52f50fd3479a9f2d3d
ms.lasthandoff: 02/17/2017


---
# <a name="azure-wcf-relay-tutorial"></a>Azure WCF Relay のチュートリアル
このチュートリアルでは、Azure Relay を使用して簡単な WCF Relay クライアント アプリケーションとサービスを構築する方法について説明します。 [Service Bus メッセージング](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging)を使用した同様のチュートリアルについては、「[Service Bus キューの使用](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)」を参照してください。

このチュートリアルを利用すると、WCF Relay のクライアント アプリケーションとサービス アプリケーションを作成するために必要な手順を理解できます。 元の WCF サービスと同様に、サービスは&1; つ以上のエンドポイントを公開するコンストラクトであり、各エンドポイントは&1; つ以上のサービス操作を公開します。 サービスのエンドポイントでは、サービスが見つかるアドレス、クライアントがサービスとやり取りする必要がある情報を含むバインド、サービスがそのクライアントに提供する機能を定義するコントラクトを指定します。 WCF と WCF Relay の主な違いは、エンドポイントがコンピューターのローカルではなくクラウドで公開される点です。

このチュートリアルの一連のトピックに従って操作が完了すると、実行中のサービスとサービスの操作を呼び出すことができるクライアントが作成されます。 最初のトピックでは、アカウントを設定する方法について説明します。 次の手順では、コントラクトを使用するサービスを定義する方法、そのサービスを実装する方法、コードでそのサービスを構成する方法について説明します。 また、サービスをホストして実行する方法についても説明します。 作成されるサービスは自己ホスト型であり、クライアントとサービスは同じコンピューターで実行されます。 このサービスは、コードまたは構成ファイルを使用して構成することができます。

最後の&3; つの手順では、クライアント アプリケーションの作成方法、クライアント アプリケーションの構成方法、ホストの機能にアクセスできるクライアントを作成して使用する方法について説明します。

このセクションのすべてのトピックでは、開発環境として Visual Studio を使用していることを前提とします。

## <a name="create-a-service-namespace"></a>サービス名前空間の作成

最初の手順として、名前空間を作成し、[Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) キーを取得します。 名前空間により、リレー サービスが公開する各アプリケーションにアプリケーション境界が設けられます。 サービス名前空間が作成された時点で、SAS キーが生成されます。 サービス名前空間と SAS キーの組み合わせが、アプリケーションへのアクセスを Azure が認証する資格情報になります。 [こちらの手順](relay-create-namespace-portal.md)に従って、Relay 名前空間を作成します。

## <a name="define-a-wcf-service-contract"></a>WCF サービス コントラクトを定義する
サービス コントラクトは、サービスでサポートされる操作 (メソッドや関数を表す Web サービスの用語) を指定します。 コントラクトを作成するには、C++、C#、または Visual Basic インターフェイスを定義します。 インターフェイスの各メソッドは、特定のサービス操作に対応しています。 各インターフェイスには、[ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) 属性が適用されている必要があります。また、各操作には、[OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) 属性が適用されている必要があります。 [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) 属性があるインターフェイスのメソッドに [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) 属性がない場合、そのメソッドは公開されません。 以下の手順では、これらのタスクのコード例を示します。 コントラクトとサービスの概要については、WCF ドキュメントの [「サービスの設計と実装」](https://msdn.microsoft.com/library/ms729746.aspx) を参照してください。

### <a name="to-create-a-relay-contract-with-an-interface"></a>インターフェイスを使用してリレー コントラクトを作成するには
1. 管理者として Visual Studio を開きます。これには、**[スタート]** メニューの [Visual Studio] を右クリックし、**[管理者として実行]** を選択します。
2. 新しいコンソール アプリケーション プロジェクトを作成します。 **[ファイル]** メニューをクリックし、**[新規作成]** を選択して、**[プロジェクト]** をクリックします。 **[新しいプロジェクト]** ダイアログ ボックスで、**[Visual C#]** をクリックします (**[Visual C#]** が表示されない場合は、**[他の言語]** の下を確認してください)。 **[コンソール アプリケーション]** テンプレートをクリックし、**EchoService** という名前を付けます。 **[OK]** をクリックしてプロジェクトを作成します。

    ![][2]
3. Service Bus NuGet パッケージをインストールします。 WCF の **System.ServiceModel** と Service Bus ライブラリへの参照が、このパッケージによって自動的に追加されます。 [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) は、WCF の基本機能にプログラムでアクセスできるようにする名前空間です。 Service Bus は、サービス コントラクトの定義に WCF の多くのオブジェクトと属性を使用します。

    ソリューション エクスプローラーでソリューションを右クリックし、 **[ソリューションの NuGet パッケージの管理]**をクリックします。 **[参照]** タブをクリックして、`Microsoft Azure Service Bus` を検索します。 対応するプロジェクト名が **[バージョン]** ボックスで選択されていることを確認します。 **[インストール]**をクリックして、使用条件に同意します。

    ![][3]
4. エディターに Program.cs ファイルがまだ表示されていない場合は、ソリューション エクスプローラーでこのファイルをダブルクリックして開きます。
5. 次の using ステートメントをファイルの先頭に追加します。

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. 名前空間の名前を、既定の名前である **EchoService** から **Microsoft.ServiceBus.Samples** に変更します。

   > [!IMPORTANT]
   > このチュートリアルでは、C# の名前空間 **Microsoft.ServiceBus.Samples** を使用します。これは、コントラクトのマネージ型の名前空間で、[「WCF クライアントを構成する」](#configure-the-wcf-client)の手順に出てくる構成ファイルで使用されます。 このサンプルをビルドするときに必要な名前空間を指定できますが、それに応じて、アプリケーション構成ファイルでコントラクトとサービスの名前空間を変更しない限り、このチュートリアルは機能しません。 App.config ファイルで指定する名前空間は、C# ファイルで指定した名前空間と同じである必要があります。
   >
   >
7. `Microsoft.ServiceBus.Samples` 名前空間の宣言の直後 (ただし、名前空間内部) に、`IEchoContract` という名前の新しいインターフェイスを定義し、このインターフェイスに名前空間の値が `http://samples.microsoft.com/ServiceModel/Relay/` の `ServiceContractAttribute` 属性を適用します。 この名前空間の値は、コードのスコープ全体で使用する名前空間とは異なります。 代わりに、この名前空間の値は、このコントラクトの一意の識別子として使用されます。 名前空間を明示的に指定すると、既定の名前空間値がコントラクト名に追加されなくなります。

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > 通常、サービス コントラクトの名前空間には、バージョン情報を含む名前付けスキームが含まれています。 サービス コントラクトの名前空間にバージョン情報を含めると、サービスは、新しいサービス コントラクトを新しい名前空間を使用して定義し、それを新しいエンドポイントで公開することで、主要な変更を分離できます。 この方法では、クライアントは、古いサービス コントラクトを更新する必要なく、使用し続けることができます。 バージョン情報は、日付またはビルド番号で構成できます。 詳細については、[「サービスのバージョン管理」](http://go.microsoft.com/fwlink/?LinkID=180498)を参照してください。 このチュートリアルの目的では、サービス コントラクトの名前空間の名前付けスキームにバージョン情報は含まれません。
   >
   >
8. `IEchoContract` インターフェイス内で、`IEchoContract` コントラクトがインターフェイスで公開している単一操作のメソッドを宣言し、パブリック WCF Relay コントラクトの一部として公開するメソッドに `OperationContractAttribute` 属性を適用します。

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. 次に示すように、`IEchoContract` インターフェイスの定義の直後に、`IEchoContract` インターフェイスと `IClientChannel` インターフェイスの両方から継承するチャネルを宣言します。

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    チャネルは、ホストとクライアントが相互に情報を渡すときに経由する WCF オブジェクトです。 後で、2 つのアプリケーション間で情報をエコーするチャネルに対するコードを記述します。
10. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックするか、**Ctrl + Shift + B** キーを押して、ここまでの作業に問題がないことを確認します。

### <a name="example"></a>例
次のコードに、WCF Relay コントラクトを定義する基本的なインターフェイスを示します。

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

これでインターフェイスが作成されたので、このインターフェイスを実装することができます。

## <a name="implement-the-wcf-contract"></a>WCF コントラクトを実装する
Azure Relay を作成するには、まずコントラクトを作成する必要があります。コントラクトは、インターフェイスを使用して定義します。 インターフェイスの作成の詳細については、前の手順を参照してください。 次の手順はインターフェイスの実装です。 これには、ユーザー定義の `EchoService` インターフェイスを実装する `IEchoContract` というクラスの作成が含まれます。 インターフェイスを実装したら、App.config 構成ファイルを使用してインターフェイスを構成します。 構成ファイルには、サービス名、コントラクト名、リレー サービスとの通信に使用するプロトコルの種類など、アプリケーションに必要な情報が含まれています。 以下の手順では、これらのタスクに使用するコード例を示します。 サービス コントラクトの実装方法の全般的な説明については、WCF のドキュメントの [「サービス コントラクトの実装」](https://msdn.microsoft.com/library/ms733764.aspx) をご覧ください。

1. `IEchoContract` インターフェイスの定義の直後に、`EchoService` という新しいクラスを作成します。 `EchoService` クラスによって、`IEchoContract` インターフェイスが実装されます。

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    他のインターフェイス実装と同様に、別のファイルに指定した定義を実装することができます。 ただし、このチュートリアルでは、インターフェイス定義や `Main` メソッドと同じファイルに実装します。
2. `IEchoContract` インターフェイスに [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) 属性を適用します。 サービスの名前と名前空間をこの属性で指定します。 指定後の `EchoService` クラスは次のようになります。

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. `EchoService` クラスの `IEchoContract` インターフェイスで定義された `Echo` メソッドを実装します。

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. **[ビルド]**、**[ソリューションのビルド]** の順にクリックして、作業に問題がないことを確認します。

### <a name="to-define-the-configuration-for-the-service-host"></a>サービス ホストの構成を定義するには
1. 構成ファイルは WCF 構成ファイルとよく似ており、 サービス名、エンドポイント (つまり、クライアントとホストの相互通信用に Azure Relay が公開している場所)、バインド (通信に使用するプロトコルの種類) が記載されています。 主な違いは、構成されているこのサービス エンドポイントが、.NET Framework に含まれていない [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) バインドを参照している点です。 [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) は、サービスによって定義されたバインドの&1; つです。
2. **ソリューション エクスプローラー**で、App.config ファイルをダブルクリックして、Visual Studio エディターでそのファイルを開きます。
3. `<appSettings>` 要素内のプレースホルダーを、実際のサービスの名前空間名と先ほどコピーした SAS キーに置き換えます。
4. `<system.serviceModel>` タグ内に、`<services>` 要素を追加します。 1 つの構成ファイルで複数のリレー アプリケーションを定義できます。 ただし、このチュートリアルで定義するのは&1; つだけです。

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. `<services>` 要素内に、サービスの名前を定義する `<service>` 要素を追加します。

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. `<service>` 要素内で、エンドポイント コントラクトの場所に加え、エンドポイントのバインドの種類も定義します。

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    エンドポイントは、クライアントがホスト アプリケーションを検索する場所を定義します。 このチュートリアルでは、後でこの手順を使用して、Azure Relay を介してホストを完全に公開する URI を作成します。 バインドでは、リレー サービスと通信するプロトコルとして TCP を使用することを宣言します。
7. **[ビルド]** メニューの **[ソリューションのビルド]** をクリックして、作業に問題がないことを確認します。

### <a name="example"></a>例
次のコードは、サービス コントラクトの実装を示します。

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

次のコードは、サービス ホストに関連付けられる App.config ファイルの基本的な形式を示します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-the-relay-service"></a>リレー サービスに登録する基本的な Web サービスをホストして実行する
この手順では、Azure Relay サービスの実行方法について説明します。

### <a name="to-create-the-relay-credentials"></a>リレー資格情報を作成するには
1. `Main()` で、コンソール ウィンドウから読み取った名前空間と SAS キーを格納するための&2; つの変数を作成します。

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    SAS キーは、後でプロジェクトにアクセスするために使用します。 名前空間は、サービスの URI を作成するために、パラメーターとして `CreateServiceUri` に渡されます。
2. [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) オブジェクトを使用して、資格情報の種類として SAS キーを使用することを宣言します。 1 つ前の手順で追加したコードの直後に次のコードを追加します。

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>サービスのベース アドレスを作成するには
1 つ前の手順で追加したコードの後に、サービスのベース アドレスの `Uri` インスタンスを作成します。 この URI では、Service Bus スキーム、名前空間、サービス インターフェイスのパスを指定します。

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

"sb" は Service Bus スキームの省略形であり、プロトコルとして TCP を使用していることを示します。 これは、以前に [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) がバインドとして指定されたときにも、構成ファイルで示されていました。

このチュートリアルでは、URI は `sb://putServiceNamespaceHere.windows.net/EchoService` です。

### <a name="to-create-and-configure-the-service-host"></a>サービス ホストを作成して構成するには
1. 接続モードを `AutoDetect` に設定します。

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    接続モードは、サービスがリレー サービスとの通信に使用するプロトコル、つまり HTTP または TCP を表します。 既定の設定である `AutoDetect` を使用すると、サービスは、TCP が利用できる場合は TCP 経由で、TCP が利用できない場合は HTTP 経由で Azure Relay に接続しようとします。 これは、サービスがクライアントの通信に指定するプロトコルとは異なることに注意してください。 そのプロトコルは、使用されるバインドによって決まります。 たとえば、サービスでは [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) バインドを使用できますが、このバインドは、そのエンドポイントが HTTP 経由でクライアントと通信するように指定します。 同じサービスで、サービスが TCP 経由で Azure Relay と通信するように **ConnectivityMode.AutoDetect** を指定することもできます。
2. このセクションで先に作成した URI を使用して、サービス ホストを作成します。

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    サービス ホストは、サービスをインスタンス化する WCF オブジェクトです。 ここで、作成するサービスの種類 (`EchoService` 型) とサービスを公開するアドレスをサービス ホストに渡します。
3. Program.cs ファイルの先頭に、[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) と [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description) への参照を追加します。

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. `Main()` に戻り、パブリック アクセスを有効にするようにエンドポイントを構成します。

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    この手順では、プロジェクトの ATOM フィードを調べることでアプリケーションがパブリックに見つけられることを、リレー サービスに通知します。 **DiscoveryType** を **private** に設定した場合、クライアントは引き続きサービスにアクセスできます。 ただし、Relay 名前空間を検索するときには、サービスは表示されなくなります。 代わりに、クライアントは、エンドポイントのパスを事前に認識しておく必要があります。
5. App.config ファイルで定義されているサービス エンドポイントにサービスの資格情報を適用します。

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    前の手順で説明したように、構成ファイルで複数のサービスとエンドポイントを宣言しておくことができます。 宣言した場合、このコードは構成ファイルを走査し、資格情報を適用する必要があるすべてのエンドポイントを検索します。 ただし、このチュートリアルでは、構成ファイルのエンドポイントは&1; つだけです。

### <a name="to-open-the-service-host"></a>サービス ホストを開くには
1. サービスを開きます。

    ```csharp
    host.Open();
    ```
2. サービスが実行中であることをユーザーに通知し、サービスをシャットダウンする方法を説明します。

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. 完了したら、サービス ホストを閉じます。

    ```csharp
    host.Close();
    ```
4. **Ctrl + Shift + B** キーを押して、プロジェクトをビルドします。

### <a name="example"></a>例
次の例では、チュートリアルの前の手順で説明したサービス コントラクトと実装が含まれています。ここでは、コンソール アプリケーションでサービスをホストします。 次のコードをコンパイルして、EchoService.exe という実行可能ファイルを作成します。

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>サービス コントラクト用の WCF クライアントを作成する
次の手順では、クライアント アプリケーションを作成し、後の手順で実装するサービス コントラクトを定義します。 これらの手順の多くは、コントラクトの定義、App.config ファイルの編集、資格情報を使用したリレー サービスへの接続など、サービスの作成に使用した手順に似ています。 以下の手順では、これらのタスクに使用するコード例を示します。

1. 次の手順に従って、クライアント用の現在の Visual Studio ソリューションに新しいプロジェクトを作成します。

   1. ソリューション エクスプローラーで、サービスを含む同じソリューションの (プロジェクトではなく) 現在のソリューションを右クリックし、**[追加]** をクリックします。 次に、**[新しいプロジェクト]** をクリックします。
   2. **[新しいプロジェクトの追加]** ダイアログ ボックスで **[Visual C#]** をクリックします (**[Visual C#]** が表示されない場合は、**[他の言語]** の下を探してください)。**[コンソール アプリケーション]** テンプレートを選択し、**EchoClient**という名前を付けます。
   3. **[OK]**をクリックします。
      <br />
2. ソリューション エクスプローラーで **EchoClient** プロジェクトの Program.cs ファイルをダブルクリックしてエディターで開きます (まだ開いていない場合)。
3. 名前空間の名前を、既定の名前である `EchoClient` から `Microsoft.ServiceBus.Samples` に変更します。
4. [Service Bus NuGet パッケージ](https://www.nuget.org/packages/WindowsAzure.ServiceBus)をインストールします。ソリューション エクスプローラーで **EchoClient** プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。 **[参照]** タブをクリックして、`Microsoft Azure Service Bus` を検索します。 **[インストール]**をクリックして、使用条件に同意します。

    ![][3]
5. Program.cs ファイルに、[System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) 名前空間の `using` ステートメントを追加します。

    ```csharp
    using System.ServiceModel;
    ```
6. 次の例に示すように、サービス コントラクトの定義を名前空間に追加します。 この定義は **Service** プロジェクトで使用される定義と同じです。 `Microsoft.ServiceBus.Samples` 名前空間の先頭に次のコードを追加する必要があります。

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. **Ctrl + Shift + B** キーを押して、クライアントをビルドします。

### <a name="example"></a>例
次のコードは、**EchoClient** プロジェクトの Program.cs ファイルの現在の状態を示します。

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>WCF クライアントを構成する
この手順では、このチュートリアルで前に作成したサービスにアクセスする基本的なクライアント アプリケーションの App.config ファイルを作成します。 この App.config ファイルでは、エンドポイントのコントラクト、バインド、および名前を定義します。 以下の手順では、これらのタスクに使用するコード例を示します。

1. ソリューション エクスプローラーで、**EchoClient** プロジェクトの **[App.config]** をダブルクリックし、Visual Studio エディターでファイルを開きます。
2. `<appSettings>` 要素内のプレースホルダーを、実際のサービスの名前空間名と先ほどコピーした SAS キーに置き換えます。
3. system.serviceModel 要素内に、`<client>` 要素を追加します。

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    この手順により、WCF スタイルのクライアント アプリケーションを定義していることを宣言します。
4. `client` 要素内で、エンドポイントの名前、コントラクト、バインドの種類を定義します。

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    この手順では、エンドポイントの名前、サービスで定義されたコントラクト、クライアント アプリケーションが Azure Relay との通信に TCP を使用することを定義します。 エンドポイント名は、次の手順でこのエンドポイント構成をサービス URI とリンクするために使用されます。
5. **[ファイル]**、**[すべて保存]** の順にクリックします。

## <a name="example"></a>例
次のコードは、Echo クライアントの App.config ファイルを示します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>WCF クライアントを実装する
この手順では、このチュートリアルで作成したサービスにアクセスする基本的なクライアント アプリケーションを実装します。 サービスと同様に、クライアントも、Azure Relay へアクセスするために多くの同じ操作を実行します。

1. 接続モードを設定します。
2. ホスト サービスの場所を示す URI を作成します。
3. セキュリティ資格情報を定義します。
4. 資格情報を接続に適用します。
5. 接続を開きます。
6. アプリケーション固有のタスクを実行します。
7. 接続を閉じます。

ただし、主な違いの&1; つとして、クライアント アプリケーションではリレー サービスへの接続にチャネルを使用するのに対し、サービスでは **ServiceHost** の呼び出しを使用します。 以下の手順では、これらのタスクに使用するコード例を示します。

### <a name="to-implement-a-client-application"></a>クライアント アプリケーションを実装するには
1. 接続モードを **AutoDetect** に設定します。 **EchoClient** アプリケーションの `Main()` メソッドの中に次のコードを追加します。

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. コンソールから読み取られるサービス名前空間と SAS キーの値を保持する変数を定義します。

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Relay プロジェクトでのホストの場所を定義する URI を作成します。

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. サービス名前空間のエンドポイントの資格情報オブジェクトを作成します。

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. App.config ファイルに記述されている構成を読み込むチャネル ファクトリを作成します。

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    チャネル ファクトリは、サービスおよびクライアント アプリケーションが通信に使用するチャネルを作成する WCF オブジェクトです。
6. 資格情報を適用します。

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. サービスに対するチャネルを作成して開きます。

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. エコーの基本的なユーザー インターフェイスと機能を記述します。

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    このコードではサービスのプロキシとしてチャネル オブジェクトのインスタンスを使用することに注意してください。
9. チャネルを閉じ、ファクトリを終了します。

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>アプリケーションを実行するには
1. **Ctrl + Shift + B** キーを押して、ソリューションをビルドします。 前の手順で作成したサービス プロジェクトとクライアント プロジェクトの両方がビルドされます。
2. クライアント アプリケーションを実行する前に、サービス アプリケーションが実行されていることを確認してください。 Visual Studio のソリューション エクスプローラーで **[EchoService]** ソリューションを右クリックし、**[プロパティ]** をクリックします。
3. [ソリューションのプロパティ] ダイアログ ボックスの **[スタートアップ プロジェクト]** をクリックし、**[マルチ スタートアップ プロジェクト]** ボタンをクリックします。 **EchoService** がリストの先頭に表示されていることを確認してください。
4. **EchoService** プロジェクトと **EchoClient** プロジェクトの両方の **[アクション]** ボックスを **[開始]** に設定します。

    ![][5]
5. **[プロジェクトの依存関係]**をクリックします。 **[プロジェクト]** ボックスの **[EchoClient]** を選択します。 **[依存先]** ボックスで、**[EchoService]** がオンになっていることを確認します。

    ![][6]
6. **[OK]** をクリックして、**[プロパティ]** ダイアログを閉じます。
7. **F5** キーを押して両方のプロジェクトを実行します。
8. 両方のコンソール ウィンドウが開き、名前空間名の入力が求められます。 先にサービスを実行しておく必要があるので、**[EchoService]** コンソール ウィンドウで名前空間を入力し、**Enter** キーを押します。
9. 次に、SAS キーの入力を求められます。 SAS キーを入力し、Enter キーを押します。

    コンソール ウィンドウの出力例を次に示します。 ここで示されている値は例での使用のみを目的としています。

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    次の例に示すように、サービス アプリケーションによってリッスンされているアドレスがコンソール ウィンドウに出力されます。

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
10. **[EchoClient]** コンソール ウィンドウに、先ほどサービス アプリケーションについて入力したものと同じ情報を入力します。 前の手順に従い、クライアント アプリケーションについても同じサービス名前空間と SAS キーの値を入力してください。
11. これらの値を入力すると、クライアントはサービスに対するチャネルを開き、次のコンソール出力例に示すように、なんらかのテキストを入力するよう求めます。

    `Enter text to echo (or [Enter] to exit):`

    サービス アプリケーションに送信するテキストを入力し、Enter キーを押します。 このテキストは、Echo サービス操作を介してサービスに送信され、次の出力例のように、サービス コンソール ウィンドウに表示されます。

    `Echoing: My sample text`

    クライアント アプリケーションは、`Echo` 操作の戻り値を受け取ります。これは元のテキストで、コンソール ウィンドウに出力されます。 クライアント コンソール ウィンドウの出力例を次に示します。

    `Server echoed: My sample text`
12. この方法で、クライアントからサービスにテキスト メッセージの送信を続けることができます。 終了したら、クライアント コンソール ウィンドウとサービス コンソール ウィンドウで Enter キーを押して両方のアプリケーションを終了します。

## <a name="example"></a>例
次の例では、クライアント アプリケーションを作成する方法、サービスの操作を呼び出す方法、操作の呼び出しが完了した後でクライアントを終了する方法を示します。

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>次のステップ
このチュートリアルでは、Service Bus の WCF Relay 機能を使用して、Azure Relay クライアント アプリケーションとサービスを構築する方法を紹介しました。 [Service Bus メッセージング](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging)を使用した同様のチュートリアルについては、「[Service Bus キューの使用](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)」を参照してください。

Azure Relay の詳細については、次のトピックを参照してください。

* [Azure Service Bus アーキテクチャの概要](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Azure Relay の概要](relay-what-is-it.md)
* [.NET で WCF リレー サービスを使用する方法](service-bus-dotnet-how-to-use-relay.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png

