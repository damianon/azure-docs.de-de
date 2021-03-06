---
title: Verwalten von ausführenden Azure Automation-Konten
description: In diesem Artikel wird beschrieben, wie Sie mit PowerShell oder über das Portal Ihre ausführenden Konten verwalten.
services: automation
ms.subservice: shared-capabilities
ms.date: 05/24/2019
ms.topic: conceptual
ms.openlocfilehash: 8d7d0baacd5f702e8f435ab440eaf0338a60f4cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79500776"
---
# <a name="manage-azure-automation-run-as-accounts"></a>Verwalten von ausführenden Azure Automation-Konten

In Azure Automation bieten ausführende Konten Authentifizierung für die Verwaltung von Ressourcen in Azure mit Azure-Cmdlets. Beim Erstellen eines ausführenden Kontos wird ein neuer Dienstprinzipalbenutzer in Azure Active Directory (AD) erstellt, und diesem Benutzer wird dann auf Abonnementebene die Rolle „Mitwirkender“ zugewiesen. Für Runbooks, die Hybrid Runbook Worker auf virtuellen Azure-Computern nutzen, können Sie anstelle von ausführenden Konten [verwaltete Identitäten für Azure-Ressourcen](automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) für die Authentifizierung bei Ihren Azure-Ressourcen verwenden.

Der Dienstprinzipal für ein ausführendes Konto hat standardmäßig keine Leseberechtigungen für Azure AD. Wenn Sie Berechtigungen zum Lesen oder Verwalten von Azure AD hinzufügen möchten, müssen Sie dem Dienstprinzipal diese Berechtigung unter **API-Berechtigungen** erteilen. Weitere Informationen finden Sie unter [Hinzufügen von Zugriffsberechtigungen für Web-APIs](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="types-of-run-as-accounts"></a>Typen von ausführenden Konten

Azure Automation verwendet zwei Typen von ausführenden Konten:

* Ausführendes Azure-Konto
* Klassisches ausführendes Azure-Konto

>[!NOTE]
>Azure Cloud Solution Provider-Abonnements (Azure CSP) unterstützen nur das Azure Resource Manager-Modell. Dienste, die nicht auf Azure Resource Manager basieren, sind in diesem Programm nicht verfügbar. Wenn Sie ein CSP-Abonnement verwenden, wird kein klassisches ausführendes Azure-Konto erstellt, sondern das ausführende Azure-Konto. Weitere Informationen zu CSP-Abonnements finden Sie unter [Verfügbare Dienste in CSP-Abonnements](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).

### <a name="run-as-account"></a>Ausführendes Konto

Das ausführende Konto verwaltet [Resource Manager-Bereitstellungsmodell](../azure-resource-manager/management/deployment-models.md)ressourcen. Folgende Aufgaben werden von ihm ausgeführt.

* Erstellt eine Azure AD-Anwendung mit einem selbstsignierten Zertifikat, erstellt ein Dienstprinzipalkonto für die Anwendung in Azure AD und weist die Rolle „Mitwirkender“ für das Konto in Ihrem aktuellen Abonnement zu. Sie können die Zertifikateinstellung in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
  
* Erstellt ein Automation-Zertifikatasset mit dem Namen `AzureRunAsCertificate` im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.
  
* Erstellt ein Automation-Verbindungsasset mit dem Namen `AzureRunAsConnection` im angegebenen Automation-Konto. Die Verbindungsressource enthält die Anwendungs-ID, die Mandanten-ID, die Abonnement-ID und den Zertifikatfingerabdruck.

### <a name="azure-classic-run-as-account"></a>Klassisches ausführendes Azure-Konto

Das klassische ausführende Azure-Konto verwaltet [klassische Bereitstellungsmodell](../azure-resource-manager/management/deployment-models.md)ressourcen. Sie müssen zum Erstellen oder Erneuern dieses Kontotyps Co-Administrator für das Abonnement sein.

Das klassische ausführende Azure-Konto führt die folgenden Aufgaben durch.

  * Erstellen eines Verwaltungszertifikats im Abonnement.

  * Erstellen eines Automation-Zertifikatassets mit dem Namen `AzureClassicRunAsCertificate` im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.

  * Erstellen ein Automation-Verbindungsassets mit dem Namen `AzureClassicRunAsConnection` im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die Abonnement-ID und den Namen des Zertifikatassets.

## <a name="run-as-account-permissions"></a><a name="permissions"></a>Berechtigungen des ausführenden Kontos

In diesem Abschnitt werden Berechtigungen sowohl für normale ausführende Konten als auch für klassische ausführende Konten definiert.

### <a name="permissions-to-configure-run-as-accounts"></a>Berechtigungen zum Konfigurieren von ausführenden Konten

Um ein ausführendes Konto erstellen oder aktualisieren zu können, müssen Sie über bestimmte Rechte und Berechtigungen verfügen. Ein Anwendungsadministrator in Azure Active Directory und ein Besitzer in einem Abonnement können sämtliche Aufgaben erledigen. In der folgenden Tabelle werden die Aufgaben, das entsprechende Cmdlet und die erforderlichen Berechtigungen für Situationen mit Aufgabentrennung aufgeführt:

|Aufgabe|Cmdlet  |Mindestberechtigungen  |Hier legen Sie die Berechtigungen fest|
|---|---------|---------|---|
|Erstellen einer Azure AD-Anwendung|[New-AzADApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication?view=azps-3.5.0)     | Rolle „Anwendungsentwickler“<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Start > Azure AD > App-Registrierungen |
|Hinzufügen von Anmeldeinformationen zur Anwendung|[New-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential?view=azps-3.5.0)     | „Anwendungsadministrator“ oder „Globaler Administrator“<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Start > Azure AD > App-Registrierungen|
|Erstellen und Abrufen eines Azure AD-Dienstprinzipals|[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-3.5.0)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal?view=azps-3.5.0)     | „Anwendungsadministrator“ oder „Globaler Administrator“<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Start > Azure AD > App-Registrierungen|
|Zuweisen oder Erhalten der RBAC-Rolle für den angegebenen Prinzipal|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment?view=azps-3.5.0)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment?view=azps-3.5.0)      | „Benutzerzugriffsadministrator“ oder „Besitzer“, oder muss die folgenden Berechtigungen besitzen:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Abonnement](../role-based-access-control/role-assignments-portal.md)</br>Home > Abonnements > \<Abonnementname\> – Zugriffssteuerung (IAM)|
|Erstellen oder Entfernen eines Automation-Zertifikats|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.5.0)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate?view=azps-3.5.0)     | Mitwirkender der Ressourcengruppe         |Ressourcengruppe des Automation-Kontos|
|Erstellen oder Entfernen einer Automation-Verbindung|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection?view=azps-3.5.0)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection?view=azps-3.5.0)|Mitwirkender der Ressourcengruppe |Ressourcengruppe des Automation-Kontos|

<sup>1</sup> Benutzer Ihres Azure AD-Mandanten, die keine Administratoren sind, können [AD-Anwendungen registrieren](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions), wenn die Option **Benutzer können Anwendungen registrieren** des Azure AD-Mandanten auf der Seite „Benutzereinstellungen“ auf **Ja** festgelegt ist. Wenn die Anwendungsregistrierungseinstellung **Nein** lautet, muss der Benutzer, der diese Aktion ausführt, eine der in dieser Tabelle definierten Rollen besitzen.

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Globaler Administrator“ des Abonnements hinzugefügt werden, werden Sie als Gast hinzugefügt. In diesem Fall wird auf der Seite „Automation-Konto hinzufügen“ die Warnung `You do not have permissions to create…` angezeigt. 

Wenn Sie Mitglied der Active Directory-Instanz des Abonnements sind, wenn die Rolle „Globaler Administrator“ zugewiesen wird, können Sie auch die Warnung `You do not have permissions to create…` auf der Seite „Automation-Konto hinzufügen“ erhalten. In diesem Fall können Sie die Entfernung aus der Active Directory-Instanz des Abonnements anfordern und dann anfordern, erneut hinzugefügt zu werden, damit Sie ein Vollbenutzer in Active Directory werden.

So überprüfen Sie, ob die Situation, die die Fehlermeldung erzeugt, behoben wurde

1. Wählen Sie im Azure-Portal im Bereich „Azure Active Directory“ **Benutzer und Gruppen** aus. 
2. Wählen Sie **Alle Benutzer**.
3. Wählen Sie Ihren Namen aus, und wählen Sie dann **Profil** aus. 
4. Stellen Sie sicher, dass der Wert des Attributs **Benutzertyp** im Profil des Benutzers nicht auf **Gast** festgelegt ist.

### <a name="permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>Berechtigungen zum Konfigurieren von klassischen ausführenden Konten

Zum Konfigurieren oder Erneuern klassischer ausführender Konten benötigen Sie die Rolle „Co-Administrator“ auf Abonnementebene. Weitere Informationen zu klassischen Abonnementberechtigungen finden Sie unter [Administratoren für klassische Azure-Abonnements](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="creating-a-run-as-account-in-azure-portal"></a>Erstellen eines ausführenden Kontos im Azure-Portal

Führen Sie die folgenden Schritte aus, um Ihr Azure Automation-Konto im Azure-Portal zu aktualisieren. Erstellen Sie das ausführende Konto und das klassische ausführende Konto separat. Wenn Sie keine klassischen Ressourcen verwalten müssen, ist es nur erforderlich, das ausführende Azure-Konto zu erstellen.

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Suchen Sie nach **Automation-Konten**, und wählen Sie diese Option aus.
3. Wählen Sie auf der Seite „Automation-Konten“ in der entsprechenden Liste Ihr Automation-Konto aus.
4. Wählen Sie im linken Bereich im Abschnitt „Kontoeinstellungen“ **Ausführende Konten** aus.
5. Abhängig vom benötigten Konto wählen Sie **Ausführendes Azure-Konto** oder **Klassisches ausführendes Azure-Konto** aus. 
6. Verwenden Sie je nach gewünschtem Konto den Bereich **Ausführendes Azure-Konto hinzufügen** oder **Klassisches ausführendes Azure-Konto hinzufügen**. Klicken Sie nach dem Überprüfen der Übersichtsinformationen auf **Erstellen**.
6. Während das ausführende Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen. Es wird auch ein Banner mit dem Hinweis angezeigt, dass das Konto erstellt wird. Der Vorgang kann einige Minuten dauern.

## <a name="creating-a-run-as-account-using-powershell"></a>Erstellen eines ausführenden Kontos mit PowerShell

In der folgenden Liste sind die Anforderungen zum Erstellen eines ausführenden Kontos in PowerShell aufgeführt. Diese Anforderungen gelten für beide Typen von ausführenden Konten.

* Windows 10 oder Windows Server 2016 mit Azure Resource Manager-Modul 3.4.1 und höher. Das PowerShell-Skript unterstützt keine früheren Versionen von Windows.
* Azure PowerShell 1.0 und höher. Informationen zur PowerShell 1.0-Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Ein Automation-Konto, auf das als Wert für die Parameter `AutomationAccountName` und `ApplicationDisplayName` verwiesen wird.
* Berechtigungen, die den unter [Berechtigungen zum Konfigurieren von ausführenden Konten](#permissions) aufgelisteten entsprechen.

Führen Sie die nächsten Schritte aus, um die Werte für `SubscriptionId` und `ResourceGroupName` abzurufen, bei denen es sich um erforderliche Parameter für das PowerShell-Skript handelt.

1. Wählen Sie im Azure-Portal **Automation-Konten** aus.
1. Wählen Sie auf der Seite „Automation-Konten“ Ihr Automation-Konto aus.
1. Wählen Sie im Abschnitt „Einstellungen“ die Option **Eigenschaften** aus.
1. Notieren Sie sich auf der Seite „Eigenschaften“ die Werte für **NAME**, **ABONNEMENT-ID** und **RESSOURCENGRUPPE**. Diese Werte entsprechen den Werten für die PowerShell-Skript Parameter `AutomationAccountName`, `SubscriptionId` bzw. `ResourceGroupName`.

   ![Seite „Eigenschaften“ des Automation-Kontos](media/manage-runas-account/automation-account-properties.png)

### <a name="powershell-script-to-create-a-run-as-account"></a>PowerShell-Skript zum Erstellen eines ausführenden Kontos

In diesem Abschnitt finden Sie ein PowerShell-Skript zum Erstellen eines ausführenden Kontos. Das Skript umfasst Unterstützung für mehrere Konfigurationen.

* Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen Sie ein ausführendes Konto und ein klassisches ausführendes Konto, indem Sie ein Zertifikat nutzen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

Das Skript verwendet mehrere Azure Resource Manager-Cmdlets zum Erstellen von Ressourcen. Informationen zu den Cmdlets und den Berechtigungen, die sie benötigen, finden Sie unter [Berechtigungen zum Konfigurieren von ausführenden Konten](#permissions-to-configure-run-as-accounts).

Speichern Sie das Skript auf Ihrem Computer unter dem Dateinamen **New-RunAsAccount. ps1**.

```powershell
    #Requires -RunAsAdministrator
    Param (
        [Parameter(Mandatory = $true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory = $true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory = $true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory = $true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory = $true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory = $true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory = $false)]
        [string] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory = $false)]
        [ValidateSet("AzureCloud", "AzureUSGovernment")]
        [string]$EnvironmentName = "AzureCloud",

        [Parameter(Mandatory = $false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
    )

    function CreateSelfSignedCertificate([string] $certificateName, [string] $selfSignedCertPlainPassword,
        [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired) -HashAlgorithm SHA256

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
    }

    function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $keyId = (New-Guid).Guid

        # Create an Azure AD application, AD App Credential, AD ServicePrincipal

        # Requires Application Developer Role, but works with Application administrator or GLOBAL ADMIN
        $Application = New-AzADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $keyId)
        # Requires Application administrator or GLOBAL ADMIN
        $ApplicationCredential = New-AzADAppCredential -ApplicationId $Application.ApplicationId -CertValue $keyValue -StartDate $PfxCert.NotBefore -EndDate $PfxCert.NotAfter
        # Requires Application administrator or GLOBAL ADMIN
        $ServicePrincipal = New-AzADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        # Requires User Access Administrator or Owner.
        $NewRole = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6) {
            Sleep -s 10
            New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
        return $Application.ApplicationId.ToString();
    }

    function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName, [string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force
        Remove-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
    }

    function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
    }

    Import-Module AzureRm.Profile
    Import-Module AzureRm.Resources

    $AureRmProfileVersion = (Get-Module AzureRm.Profile).Version
    if (!(($AzureRmProfileVersion.Major -ge 3 -and $AzureRmProfileVersion.Minor -ge 4) -or ($AzureRmProfileVersion.Major -gt 3))) {
        Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
        return
    }

    # To use the new Az modules to create your Run As accounts, please uncomment the following lines and ensure you comment out the previous 8 lines that import the AzureRM modules to avoid any issues. To learn about about using Az modules in your Automation account see https://docs.microsoft.com/azure/automation/az-modules.

    # Import-Module Az.Automation
    # Enable-AzureRmAlias


    Connect-AzAccount -Environment $EnvironmentName
    $Subscription = Get-AzSubscription -SubscriptionId $SubscriptionId

    # Create a Run As account by using a service principal
    $CertifcateAssetName = "AzureRunAsCertificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionTypeName = "AzureServicePrincipal"

    if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
    }
    else {
        $CertificateName = $AutomationAccountName + $CertifcateAssetName
        $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
        $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
        $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
        CreateSelfSignedCertificate $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
    }

    # Create a service principal
    $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
    $ApplicationId = CreateServicePrincipal $PfxCert $ApplicationDisplayName

    # Create the Automation certificate asset
    CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

    # Populate the ConnectionFieldValues
    $SubscriptionInfo = Get-AzSubscription -SubscriptionId $SubscriptionId
    $TenantID = $SubscriptionInfo | Select TenantId -First 1
    $Thumbprint = $PfxCert.Thumbprint
    $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

    # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
    CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

    if ($CreateClassicRunAsAccount) {
        # Create a Run As account by using a service principal
        $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
        $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
        $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
        $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
        "Log in to the Microsoft Azure portal (https://portal.azure.com) and select Subscriptions -> Management Certificates." + [Environment]::NewLine +
        "Then click Upload and upload the .cer format of #CERT#"

        if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
            $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
            $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
            $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        }
        else {
            $ClassicRunAsAccountCertificateName = $AutomationAccountName + $ClassicRunAsAccountCertifcateAssetName
            $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
            $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
            $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
            $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
            CreateSelfSignedCertificate $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName   $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red       $UploadMessage
    }
```

>[!NOTE]
>`Add-AzAccount` und `Add-AzureRMAccount` sind Aliase für [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.5.0). Sie können diese Cmdlets verwenden, oder Sie können Ihre Module in Ihrem Automation-Konto auf die aktuellen Versionen [aktualisieren](automation-update-azure-modules.md). Möglicherweise müssen Sie Ihre Module auch dann aktualisieren, wenn Sie gerade ein neues Automation-Konto erstellt haben.

### <a name="execute-the-powershell-script"></a>Ausführen des PowerShell-Skripts

1. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.
1. Wechseln Sie von der Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der Ihr Skript enthält.
1. Führen Sie das Skript aus, indem Sie die Parameterwerte für die benötigte Konfiguration verwenden.
1. Wenn Sie ein klassisches ausführendes Konto erstellen, laden Sie nach dem Ausführen des Skripts das öffentliche Zertifikat (Dateinamenerweiterung **CER**) in den Verwaltungsspeicher für das Abonnement hoch, in dem das Automation-Konto erstellt wurde.

Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich mit einem Konto an, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.

#### <a name="create-a-run-as-account-by-using-a-self-signed-certificate"></a>Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate"></a>Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true
```

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-an-enterprise-certificate"></a>Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>
```

Wenn Sie ein klassisches ausführendes Konto mit einem öffentlichen Unternehmenszertifikat (**CER**-Datei) erstellt haben, sollten Sie dieses Zertifikat verwenden. Siehe [Hochladen eines Verwaltungs-API-Zertifikats in das Azure-Portal](../azure-api-management-certs.md).

#### <a name="create-a-run-as-account-and-a-classic-run-as-account-by-using-a-self-signed-certificate-in-the-azure-government-cloud"></a>Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

```powershell
    .\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment
```

Falls Sie ein klassisches ausführendes Konto mit einem selbstsignierten öffentlichen Zertifikat (**CER**-Datei) erstellt haben, führt das Skript die Erstellung durch und speichert es im Ordner für temporäre Dateien auf Ihrem Computer. Sie finden es unter dem Benutzerprofil `%USERPROFILE%\AppData\Local\Temp`, das Sie zum Ausführen der PowerShell-Sitzung verwendet haben.

## <a name="deleting-a-run-as-or-classic-run-as-account"></a>Löschen eines ausführenden Kontos oder klassischen ausführenden Kontos

In diesem Abschnitt wird beschrieben, wie Sie ein ausführendes Konto oder klassisches ausführendes Konto löschen. Bei dieser Aktion wird das Automation-Konto beibehalten. Nach dem Löschen des Kontos können Sie es im Azure-Portal neu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

2. Wählen Sie im linken Bereich im Abschnitt „Kontoeinstellungen“ **Ausführende Konten** aus.

3. Wählen Sie auf der Eigenschaftenseite „Ausführende Konten“ entweder das ausführende Konto oder das klassische ausführende Konto aus, das Sie löschen möchten. 

4. Klicken Sie auf der Seite „Eigenschaften“ für das ausgewählte Konto auf **Löschen**.

   ![Löschen des ausführenden Azure-Kontos](media/manage-runas-account/automation-account-delete-runas.png)

5. Während das Konto gelöscht wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

6. Nach dem Löschen des Kontos können Sie das Konto neu erstellen, indem Sie auf der Eigenschaftenseite „Ausführende Konten“ auf die Erstellungsoption für **Ausführendes Azure-Konto** klicken.

   ![Neuerstellen des ausführenden Automation-Kontos](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renewing-a-self-signed-certificate"></a><a name="cert-renewal"></a>Erneuern eines selbstsignierten Zertifikats

Das selbstsignierte Zertifikat, das Sie für das ausführende Konto erstellt haben, läuft ein Jahr nach dem Erstellungsdatum ab. Das Zertifikat muss vor Ablauf Ihres ausführenden Kontos erneuert werden. Sie können es vor dem Ablaufdatum jederzeit erneuern. 

Beim Erneuern des selbstsignierten Zertifikats wird das aktuell gültige Zertifikat beibehalten, um sicherzustellen, dass sich keine negativen Auswirkungen auf Runbooks ergeben, die sich in der Warteschlange befinden oder aktiv ausgeführt werden und die mit dem ausführenden Konto authentifiziert werden. Das Zertifikat bleibt bis zum Ablaufdatum gültig.

>[!NOTE]
>Wenn Sie der Meinung sind, dass das ausführende Konto kompromittiert wurde, können Sie das selbstsignierte Zertifikat löschen und neu erstellen.

>[!NOTE]
>Wenn Sie Ihr ausführendes Konto für die Verwendung eines Zertifikats konfiguriert haben, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde, und Sie verwenden die Option zum Erneuern eines selbstsignierten Zertifikats, wird das Unternehmenszertifikat durch ein selbstsigniertes Zertifikat ersetzt.

Verwenden Sie die folgenden Schritte, um das selbstsignierte Zertifikat zu erstellen.

1. Öffnen Sie das Automation-Konto im Azure-Portal.

1. Wählen Sie im Abschnitt mit den Kontoeinstellungen **Ausführende Konten** aus.

    ![Eigenschaftenbereich des Automation-Kontos](media/manage-runas-account/automation-account-properties-pane.png)

1. Wählen Sie auf der Eigenschaftenseite „Ausführende Konten“ entweder das ausführende Konto oder das klassische ausführende Konto aus, für das Sie das Zertifikat erneuern möchten.

1. Klicken Sie im Bereich „Eigenschaften“ für das ausgewählte Konto auf **Zertifikat erneuern**.

    ![Erneuern des Zertifikats für das ausführende Konto](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Während das Zertifikat erneuert wird, können Sie den Status im Menü unter **Benachrichtigungen** verfolgen.

## <a name="setting-up-automatic-certificate-renewal-with-an-automation-runbook"></a><a name="auto-cert-renewal"></a>Einrichten der automatischen Zertifikaterneuerung mit einem Automation-Runbook

Zum automatischen Erneuern von Zertifikaten können Sie ein Automation-Runbook verwenden. Mit diesem Skript auf [GitHub](https://github.com/ikanni/PowerShellScripts/blob/master/AzureAutomation/RunAsAccount/GrantPermissionToRunAsAccountAADApplication-ToRenewCertificateItself-CreateSchedule.ps1) wird diese Funktion in Ihrem Automation-Konto aktiviert.

>[!NOTE]
>Sie müssen ein „Globaler Administrator“ oder „Unternehmensadministrator“ in Azure AD sein, um das Skript auszuführen.

Dieses Skript erstellt einen wöchentlichen Zeitplan zum Erneuern von Zertifikaten für ausführende Konten. Es fügt Ihrem Automation-Konto ein **Update-AutomationRunAsCredential**-Runbook hinzu. Sie können den Runbookcode auch auf GitHub im Skript [Update-AutomationRunAsCredential.ps1](https://github.com/azureautomation/runbooks/blob/master/Utility/ARM/Update-AutomationRunAsCredential.ps1) anzeigen. Sie können den PowerShell-Code in der Datei verwenden, um Zertifikate nach Bedarf manuell zu erneuern.

Führen Sie die folgenden Schritte aus, um den Erneuerungsprozess sofort zu testen.

1. Bearbeiten Sie das Runbook **Update-AutomationRunAsCredential**, und platzieren Sie vor dem Befehl **Exit(1)** in Zeile 122 ein Kommentarzeichen (#).

   ```powershell
   #Exit(1)
   ```

2. Veröffentlichen Sie das Runbook.
3. Starten Sie das Runbook.
4. Überprüfen Sie die erfolgreiche Erneuerung mit dem folgenden Code:

   ```powershell
   (Get-AzAutomationCertificate -AutomationAccountName TestAA
                                -Name AzureRunAsCertificate
                                -ResourceGroupName TestAutomation).ExpiryTime.DateTime
   ```
    Ausgabe:

   ```Output
   Thursday, November 7, 2019 7:00:00 PM
   ```

5. Bearbeiten Sie nach dem Test das Runbook, und entfernen Sie das Kommentarzeichen, das Sie in Schritt 1 hinzugefügt haben.
6. Veröffentlichen Sie das Runbook.

## <a name="limiting-run-as-account-permissions"></a><a name="limiting-run-as-account-permissions"></a>Einschränken der Berechtigungen für ausführendes Konto

Zum Steuern der Automatisierungsziele für Ressourcen in Azure können Sie das Skript [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8)ausführen. Dieses Skript ändert Ihren vorhandenen Dienstprinzipal für das ausführende Konto, um eine benutzerdefinierte Rollendefinition zu erstellen und zu verwenden. Die Rolle verfügt über Berechtigungen für alle Ressourcen außer [Key Vault](https://docs.microsoft.com/azure/key-vault/).

>[!IMPORTANT]
>Nachdem Sie das Skript **Update-AutomationRunAsAccountRoleAssignments.ps1** ausgeführt haben, funktionieren Runbooks, die unter Verwendung von ausführenden Konten auf Key Vault zugreifen, nicht mehr. Überprüfen Sie vor dem Ausführen des Skripts Runbooks in Ihrem Konto auf Aufrufe von Azure Key Vault. Um den Zugriff auf Key Vault aus Azure Automation-Runbooks zu aktivieren, müssen Sie [das ausführende Konto den Berechtigungen von Key Vault hinzufügen](#add-permissions-to-key-vault).

Wenn Sie noch weiter einschränken müssen, was der Dienstprinzipal des ausführenden Kontos darf, können Sie dem `NotActions`-Element der benutzerdefinierten Rollendefinition weitere Ressourcentypen hinzufügen. Im folgenden Beispiel wird der Zugriff auf `Microsoft.Compute/*` eingeschränkt. Wenn Sie diesen Ressourcentyp zu `NotActions` für die Rollendefinition hinzufügen, kann die Rolle auf keine Compute-Ressource zugreifen. Weitere Informationen zu Rollendefinitionen finden Sie unter [Grundlegendes zu Rollendefinitionen für Azure-Ressourcen](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Sie können bestimmen, ob sich der Dienstprinzipal, der von Ihrem ausführenden Konto verwendet wird, in der Rollendefinition „Mitwirkender“ oder in einer benutzerdefinierten befindet. 

1. Wechseln Sie zu Ihrem Automation-Konto, und wählen Sie im Abschnitt mit den Kontoeinstellungen **Ausführende Konten** aus.
2. Wählen Sie **Ausführendes Azure-Konto** aus. 
3. Wählen Sie **Rolle** aus, um die Rollendefinition zu suchen, die verwendet wird.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

Sie können auch die Rollendefinition bestimmen, die von den ausführenden Konten für mehrere Abonnements oder Automation-Konten verwendet wird. Verwenden Sie hierzu das Skript [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) im PowerShell-Katalog.

### <a name="add-permissions-to-key-vault"></a>Hinzufügen von Berechtigungen zu Key Vault

Sie können Azure Automation gestatten, zu überprüfen, ob Key Vault und Ihr Dienstprinzipal des ausführenden Kontos eine benutzerdefinierte Rollendefinition verwenden. Die Voraussetzungen lauten wie folgt:

* Gewähren von Berechtigungen für Key Vault.
* Festlegen der Zugriffsrichtlinie.

Sie können das Skript [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) im PowerShell-Katalog verwenden, um Ihrem ausführenden Konto Berechtigungen für KeyVault zu erteilen. Weitere Einzelheiten zum Festlegen von Berechtigungen für Key Vault finden Sie unter [Gewähren des Zugriffs auf einen Schlüsseltresor für Anwendungen](../key-vault/key-vault-group-permissions-for-apps.md).

## <a name="resolving-misconfiguration-issues-for-run-as-accounts"></a>Beheben von Fehlkonfigurationsproblemen bei ausführenden Konten

Einige Konfigurationselemente, die für ein ausführendes oder ein klassisches ausführendes Konto erforderlich sind, können eventuell gelöscht oder während der anfänglichen Einrichtung nicht ordnungsgemäß erstellt worden sein. Mögliche Instanzen für Fehlkonfigurationen können sein:

* Zertifikatasset
* Verbindungsasset
* Ausführendes Kontos wurde aus der Rolle „Mitwirkender“ entfernt.
* Dienstprinzipal oder Anwendung in Azure AD

Bei solchen Fehlkonfigurationsinstanzen erkennt das Automation-Konto die Änderungen und zeigt im Eigenschaftenbereich des ausführenden Kontos einen Status von `Incomplete` für das Konto an.

![Konfigurationsstatus „Unvollständig“ für ausführendes Konto](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Wenn Sie das ausführende Konto auswählen, wird im Bereich „Eigenschaften“ die folgende Fehlermeldung angezeigt:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Sie können diese Probleme mit ausführenden Konten schnell beheben, indem Sie das Konto löschen und neu erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/develop/app-objects-and-service-principals.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
