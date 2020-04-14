﻿---
lab:
    title: '08 - 仮想マシンを管理する'
   module: 'モジュール 08 - 仮想マシン'
---

# ラボ 08 - Virtual Machines を管理する
# 受講者用ラボ マニュアル

## ラボ シナリオ

Azure 仮想マシンをデプロイおよび構成するためのさまざまなオプションを特定する作業を担当することになりました。まず、Azure 仮想マシンを使用する場合に実装できる計算とストレージの回復性とスケーラビリティのオプションを決定する必要があります。次に、Azure Virtual Machine scale Set を使用する場合に使用できる計算とストレージの回復性とスケーラビリティ オプションを調査する必要があります。また、Azure 仮想マシン のカスタム スクリプト拡張機能を使用して、仮想マシンと仮想マシン スケール セットを自動的に構成する機能についても調査する必要があります。

## 目標

このラボでは、次の内容を学習します。

+ タスク 1: Azure portal と Azure Resource Manager テンプレートを使用して、ゾーン復元可能な Azure Virtual Machines をデプロイする
+ タスク 2: 仮想マシン拡張機能を使用して Azure virtual machine を構成する
+ タスク 3: Azure Virtual Machine の計算とストレージをスケーリングする
+ タスク 4: Azure portal を使用して、ゾーン復元可能な Azure Virtual Machine Scale Sets をデプロイする
+ タスク 5: 仮想マシン拡張機能を使用して Azure Virtual Machine Scale Sets を構成する
+ タスク 6: Azure Virtual Machine Scale Sets の計算とストレージをスケーリングする (オプション)

## 指示

### 演習 1

#### タスク 1: Azure portal と Azure Resource Manager テンプレートを使用して、ゾーン復元可能な Azure Virtual Machines をデプロイする

このタスクでは、Azure portal と Azure Resource Manager テンプレートを使用して、Azure 仮想マシンをさまざまな可用性ゾーンにデプロイします。

1. [Azure portal](http://portal.azure.com) にログインします。

1. Azure portalで**仮想マシン** を検索して選択し、[**仮想マシン**] ブレードで [**+ 追加**] をクリックします。

1. [**仮想マシンの作成**] ブレードの [**基本**] タブで、次の設定を指定します (その他は規定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | サブスクリプション | このラボで使用する Azure サブスクリプションの名前 |
    | リソース グループ | 新しいリソース グループ **az104-08-rg01** の名前 |
    | 仮想マシン名 | **az104-08-vm0** |
    | リージョン | 可用性ゾーンをサポートし Azure 仮想マシンをプロビジョニングできるリージョンの 1 つを選択します | 
    | 可用性オプション | **可用性ゾーン** |
    | 可用性ゾーン | **1** |
    | イメージ | **Windows Server 2019 Datacenter** |
    | Azure Spot インスタンス | **No** |
    | サイズ | **Standard D2s v3** |
    | ユーザー名 | **Student** |
    | パスワード | **Pa55w.rd1234** |
    | パブリック受信ポート | **なし** |
    | Windows Server ライセンスを持っている | **No** |

1. [**次へ:**] をクリックする[**[ディスク >**] をクリックし、[**仮想マシンの作成**] ブレードの [**ディスク**] タブで、次の設定を指定します (他のユーザーには既定値を使用します)。

    | 設定 | 値 | 
    | --- | --- |
    | OS ディスク タイプ:  | **Standard HDD** |
    | Ultra Disk の互換性を有効にする | **No** |

1. [**次へ:**] をクリックする**ネットワーク >** で、**仮想マシンの作成**ブレードの**ネットワーク**タブで、**仮想ネットワーク** テキスト ボックスの下にある**新規作成**をクリックします。

1. **仮想ネットワークの作成** ブレードで、次の設定を指定します (他のユーザーは規定値のままにします)。 

    | 設定 | 値 | 
    | --- | --- |
    | 名前 | **az104-08-rg01-vnet** |
    | アドレス範囲 | **10.80.0.0/20** |
    | サブネット名 | **subnet0** |
    | サブネット範囲 | **10.80.0.0/24** |
 
1. **OK** をクリックし、 **仮想マシンの作成** ブレードの **ネットワーク** タブに戻り 、次の設定を指定します (他のユーザーは既定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | パブリック IP | **なし** |
    | NIC ネットワーク セキュリティ グループ | **なし** |
    | 高速ネットワーク | **オフ** |
    | この仮想マシンを既存の負荷分散ソリューションの背後に配置しますか？ | **No** |

1. [**Next: Management >**] をクリックし、[**仮想マシンの作成**] ブレードの [**管理**] タブで、次の設定を指定します (他のユーザーは既定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | 基本プランを無料にします。 | **No** |
    | ブート診断 | **オフ** |

1. [**Next: Advanced >**] をクリックし、[**仮想マシンの作成**] ブレードの [**管理**] タブで、利用可能な設定を変更せずに確認し、[**確認と作成**] をクリックします

1. [**確認と作成**] ブレードで、[**作成**] をクリックします。

1. [**デプロイ**] ブレードで、[**テンプレート**] をクリックします。

1. 進行中のデプロイを表すテンプレートをレビューし、[**デプロイ**] をクリックします。

    >**注**: このオプションを使用して、可用性ゾーンを除く、一致する構成を持つ 2つ目の仮想マシンをデプロイします。 

1. [**カスタム展開**] ブレードで、次の設定を指定します （他のユーザーは、既定値のままにします)。 

    | 設定 | 値 | 
    | --- | --- |
    | リソース グループ | **az104-08-rg01** |
    | ネットワーク インターフェイス 名 | **az104-08-vm1-nic1** |
    | 仮想マシン名 | **az104-08-vm1** |
    | 管理者ユーザー名： | **Student** |
    | 管理者のパスワード | **Pa55w.rd1234** |
    | ゾーン | **2** |

    >**注**: 仮想マシンとそのネットワーク インターフェイスを含むテンプレートを使用して、デプロイする個別のリソースのプロパティに対応するパラメーターを変更する必要があります。また、2つの仮想マシンで構成されるデプロイメントをゾーン冗長にする場合は、別の可用性ゾーンを指定する必要があります。

1. **上記の利用規約に同意します**のチェックマークをオンにし、**購入**を クリックします。

    >**注**: 次のタスクを進める前に、両方のデプロイメントが完了するのを待ちます。これにはおよそ 5 分間かかる場合があります。

#### タスク 2: 仮想マシン拡張機能を使用して Azure virtual machine を構成する

このタスクでは、カスタム スクリプト仮想マシン拡張機能を使用して、前のタスクで展開した 2つの Azure virtual machine に Windows Server Web サーバー ロールをインストールします。 

1. Azure portal で、 **仮想マシン** を検索して選択し、[**仮想マシン**] ブレードで [**az104-08-vm0**] をクリックします。

1. [**az104-08-vm0**] 仮想マシン ブレードの [**設定**] セクションで、[**拡張機能**] をクリックし、[**+ 追加**] をクリックします。

1. [**新しいリソース**] ブレードで、[**カスタム スクリプト拡張機能**] をクリックし、[**作成**] をクリックします。

1. [**拡張機能のインストール**] ブレードで、 スクリプト **az104-08-install_IIS.ps1** を **\\Allfiles\\Labs\\08** からアップロードし 、[**OK**] をクリックします。       

1. Azure portal で、[**仮想マシン**] を検索して選択し、[**仮想マシン**] ブレードで [**az104-08-vm1**] をクリックします。

1. **az104-08-vm1** ブレードの [**設定**] セクションで、[**テンプレートのエクスポート**] をクリックします。

1. [**az104-08-vm1 - テンプレートのエクスポート**] ブレードで、[**展開**] をクリックします。 

1. [**カスタム展開**] ブレードで、[**テンプレートの編集**] をクリックします。

1. [**テンプレートの編集**] ブレードのテンプレートのコンテンツを表示するセクションに、次のコードを、**20** 行目の始めに挿入します。(`    "resources": [` 行のすぐ下)

   ```json
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "az104-08-vm1/customScriptExtension",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "az104-08-vm1"
            ],
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "CustomScriptExtension",
                "typeHandlerVersion": "1.7",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "commandToExecute": "powershell.exe Install-WindowsFeature -name Web-Server -IncludeManagementTools && powershell.exe remove-item 'C:\\inetpub\\wwwroot\\iisstart.htm' && powershell.exe Add-Content -Path 'C:\\inetpub\\wwwroot\\iisstart.htm' -Value $('Hello World from ' + $env:computername)"
              }
            }
        },   

   ```

    >**注**: テンプレートのこのセクションでは、Azure PowerShell を介して最初の仮想マシンに以前デプロイしたのと同じ Azure virtual machineのカスタム スクリプト拡張機能を定義します。

1. [**保存**] をクリックし、[**カスタム テンプレート**] ブレードに戻って 、[**上記の利用規約に同意します**]チェック ボックスをオンにして、[**購入**] をクリックします。

    >**注**: **リソース グループがテンプレート内の 1つ以上のリソースでサポートされていない場所にあります**というメッセージは無視します。**別のリソース グループを選択してください**。この結果は正常の動作のためこの場合は無視できます。

    >**注**: template deployment が完了するのを待ちます。その進行状況は **az104-08-vm0** および **az104-08-vm1** の仮想マシンの **拡張機能** ブレードから監視できます。これには 3 分ほどかかります。

1. カスタム スクリプト拡張ベースの構成が正常に実行されたことを確認するには、 [**az104-08-vm1**] ブレードに戻り、[**操作**] セクションの [**コマンドの実行**] をクリックし、コマンドの一覧で [**RunPowerShellScript**] をクリックします。

1. [**コマンド スクリプトの実行**] ブレードで、次のコマンドを入力し、[**実行**] をクリックして [**az104-08-vm0**] でホストされている Web サイトにアクセスします。

   ```pwsh
   Invoke-WebRequest -URI http://10.80.0.4 -UseBasicParsing
   ```

    >**Note**: コマンドレットの実行を完了するために Internet Explorer への依存関係を排除するには、 **-UseBasicParsing** パラメーターが必要です。

    >**注**: **az104-08-vm0 に接続** し、`Invoke-WebRequest -URI http://10.80.0.5` を実行して、 **az104-08-vm1** でホストされている Web サイトにアクセスすることもできます。

#### タスク 3: Azure Virtual Machine の計算とストレージをスケーリングする

このタスクでは、サイズを変更して Azure 仮想マシンのコンピューティングをスケーリングし、データ ディスクを接続して構成することでストレージをスケーリングします。

1. Azure portal で、 **仮想マシン** を検索して選択し、[**仮想マシン**] ブレードで [**az104-08-vm0**] をクリックします。

1. [**az104-08-vm0** 仮想マシン] ブレードで、[**サイズ**] をクリックし、仮想マシンのサイズを **Standard DS1_v2** にします。

    >**注**: **Standard DS1_v2** がない場合は、別のサイズを選択します。

1. **az104-08-vm0** 仮想マシン ブレードで、[**ディスク**] をクリックし、[**+データ ディスクの追加**] をクリックして、[**名前**] のドロップダウン リストで、[**ディスクの作成**] をクリックします。

1. 次の設定でマネージド ディスクを作成します (他の設定はデフォルト値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | ディスク名 | **az104-08-vm0-datadisk-0** |
    | ソース タイプ | **なし** |
    | アカウント タイプ | **Premium SSD** |
    | サイズ | **1024 GiB** |


1. [**az104-08-vm0 - ディスク**] ブレードに戻り[**+ データ ディスクの追加**] をクリックし、[**名前**] ドロップダウン リストで [**ディスクの作成**] をクリックします。

1. 次の設定でマネージド ディスクを作成します (他のディスクには既定値を残します)。

    | 設定 | 値 | 
    | --- | --- |
    | ディスク名 | **az104-08-vm0-datadisk-1** |
    | ソース タイプ | **なし** |
    | アカウント タイプ | **Premium SSD** |
    | サイズ | **1024 GiB** |

1. [**az104-08-vm0 - ディスク**] ブレードに戻り 、[**保存**] をクリックします。

1. [**操作**] セクションの [**az104-08-vm0**] ブレードで [**コマンドの実行**] をクリックし、コマンドの一覧で [**RunPowerShellScript**]をクリックします。

1. [**コマンド スクリプトの実行**] ブレードで、次のようにタイプし、[**実行**] をクリック して、単純なレイアウトと固定プロビジョニングを備えた、 2 つの新しく接続されたディスクで構成されるドライブ Z を作成します。  

   ```pwsh
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 2046GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```

    > **注意**: コマンドが正常に完了したことを確認します。

1. Azure portal で、[**仮想マシン**] を検索して選択し、[**仮想マシン**] ブレードで [**az104-08-vm1**] をクリックします。

1. **az104-08-vm1** ブレードの [**設定**] セクションで、[**テンプレートのエクスポート**] をクリックします。

1. [**az104-08-vm1 - テンプレートのエクスポート**] ブレードで、[**展開**] をクリックします。 

1. [**カスタム展開**] ブレードで、[**テンプレートの編集**] をクリックします。

1. **テンプレートの編集** ブレードの [テンプレートの内容を表示するセクション] で、 **30**行目の `                    "vmSize" を置き換えます。「Standard_D2s_v3」` を次の行で表示します。

   ```json
                    "vmSize": "Standard_DS1_v2"

   ```

    >**Note**: テンプレートのこのセクションでは、Azure portalを介して最初の仮想マシンに指定したものと同じ Azure Virtual Machine サイズを定義します。


1. **テンプレートの編集** ブレードの、テンプレートの内容を表示するセクションで、**49** 行目を次のコードに置き換えます ( "dataDisks": [ ] の行) 。

   ```json
                    "dataDisks": [
                      {
                        "lun": 0,
                        "name": "az104-08-vm1-datadisk0",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      },
                      {
                        "lun": 1,
                        "name": "az104-08-vm1-datadisk1",
                        "diskSizeGB": "1024",
                        "caching": "ReadOnly",
                        "createOption": "Empty"
                      }
                    ]
   ```

    >**Note**: テンプレートのこのセクションでは、Azure portal を介して最初の仮想マシンのストレージ構成と同様に、2つのマネージド ディスクを作成し、**az104-08-vm1** に添付します。

1. **保存** をクリックし、**カスタム テンプレート** ブレードに戻って 、**上記の利用規約に同意します**チェック ボックスをオンにして、 **購入**をクリックします。

    >**注**: **リソース グループがテンプレート内の 1つ以上のリソースでサポートされていない場所にあります**というメッセージは無視します。**別のリソース グループを選択してください**。この結果は正常な動作のため、この場合は無視できます。

    >**注**: template deployment が完了するのを待ちます。**az104-08-vm1**仮想マシンの**拡張機能**ブレードから進行状況を監視できます。これには　3分以内で終わるはずです。

1. [**操作**] セクションの [**az104-08-vm1**] ブレードに戻り 、[**コマンドの実行**] をクリックし、コマンドの一覧で[**RunPowerShellScript**] をクリックします。  

1. [**コマンド スクリプトの実行**] ブレードで、次のようにタイプし、[**実行**] をクリック して、単純なレイアウトと固定プロビジョニングを備えた 2つの新しく接続されたディスクで構成されるドライブ Z を作成します。   

   ```pwsh
   New-StoragePool -FriendlyName storagepool1 -StorageSubsystemFriendlyName "Windows Storage*" -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

   New-VirtualDisk -StoragePoolFriendlyName storagepool1 -FriendlyName virtualdisk1 -Size 2046GB -ResiliencySettingName Simple -ProvisioningType Fixed

   Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName virtualdisk1)

   New-Partition -DiskNumber 4 -UseMaximumSize -DriveLetter Z
   ```
    > **注意**: コマンドが正常に完了したことを確認します。

#### タスク 4: Azure portal を使用して、ゾーン復元可能な Azure Virtual Machine Scale Sets をデプロイする

このタスクでは、Azure portalを使用して可用性ゾーン間でAzure 仮想マシン スケール セットをデプロイします。

1. Azure portalで、**仮想マシン スケール セット** を検索して選択し、[**仮想マシン スケール セット**] ブレードで [**+ 追加**] をクリックします。

1. [**仮想マシン スケール セットの作成**] ブレードの [**基本**] タブで 、次の設定を指定し (他の設定は既定値のままにします)、[**Next : Disks >**] をクリックします。

    | 設定 | 値 | 
    | --- | --- |
    | サブスクリプション | このラボで使用している Azure サブスクリプションの名前 |    
    | リソース グループ | 新しいリソース グループ **az104-08-rg02** の名前 |    
    | 仮想マシン スケール セット名 | **az10408vmss0** |
    | リージョン | 可用性ゾーンをサポートするリージョンの 1 つを選択、Azure Virtual Machines をプロビジョニングできる場所は、このラボの前半で Virtual Machines のデプロイに使用したものとは異なります | 
    | 可用性ゾーン | **ゾーン 1, 2, 3** |
    | イメージ | **Windows Server 2016 Datacenter** |
    | Azure Spot インスタンス | **No** |
    | サイズ | **Standard (D2s_v3)** |    
    | ユーザー名 | **Student** |
    | パスワード | **Pa55w.rd1234** |
    | 既にWindows Server ライセンスを持っていますか?:  | **No** |

    >**注**: Windows virtual machine のAvailability Zonesへのデプロイをサポートする Azure リージョンの一覧については、[AzureにおけるAvailability Zonesとは？](https://docs.microsoft.com/ja-jp/azure/availability-zones/az-overview)を参照してください。

1. [**仮想マシン スケール セットの作成**] ブレードの [ディスク] タブ で、既定値を受け入れて [**Next : Networking >**] をクリック します。

1. [**仮想マシン スケール セットの作成**] ブレードの [ネットワーク] タブで 、 [**Virtual network**] テキスト ボックスの下の [**仮想ネットワークの作成**] リンクをクリック し、次の設定で新しい仮想ネットワークを作成します (他のユーザーは既定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | 名前 | **az104-08-rg02-vnet** |
    | アドレス範囲 | **10.82.0.0/20** |
    | サブネット名 | **subnet0** |
    | サブネット範囲 | **10.82.0.0/24** |
 
    >**注**: 新しい仮想ネットワークを作成し、[**仮想マシン スケール セットの作成**] ブレードの [**ネットワーク**]タブに戻ると、仮想ネットワークの値が自動的に **az104-08-rg02-vnet** に設定されます。

1. [**仮想マシン スケール セットの作成**] ブレードの [**ネットワーク**] タブに戻り 、ネットワーク インターフェイス エントリの右側にある [**ネットワーク インターフェイスの編集**] アイコンをクリックします。

1. [**ネットワーク インターフェイスの編集**] ブレードの [**NIC ネットワーク セキュリティ グループ**] セクションで、[**詳細設定**]をクリックし、[**ネットワーク セキュリティ グループの構成**] ドロップダウン リストから [**新規作成**] をクリック します。

1. [**ネットワーク セキュリティ グループの作成**] ブレードで、次の設定を指定します (他のユーザーは、既定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | 名前 | **az10408vmss0-nsg** |

1. [**受信規則の追加**] をクリックし、次の設定を使用して受信セキュリティ規則を追加します (他のユーザーは、既定値のままにします)。

    | 設定 | 値 | 
    | --- | --- |
    | ソース | **任意** |
    | ソース ポート範囲 | **\*** |
    | 目的地 | **任意** |
    | ターゲットのポート範囲 | **80** |
    | プロトコル | **TCP** |
    | アクション | **Allow** |
    | 優先度 | **1010** | 
    | 名前 | **custom-allow-http** |

1. [**追加**] をクリックし、[**ネットワーク セキュリティ グループの作成**] ブレードに戻って [**OK**] をクリックします。

1. [**ネットワーク インターフェイスの編集**] ブレードに戻り 、[**パブリック IP アドレス**] セクションで [**有効**] 、[**OK**] の順にクリックします。

1. [**仮想マシン スケール セットの作成**] ブレードの [**ネットワーク**] タブに戻り 、次の設定を指定し (他のユーザーには既定値をそのまま使用) 、[**Next : Scaling >**] をクリック します。

    | 負荷分散装置の使用 | **Yes** |
    | 負荷分散 オプション | **Azure Load Balancer** |
    | ロード バランサーの選択 | **(new) az10408vmss0-lb** |
    | バックエンド プールを選択する | **(new) bepool** |
    
1. [**仮想マシン スケール セットの作成**] ブレードの [**スケーリング**] タブ で、次の設定を指定し (他の設定は既定値のままにします)、[**Next : Management >**] をクリック します。

    | 設定 | 値 | 
    | --- | --- |
    | 初期インスタンス数 | **2** |
    | スケーリングポリシー | **Manual** |

1. [**仮想マシン スケール セットの作成**] ブレードの [**管理**] タブ で、次の設定を指定し (他の設定は既定値のまま)、[**次へ**] をクリック します。**ヘルス >**:

    | 設定 | 値 | 
    | --- | --- |
    | ブート診断 | **Off** |
    
1. [**仮想マシン スケール セットの作成**] ブレードの [**ヘルス**] タブで 、変更を加えずに既定の設定を確認し、[**次へ**] をクリックします。**詳細 >**。

1. [**仮想マシン スケール セットの作成**] ブレードの [**詳細設定**] タブで 、次の設定を指定し (他の設定は既定値のまま) 、[**確認と作成**] をクリックします。

    | 設定 | 値 | 
    | --- | --- |
    | 拡散アルゴリズム | **最大拡散** |

1. [**仮想マシン スケール セットの作成**] ブレードの [**確認 + 作成**] タブで 、検証が成功したことを確認し、[**作成**] をクリックします。

    >**注**: 仮想マシン スケール セットのデプロイの完了を待ちます。これは約 5 分間かかります。


#### タスク 5: 仮想マシン拡張機能を使用して Azure Virtual Machine Scale Sets を構成する

このタスクでは、カスタム スクリプト仮想マシン拡張機能を使用して、前のタスクでデプロイした Azure 仮想マシン スケール セットのインスタンスに Windows Server Web サーバー ロールをインストールします。 

1. Azure portal で [**仮想マシン スケール セット**] ブレードを更新し、 [**az10408vms0**] をクリックします。

1. [**az10408vms0**] ブレードで、[**拡張機能**] をクリックし、[**+ 追加**] をクリックします。     

1. [**新しいリソース**] ブレードで、[**カスタム スクリプト拡張機能**] をクリックし、[**作成**] をクリックします。

1. [**拡張機能のインストール**] ブレードで、 スクリプト **az104-08-install_IIS.ps1** を **\\Allfiles\\Labs\\08** からアップロードし 、[**OK**] をクリックします。       

    >**注**: 拡張機能のインストールが完了するのを待ってから、次の手順に進みます。

1. [**az10408vms0**]ブレードの [**設定**] セクションで 、[**インスタンス**] をクリックし、仮想マシン スケール セットの 2 つのインスタンスの横にあるチェック ボックスをオンにし、 [**アップグレード**] をクリックします。確認画面が表示されたら、[**Yes**] をクリックします。

    >**注**: 次のタスクを進める前に、アップグレードが完了するのを待ちます。

1. Azure portalで、**ロード バランサー** を検索して選択し、[ロード バランサー] の一覧で [**az10408vmss0lb**] をクリックします。

1. **az10408vmss0lb** ブレードで、 ロード バランサーのフロントエンドに割り当て済みの**パブリック IP アドレス**の値をメモし、新しいブラウザー タブを開いて、その IP アドレスに移動します。

    >**注**: ブラウザー ページに、Azure 仮想マシン スケール セット **az10408vmss0**のいずれかのインスタンスの名前が表示されることを確認します。


#### タスク 6: Azure Virtual Machine Scale Sets の計算とストレージをスケーリングする

このタスクでは、仮想マシン スケール セット インスタンスのサイズを変更し、自動スケール設定を構成し、それらにディスクをアタッチします。

1. Azure portal の [**az10408vmss0-lb**] ブレードで、[**サイズ**] をクリックします。

1. 使用可能なサイズの一覧で、**Standard DS1_v2** を選択し、**サイズ変更** をクリックします。

1. [**設定**] セクションで、[**インスタンス**] をクリックし、仮想マシン スケール セットの 2つのインスタンスの横にあるチェック ボックスをオンにし、[**アップグレード**] をクリックします。確認画面が表示されたら、[**Yes**] をクリックします。

1. インスタンスの一覧で、最初のインスタンスを表すエントリをクリックし、スケール セット インスタンス ブレードで **場所**(Azure 仮想マシン スケール セットをデプロイしたターゲット Azure リージョンのゾーンの 1つである必要があります) をメモします。 

1. **az10408vmss0 - Instances**ブレードに戻り、2番目のインスタンスを表すエントリをクリックし、[**Scale Sets Instances**] ブレードで場所をメモします(Azure 仮想マシン スケール セットをデプロイしたターゲット Azure リージョン内の他の 2つのゾーンのいずれかである必要があります)。 

1. [**az10408vmss0 - Instances**] ブレードに戻り、[**スケーリング**] をクリックします。

1. **[az10408vmss0 - スケーリング]** ブレードで、**[カスタム自動スケール]** オプションを選択し、次の設定で自動スケールを構成します (その他は既定値を使用します)。

    | 設定 | 値 |
    | --- |--- |
    | スケール モード | **メトリックに基づくスケーリング** | 

1. [**+ ルールの追加**] リンクをクリックし、[**スケール ルール**] ブレードで次の設定を指定します。その他の設定は既定値のままにします。

    | 設定 | 値 |
    | --- |--- |
    | メトリック ソース | **現在のリソース (az10480vms0)** |
    | 時間集計 | **Maximum** |
    | メトリック名前空間 | **仮想マシン ホスト** |
    | メトリック名 | **合計のネットワーク** |
    | 演算子 | **Greater than** |
    | スケール アクションをトリガーするメトリックしきい値 | **10** |
    | 期間 (分単位) | **1** |
    | 時間グレイン統計 | **Maximum** |
    | 操作 | **数量を増やす** |
    | インスタンス数 | **1** |
    | クール ダウン(分単位) | **5** |

    >**注**: これらの値は、待機時間を延長することなく、できるだけ早く自動スケールをトリガーすることを目的としているため、現実的な設定を表すものではありません。 

1. **[追加]** をクリックし、[**az10408vmss0 - Scaling**] ブレードに戻って、次の設定を指定します (その他は既定値のままにします)。   

    | 設定 | 値 |
    | --- |--- |
    | インスタンスの制限 最小 | **1** |
    | インスタンスの制限の最大値 | **3** |
    | インスタンスの制限 デフォルト | **1** |

1. [**保存**] をクリックします。

1. Azure portal で右上にあるアイコンをクリックして **Azure Cloud Shell** を開きます。

1. **Bash** または **PowerShell** のいずれかを選択するように求められた場合、**PowerShell** を選択します。

    >**注**: **Cloud Shell** を初めて起動し、「 **ストレージがマウントされていません**」というメッセージが表示されたら、このラボで使用しているサブスクリプションを選択し、[**ストレージの作成**] をクリックします。 

1. [クラウド シェル] ウィンドウで、次のコマンドを実行して、Azure 仮想マシン スケール セット  **az10408vms0** の前にあるロード バランサーのパブリック IP アドレスを特定します。

   ```pwsh
   $rgName = 'az104-08-rg02'

   $lbpipName = 'az10408vmss0-ip'

   $pip = (Get-AzPublicIpAddress -ResourceGroupName $rgName -Name $lbpipName).IpAddress
   ```
1. [クラウド シェル] ウィンドウから、Virtual Machine Scale Sets **az10408vms0** のインスタンスでホストされている Web サイトに HTTP 要求を送信する開始ループと無限ループを次のように実行します。

   ```pwsh
   while ($true) { Invoke-WebRequest -Uri "http://$pip" }
   ```

1. Cloud Shell ウインドウを最小化しますが、閉じるのではなく、**az10408vms0 - Instances** に戻り、インスタンス数を監視します。 

    >**注**: 数分待ってから、**[更新]** をクリックする必要がある場合があります。 

1. 3 番目のインスタンスがプロビジョニングされたら、そのブレードに移動して、その**場所**を確認します (このタスクで先ほど指定した最初の 2 つのゾーンとは異なる必要があります)。  

1. [Cloud Shell] ウィンドウを閉じます。 

1. **[az10408vms0]** ブレードで 、**[ストレージ]** をクリックし、 **[+ データ ディスクの追加]** をクリックして、新しいマネージド ディスクを次の設定で添付します (その他は既定値のままにします)。     

    | 設定 | 値 | 
    | --- | --- |
    | LUN | **0** |
    | サイズ | **32** |
    | アカウント タイプ | **Standard HDD** |
    | ホスト キャッシュ | **なし** |

1. 変更を保存して、**az10408vms0** ブレードの **[設定]** セクションで、 **[インスタンス]** をクリックし、仮想マシン スケール セットの 2 つのインスタンスの横にあるチェック ボックスをオンにし、**[アップグレード]** をクリックし、確認が求められたときに、**[Yes]** をクリックします。

    >**注**: 前の手順で接続したディスクは、未加工のディスクです。使用する前に、パーティションを作成し、ファイルシステムを作成し、マウントする必要があります。これを実現するには、Azure 仮想マシンのカスタム スクリプト拡張機能を使用します。まず、既存のカスタム スクリプト拡張を削除する必要があります。

1. **[az10408vms0]** ブレードの **[設定]** セクションで、**[拡張機能]**、**[CustomScriptExtension]**、[**アンインストール**] の順に     

    >**注**: アンインストールが完了するまで待ちます。

1. Azure portal で右上にあるアイコンをクリックして **Azure Cloud Shell** を開きます。

1. **Bash** または **PowerShell** のいずれかを選択するように求められた場合、**PowerShell** を選択します。 

1. [クラウド シェル] ウィンドウのツール バーで、**[ファイルのアップロード/ダウンロード]** アイコンをクリックし、ドロップダウン メニューで、**[アップロード]** をクリックして、ファイル **\\Allfiles\Labs\\08\az104-08-configure_VMSS_disks.ps1** をクラウド シェルのホーム ディレクトリにアップロードします。     

1. [Cloud Shell] ウィンドウから次を実行して、スクリプトのコンテンツを表示します。

   ```pwsh
   Set-Location -Path $HOME

   Get-Content -Path ./az104-08-configure_VMSS_disks.ps1
   ```

    >**Note**: このスクリプトは、接続されたディスクを構成するカスタム スクリプト拡張機能をインストールします。

1. Cloud Shell ウィンドウで、次を実行してスクリプトを削除し、Azure Virtual Machine Scale Sets のディスクを構成します。

   ```pwsh
   ./az104-08-configure_VMSS_disks.ps1
   ```

1. Cloud Shell パネルを閉じます。

1. [**az10408vms0**] ブレードの 設定 セクションで 、[**インスタンス**] をクリックし、仮想マシン スケール セットの 2つのインスタンスの横にあるチェック ボックスをオンにし、[**アップグレード**] をクリックします。確認画面が表示されたら、[**Yes**] をクリックします。

#### リソースのクリーンアップ

   >**注**: 新しく作成された Azure リソースのうち、使用しないリソースは必ず削除してください。使用していないリソースを削除することで、予期しないコストが発生しなくなります。

1. Azure portalの [**Cloud Shell**] ウインドウで、[**PowerShell**] セッションを開始します。

1. 次のコマンドを実行して、このモジュールのラボ全体で作成されたすべてのリソース グループを一覧表示します。

   ```pwsh
   Get-AzResourceGroup -Name 'az104-08*'
   ```

1. 次のコマンドを実行して、このモジュールのラボ全体で作成したすべてのリソース グループを削除します。

   ```pwsh
   Get-AzResourceGroup -Name 'az104-08*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注**: コマンドは非同期に実行されるため (-AsJob パラメーターによって決まります)、同じ PowerShell セッション内ですぐに別の PowerShell コマンドを実行できますが、リソース グループが実際に削除されるまでに数分かかります。

#### レビュー

このラボでは、次の内容を学習しました。

- Azure portal と Azure Resource Manager テンプレートを使用した、ゾーン復元可能な Azure Virtual Machines のデプロイ
- 仮想マシン拡張機能を使用した Azure virtual machine の構成
- Azure virtual Machine の計算とストレージのスケーリング
- Azure portal を使用して、ゾーン復元可能な Azure Virtual Machine Scale Sets のデプロイ
- 仮想マシン拡張機能を使用した Azure Virtual Machine Scale Sets の構成
- Azure Virtual Machine Scale Sets の計算とストレージのスケーリング