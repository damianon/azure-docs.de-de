---
title: Continuous Integration und Continuous Delivery in Azure Data Factory
description: Erfahren Sie, wie Sie Continuous Integration und Continuous Delivery verwenden, um Data Factory-Pipelines aus einer Umgebung (Entwicklung, Test, Produktion) in eine andere zu verschieben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: 6aad01808ad155b745b614d8de6009386f0d2914
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687958"
---
# <a name="continuous-integration-and-delivery-in-azure-data-factory"></a>Continuous Integration und Continuous Delivery in Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

## <a name="overview"></a>Übersicht

Bei Continuous Integration wird jede Änderung, die an Ihrer Codebasis vorgenommen wird, automatisch und so früh wie möglich getestet. Der Continuous Delivery-Prozess folgt auf das Testen während des Continuous Integration-Prozesses. Änderungen werden dabei in ein Staging- oder Produktionssystem gepusht.

In Azure Data Factory bedeuten Continuous Integration und Continuous Delivery (CI/CD), dass Data Factory-Pipelines von einer Umgebung (Entwicklung, Test, Produktion) in eine andere verschoben werden. Sie können die Integration der Data Factory-Benutzeroberfläche in Azure Resource Manager-Vorlagen verwenden, um CI/CD-Vorgänge durchzuführen.

Auf der Data Factory-Benutzeroberfläche können Sie eine Resource Manager-Vorlage über das Dropdownmenü **ARM-Vorlage** generieren. Wenn Sie **Export ARM template** (ARM-Vorlage exportieren) wählen, generiert das Portal die Resource Manager-Vorlage für die Data Factory und eine Konfigurationsdatei mit Ihren gesamten Verbindungszeichenfolgen und anderen Parametern. Anschließend erstellen Sie eine Konfigurationsdatei für jede Umgebung (Entwicklung, Test, Produktion). Die Hauptdatei mit der Resource Manager-Vorlage bleibt für alle Umgebungen gleich.

Sehen Sie sich dieses Video mit einer neunminütigen Einführung zu diesem Feature und einer Demonstration an:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cicd-lifecycle"></a>CI/CD-Lebenszyklus

Unten ist eine Beispielübersicht des CI/CD-Lebenszyklus in einer Azure Data Factory-Instanz mit Konfiguration per Azure Repos Git angegeben. Weitere Informationen zum Konfigurieren eines Git-Repositorys finden Sie unter [Quellcodeverwaltung in Azure Data Factory](source-control.md).

1.  Eine Data Factory-Entwicklungsinstanz wird erstellt und mit Azure Repos Git konfiguriert. Alle Entwickler sollten über die Berechtigung zum Erstellen von Data Factory-Ressourcen wie Pipelines und Datasets verfügen.

1.  Wenn die Entwickler Änderungen in ihren Featurebranches vornehmen, debuggen sie die Pipelineausführungen mit den neuesten Änderungen. Weitere Informationen zum Debuggen einer Pipelineausführung finden Sie unter [Iteratives Entwickeln und Debuggen mit Azure Data Factory](iterative-development-debugging.md).

1.  Wenn die Entwickler mit ihren Änderungen zufrieden sind, erstellen sie einen Pull Request von ihrem Featurebranch zum Master- oder Kollaborationsbranch, um die Änderungen von anderen Entwicklern prüfen zu lassen.

1.  Nachdem ein Pull Request genehmigt wurde und Änderungen im Masterbranch zusammengeführt wurden, können die Änderungen in der Entwicklungsfactory veröffentlicht werden.

1.  Wenn das Team bereit ist, die Änderungen in der Testfactory und dann in der Produktionsfactory bereitzustellen, exportiert es die Resource Manager-Vorlage aus dem Masterbranch.

1.  Die exportierte Resource Manager-Vorlage wird mit unterschiedlichen Parameterdateien in der Testfactory und der Produktionsfactory bereitgestellt.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Erstellen einer Resource Manager-Vorlage für jede Umgebung

1. Wählen Sie in der Liste **ARM-Vorlage** den Eintrag **Export ARM template** (ARM-Vorlage exportieren) aus, um die Resource Manager-Vorlage für Ihre Data Factory in die Entwicklungsumgebung zu exportieren.

   ![Exportieren einer Resource Manager-Vorlage](media/continuous-integration-deployment/continuous-integration-image1.png)

1. Wählen Sie in den Test- und Produktionsfactorys den Eintrag **Import ARM template** (ARM-Vorlage importieren) aus. Mit dieser Aktion gelangen Sie zum Azure-Portal, in dem sie die exportierte Vorlage importieren können. Wählen Sie **Eigene Vorlage im Editor erstellen** aus, um den Editor für Resource Manager-Vorlagen zu öffnen.

   ![Erstellen einer eigenen Vorlage](media/continuous-integration-deployment/custom-deployment-build-your-own-template.png) 

1. Wählen Sie die Option **Datei laden** und anschließend die generierte Resource Manager-Vorlage aus. Dies ist die Datei **arm_template.json** in der ZIP-Datei, die Sie in Schritt 1 exportiert haben.

   ![Bearbeiten der Vorlage](media/continuous-integration-deployment/custom-deployment-edit-template.png)

1. Geben Sie im Abschnitt mit den Einstellungen die Konfigurationswerte ein, z. B. die Anmeldeinformationen für verknüpfte Dienste. Wählen Sie anschließend die Option **Kaufen** aus, um die Resource Manager-Vorlage bereitzustellen.

   ![Einstellungsabschnitt](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Verbindungszeichenfolgen

Informationen zum Konfigurieren von Verbindungszeichenfolgen finden Sie im Artikel zum Connector. Informationen zu Azure SQL-Datenbank finden Sie z.B. unter [Kopieren von Daten nach und aus Azure SQL-Datenbank mithilfe von Azure Data Factory](connector-azure-sql-database.md). Zum Überprüfen einer Verbindungszeichenfolge können Sie die Codeansicht für die Ressource auf der Data Factory-Benutzeroberfläche öffnen. In der Codeansicht werden das Kennwort und der Kontoschlüssel der Verbindungszeichenfolge nicht angezeigt. Wählen Sie hier das hervorgehobene Symbol aus, um die Codeansicht zu öffnen:

![Öffnen der Codeansicht zum Anzeigen der Verbindungszeichenfolge](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-by-using-azure-pipelines-releases"></a>Automatisieren von Continuous Integration mit Azure Pipelines-Releases

Im Folgenden finden Sie eine Anleitung zum Einrichten eines Azure Pipelines-Release, mit dem die Bereitstellung einer Data Factory für mehrere Umgebungen automatisiert wird.

![Diagramm von Continuous Integration mit Azure Pipelines](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Requirements (Anforderungen)

-   Ein Azure-Abonnement, das mit Visual Studio Team Foundation Server oder Azure Repos verknüpft ist und für das der [Azure Resource Manager-Dienstendpunkt](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-resource-manager) verwendet wird

-   Eine Data Factory-Instanz mit konfigurierter Azure Repos Git-Integration

-   Ein [Azure-Schlüsseltresor](https://azure.microsoft.com/services/key-vault/) mit den Geheimnissen für jede Umgebung

### <a name="set-up-an-azure-pipelines-release"></a>Einrichten eines Azure-Pipelines-Release

1.  Öffnen Sie in [Azure DevOps](https://dev.azure.com/) das Projekt, das für Ihre Data Factory konfiguriert ist.

1.  Wählen Sie links auf der Seite die Option **Pipelines** und dann **Releases** aus.

    ![Auswählen von „Pipelines“ > „Releases“](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Wählen Sie **Neue Pipeline** oder – falls Pipelines vorhanden sind – die Option **Neu** und dann **Neue Releasepipeline** aus.

1.  Wählen Sie die Vorlage **Leere Stufe** aus.

    ![Auswählen von „Leere Stufe“](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  Geben Sie im Feld **Name der Stufe** den Namen der Umgebung ein.

1.  Wählen Sie **Artefakt hinzufügen** und dann dasselbe Repository aus, das für die Data Factory konfiguriert wurde. Wählen Sie **adf_publish** als **Standardbranch** aus. Wählen Sie unter **Standardversion** die Option **Letztes Element aus Standardbranch** aus.

    ![Hinzufügen eines Artefakts](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Fügen Sie eine Azure Resource Manager-Bereitstellungsaufgabe hinzu:

    a.  Wählen Sie in der Stufenansicht die Option **Stufenaufgaben anzeigen** aus.

    ![Stufenansicht](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Erstellen Sie eine neue Aufgabe. Suchen Sie nach **Bereitstellung einer Azure-Ressourcengruppe**, und wählen Sie anschließend die Option **Hinzufügen** aus.

    c.  Wählen Sie unter der Aufgabe „Bereitstellung“ das Abonnement, die Ressourcengruppe und den Speicherort für das Data Factory-Ziel aus. Geben Sie die Anmeldeinformationen an, falls dies erforderlich ist.

    d.  Wählen Sie in der Liste **Aktion** den Eintrag **Create or update resource group** (Ressourcengruppe erstellen oder aktualisieren) aus.

    e.  Wählen Sie neben dem Feld **Vorlage** die Schaltfläche mit den Auslassungszeichen ( **…** ) aus. Suchen Sie nach der Azure Resource Manager-Vorlage, die Sie erstellt haben, indem Sie im Abschnitt [Erstellen einer Resource Manager-Vorlage für jede Umgebung](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment) dieses Artikels die Option **Import ARM Template** (ARM-Vorlage importieren) verwendet haben. Suchen Sie im Ordner <FactoryName> des Branch „adf_publish“ nach dieser Datei.

    f.  Wählen Sie **…** neben dem Feld **Vorlagenparameter** aus, um die Parameterdatei auszuwählen. Die Auswahl der Datei richtet sich danach, ob Sie eine Kopie erstellt haben oder die Standarddatei „ARMTemplateParametersForFactory.json“ verwenden.

    g.  Wählen Sie **…** neben dem Feld **Vorlagenparameter überschreiben** aus, und geben Sie die Informationen für das Data Factory-Ziel ein. Geben Sie für Anmeldeinformationen aus Azure Key Vault den Namen des Geheimnisses in doppelten Anführungszeichen ein. Geben Sie als Wert hier beispielsweise Folgendes ein, wenn der Name des Geheimnisses „cred1“ lautet: **"$(cred1)"** .

    h. Wählen Sie unter **Bereitstellungsmodus** die Option **Inkrementell** aus.

    > [!WARNING]
    > Bei Auswahl von **Vollständig** als **Bereitstellungsmodus** werden vorhandene Ressourcen eventuell gelöscht. Dies gilt einschließlich aller Ressourcen in der Zielressourcengruppe, die nicht in der Resource Manager-Vorlage definiert sind.

    ![Data Factory-Bereitstellung in der Produktion](media/continuous-integration-deployment/continuous-integration-image9.png)

1.  Speichern Sie die Releasepipeline.

1. Wählen Sie zum Auslösen eines Release die Option **Release erstellen** aus.

   ![Auswählen von „Release erstellen“](media/continuous-integration-deployment/continuous-integration-image10.png)

> [!IMPORTANT]
> In CI/CD-Szenarien muss der Typ der Integration Runtime (IR) in unterschiedlichen Umgebungen identisch sein. Wenn sich in Ihrer Entwicklungsumgebung beispielsweise eine selbstgehostete IR befindet, muss diese IR auch in anderen Umgebungen, z. B. Test- und Produktionsumgebungen, den Typ „Selbstgehostet“ aufweisen. Wenn Sie Integration Runtimes in mehreren Stufen freigeben, müssen Sie die Integration Runtimes in allen Umgebungen, z. B. Entwicklungs-, Test- und Produktionsumgebungen, als „Verknüpft selbstgehostet“ konfigurieren.

### <a name="get-secrets-from-azure-key-vault"></a>Abrufen von Geheimnissen aus Azure Key Vault

Wenn Sie Geheimnisse in einer Azure Resource Manager-Vorlage übergeben möchten, empfehlen wir Ihnen die Verwendung von Azure Key Vault mit dem Azure Pipelines-Release.

Es gibt zwei Möglichkeiten, um Geheimnisse zu verarbeiten:

1.  Fügen Sie die Geheimnisse der Parameterdatei hinzu. Weitere Informationen finden Sie unter [Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung](../azure-resource-manager/templates/key-vault-parameter.md).

    Erstellen Sie eine Kopie der Parameterdatei, die in den Branch für die Veröffentlichung hochgeladen wird. Legen Sie die Werte der Parameter, die Sie aus Key Vault abrufen möchten, in diesem Format fest:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    Bei dieser Methode wird das Geheimnis per Pullvorgang automatisch aus dem Schlüsseltresor abgerufen.

    Die Parameterdatei muss sich auch im Branch für die Veröffentlichung befinden.

1. Fügen Sie vor der im vorherigen Abschnitt beschriebenen Aufgabe für die Azure Resource Manager-Bereitstellung eine [Azure Key Vault-Aufgabe](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) hinzu:

    1.  Erstellen Sie auf der Registerkarte **Tasks** eine neue Aufgabe. Suchen Sie nach **Azure Key Vault**, und fügen Sie die Komponente hinzu.

    1.  Wählen Sie in der Key Vault-Aufgabe das Abonnement aus, unter dem Sie den Schlüsseltresor erstellt haben. Geben Sie bei Bedarf Anmeldeinformationen an, und wählen Sie anschließend den Schlüsseltresor aus.

    ![Hinzufügen einer Key Vault-Aufgabe](media/continuous-integration-deployment/continuous-integration-image8.png)

#### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Gewähren von Berechtigungen für den Azure Pipelines-Agent

Unter Umständen tritt bei der Azure Key Vault-Aufgabe ein Fehler vom Typ „Zugriff verweigert“ auf, wenn nicht die richtigen Berechtigungen festgelegt wurden. Laden Sie die Protokolle für das Release herunter, und suchen Sie nach der PS1-Datei, die den Befehl zum Erteilen von Berechtigungen für den Azure Pipelines-Agent enthält. Sie können den Befehl direkt ausführen. Alternativ können Sie die Prinzipal-ID aus der Datei kopieren und die Zugriffsrichtlinie manuell im Azure-Portal hinzufügen. Die Berechtigungen `Get` und `List` sind mindestens erforderlich.

### <a name="update-active-triggers"></a>Aktualisieren von aktiven Triggern

Für die Bereitstellung kann ein Fehler auftreten, wenn Sie versuchen, aktive Trigger zu aktualisieren. Zum Aktualisieren von aktiven Triggern müssen Sie sie manuell beenden und nach der Bereitstellung wieder starten. Dies ist über eine Azure PowerShell-Aufgabe möglich:

1.  Fügen Sie auf der Registerkarte **Tasks** des Release eine **Azure PowerShell**-Aufgabe hinzu. Wählen Sie die Aufgabenversion „4.*“ aus. 

1.  Wählen Sie das Abonnement aus, in dem sich Ihre Factory befindet.

1.  Wählen Sie **Pfad zur Skriptdatei** als Skripttyp aus. Hierfür müssen Sie Ihr PowerShell-Skript in Ihrem Repository speichern. Das folgende PowerShell-Skript kann zum Beenden von Triggern verwendet werden:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

Sie können ähnliche Schritte ausführen (mit der Funktion `Start-AzDataFactoryV2Trigger`), um die Trigger nach der Bereitstellung neu zu starten.

### <a name="sample-pre--and-post-deployment-script"></a>Beispiel für Skript vor und nach der Bereitstellung

Das folgende Beispielskript kann verwendet werden, um Trigger vor der Bereitstellung zu beenden und anschließend neu starten. Außerdem enthält das Skript den Code zum Löschen von Ressourcen, die entfernt wurden. Speichern Sie das Skript in einem Azure DevOps-Git-Repository, und verweisen Sie es über eine Azure PowerShell Aufgabe unter Verwendung von „Version 4.*“.

Wenn Sie ein Skript vor der Bereitstellung ausführen, müssen Sie im Feld **Skriptargumente** eine Variation der folgenden Parameter angeben.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $true -deleteDeployment $false`


Wenn Sie ein Skript nach der Bereitstellung ausführen, müssen Sie im Feld **Skriptargumente** eine Variation der folgenden Parameter angeben.

`-armTemplate "$(System.DefaultWorkingDirectory)/<your-arm-template-location>" -ResourceGroupName <your-resource-group-name> -DataFactoryName <your-data-factory-name>  -predeployment $false -deleteDeployment $true`

![Azure PowerShell-Aufgabe](media/continuous-integration-deployment/continuous-integration-image11.png)

Hier ist das Skript, das für vor und nach der Bereitstellung verwendet werden kann. Es berücksichtigt gelöschte Ressourcen und Ressourcenverweise.

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

function getPipelineDependencies {
    param([System.Object] $activity)
    if ($activity.Pipeline) {
        return @($activity.Pipeline.ReferenceName)
    } elseif ($activity.Activities) {
        $result = @()
        $activity.Activities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        return $result
    } elseif ($activity.ifFalseActivities -or $activity.ifTrueActivities) {
        $result = @()
        $activity.ifFalseActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        $activity.ifTrueActivities | Where-Object {$_ -ne $null} | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        return $result
    } elseif ($activity.defaultActivities) {
        $result = @()
        $activity.defaultActivities | ForEach-Object{ $result += getPipelineDependencies -activity $_ }
        if ($activity.cases) {
            $activity.cases | ForEach-Object{ $_.activities } | ForEach-Object{$result += getPipelineDependencies -activity $_ }
        }
        return $result
    } else {
        return @()
    }
}

function pipelineSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]$pipeline,
    [Hashtable] $pipelineNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$pipeline.Name] -eq $true) {
        return;
    }
    $visited[$pipeline.Name] = $true;
    $pipeline.Activities | ForEach-Object{ getPipelineDependencies -activity $_ -pipelineNameResourceDict $pipelineNameResourceDict}  | ForEach-Object{
        pipelineSortUtil -pipeline $pipelineNameResourceDict[$_] -pipelineNameResourceDict $pipelineNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($pipeline)

}

function Get-SortedPipelines {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $pipelines = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $ppDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $pipelines | ForEach-Object{ $ppDict[$_.Name] = $_ }
    $pipelines | ForEach-Object{ pipelineSortUtil -pipeline $_ -pipelineNameResourceDict $ppDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSPipeline]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function triggerSortUtil {
    param([Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]$trigger,
    [Hashtable] $triggerNameResourceDict,
    [Hashtable] $visited,
    [System.Collections.Stack] $sortedList)
    if ($visited[$trigger.Name] -eq $true) {
        return;
    }
    $visited[$trigger.Name] = $true;
    $trigger.Properties.DependsOn | Where-Object {$_ -and $_.ReferenceTrigger} | ForEach-Object{
        triggerSortUtil -trigger $triggerNameResourceDict[$_.ReferenceTrigger.ReferenceName] -triggerNameResourceDict $triggerNameResourceDict -visited $visited -sortedList $sortedList
    }
    $sortedList.Push($trigger)
}

function Get-SortedTriggers {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $triggers = Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $triggerDict = @{}
    $visited = @{}
    $stack = new-object System.Collections.Stack
    $triggers | ForEach-Object{ $triggerDict[$_.Name] = $_ }
    $triggers | ForEach-Object{ triggerSortUtil -trigger $_ -triggerNameResourceDict $triggerDict -visited $visited -sortedList $stack }
    $sortedList = new-object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSTrigger]
    
    while ($stack.Count -gt 0) {
        $sortedList.Add($stack.Pop())
    }
    $sortedList
}

function Get-SortedLinkedServices {
    param(
        [string] $DataFactoryName,
        [string] $ResourceGroupName
    )
    $linkedServices = Get-AzDataFactoryV2LinkedService -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
    $LinkedServiceHasDependencies = @('HDInsightLinkedService', 'HDInsightOnDemandLinkedService', 'AzureBatchLinkedService')
    $Akv = 'AzureKeyVaultLinkedService'
    $HighOrderList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $RegularList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]
    $AkvList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]

    $linkedServices | ForEach-Object {
        if ($_.Properties.GetType().Name -in $LinkedServiceHasDependencies) {
            $HighOrderList.Add($_)
        }
        elseif ($_.Properties.GetType().Name -eq $Akv) {
            $AkvList.Add($_)
        }
        else {
            $RegularList.Add($_)
        }
    }

    $SortedList = New-Object Collections.Generic.List[Microsoft.Azure.Commands.DataFactoryV2.Models.PSLinkedService]($HighOrderList.Count + $RegularList.Count + $AkvList.Count)
    $SortedList.AddRange($HighOrderList)
    $SortedList.AddRange($RegularList)
    $SortedList.AddRange($AkvList)
    $SortedList
}

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-SortedTriggers -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Remove-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Disabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-SortedPipelines -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #dataflows
    $dataflowsADF = Get-AzDataFactoryV2DataFlow -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $dataflowsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/dataflows" }
    $dataflowsNames = $dataflowsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataflow = $dataflowsADF | Where-Object { $dataflowsNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-SortedLinkedServices -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting dataflows"
    $deleteddataflow | ForEach-Object { 
        Write-Host "Deleting dataflow " $_.Name
        Remove-AzDataFactoryV2DataFlow -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start active triggers - after cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Add-AzDataFactoryV2TriggerSubscription -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force
    $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    while ($status.Status -ne "Enabled"){
            Start-Sleep -s 15
            $status = Get-AzDataFactoryV2TriggerSubscriptionStatus -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_
    }
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Verwenden benutzerdefinierter Parameter mit der Resource Manager-Vorlage

Im Git-Modus können Sie die Standardeigenschaften in Ihrer Resource Manager-Vorlage überschreiben, um in der Vorlage parametrisierte Eigenschaften und hartcodierte Eigenschaften festzulegen. Die Überschreibung der standardmäßigen Parametrisierungsvorlage kann beispielsweise in folgenden Szenarien erforderlich sein:

* Sie verwenden automatisierte CI/CD und möchten einige Eigenschaften während der Resource Manager-Bereitstellung ändern, die Eigenschaften sind standardmäßig aber nicht parametrisiert.
* Die Resource Manager-Standardvorlage ist aufgrund der Größe Ihrer Factory ungültig, da sie mehr als die maximal zulässige Parameteranzahl (256) enthält.

In diesen Fällen können Sie die Standardparametrisierungsvorlage wie folgt außer Kraft setzen: Erstellen Sie in dem als Stammordner für die Data Factory-Git-Integration angegebenen Ordner eine Datei mit dem Namen **arm-template-parameters-definition.json**. Sie müssen exakt diesen Dateinamen verwenden. Data Factory liest diese Datei aus dem Branch aus, in dem Sie sich im Azure Data Factory-Portal gerade befinden (nicht nur aus dem Kollaborationsbranch). Sie können die Datei in einem privaten Branch erstellen oder bearbeiten und Ihre Änderungen testen, indem Sie auf der Benutzeroberfläche die Option **Export ARM Template** (ARM-Vorlage exportieren) auswählen. Anschließend können Sie die Datei mit dem Kollaborationsbranch zusammenführen. Sollte keine Datei gefunden werden, wird die Standardvorlage verwendet.

> [!NOTE]
> Durch eine benutzerdefinierte Parametrisierungsvorlage wird das ARM-Vorlagenparameterlimit von 256 nicht geändert. Sie können in ihr die parametrisierten Eigenschaften auswählen und ihre Anzahl verringern.

### <a name="syntax-of-a-custom-parameters-file"></a>Syntax einer benutzerdefinierten Parameterdatei

Hier sind einige Richtlinien angegeben, deren Einhaltung wir Ihnen empfehlen, wenn Sie die benutzerdefinierte Parameterdatei erstellen. Die Datei enthält jeweils einen eigenen Abschnitt für die Entitätstypen: „trigger“, „pipeline“, „linked service“, „dataset“, „integration runtime“ usw.
* Geben Sie den Eigenschaftenpfad unter dem relevanten Entitätstyp ein.
* Durch das Festlegen eines Eigenschaftennamens auf `*` geben Sie an, dass alle untergeordneten Eigenschaften parametrisiert werden sollen (nicht rekursiv, sondern nur bis zur ersten Ebene). Sie können auch Ausnahmen für diese Konfiguration angeben.
* Wenn Sie den Wert einer Eigenschaft als Zeichenfolge festlegen, geben Sie damit an, dass die Eigenschaft parametrisiert werden soll. Verwenden Sie das Format `<action>:<name>:<stype>`.
   *  `<action>` kann für eines dieser Zeichen durchgeführt werden:
      * `=` bedeutet, dass der aktuelle Wert als Standardwert für den Parameter beibehalten werden soll.
      * `-` bedeutet, dass der Standardwert für den Parameter nicht beibehalten werden soll.
      * `|` ist ein Sonderfall für Geheimnisse aus Azure Key Vault für Verbindungszeichenfolgen oder Schlüssel.
   * `<name>` ist der Name des Parameters. Wenn dieser Wert leer ist, wird der Name der Eigenschaft verwendet. Beginnt der Wert mit dem Zeichen `-`, wird der Name gekürzt. `AzureStorage1_properties_typeProperties_connectionString` wird beispielsweise in `AzureStorage1_connectionString` gekürzt.
   * `<stype>` ist der Typ des Parameters. Wenn `<stype>` leer ist, wird standardmäßig der Typ `string` verwendet. Unterstützte Werte: `string`, `bool`, `number`, `object` und `securestring`.
* Wenn Sie ein Array in der Definitionsdatei angeben, bedeutet dies, dass die entsprechende Eigenschaft in der Vorlage ein Array ist. Data Factory durchläuft alle Objekte im Array anhand der Definition, die im Integration Runtime-Objekt des Arrays angegeben ist. Das zweite Objekt (eine Zeichenfolge) wird zum Namen der Eigenschaft, der bei jeder Iteration als Name für den Parameter verwendet wird.
* Eine Definition kann nicht spezifisch für eine Ressourceninstanz sein. Jede Definition gilt für alle Ressourcen dieses Typs.
* Standardmäßig werden alle sicheren Zeichenfolgen parametrisiert, z. B. Key Vault-Geheimnisse, Verbindungszeichenfolgen, Schlüssel und Token.
 
### <a name="sample-parameterization-template"></a>Exemplarische Parametrisierungsvorlage

Hier ist ein Beispiel dafür angegeben, wie eine Parametrisierungsvorlage aussehen kann:

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```
Im Folgenden wird das Erstellen der obigen Vorlage mit einer Aufschlüsselung nach Ressourcentypen beschrieben.

#### <a name="pipelines"></a>Pipelines
    
* Jede Eigenschaft im Pfad `activities/typeProperties/waitTimeInSeconds` wird parametrisiert. Jede Aktivität in einer Pipeline, die eine Eigenschaft auf Codeebene mit dem Namen `waitTimeInSeconds` enthält (z. B. die Aktivität `Wait`), wird als Zahl mit einem Standardnamen parametrisiert. Die Aktivität verfügt jedoch nicht über einen Standardwert in der Resource Manager-Vorlage. Hierbei handelt es sich um eine erforderliche Eingabe bei der Resource Manager-Bereitstellung.
* Analog dazu wird eine Eigenschaft namens `headers` (etwa in einer Aktivität vom Typ `Web`) mit dem Typ `object` (JObject) parametrisiert. Sie verfügt über einen Standardwert (gleicher Wert wie für die Quellfactory).

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Alle Eigenschaften unter dem Pfad `typeProperties` werden mit ihren jeweiligen Standardwerten parametrisiert. Beispielsweise sind unter Eigenschaften vom Typ `IntegrationRuntimes` zwei Eigenschaften vorhanden: `computeProperties` und `ssisProperties`. Beide Eigenschaftentypen werden mit ihren jeweiligen Standardwerten und -typen (Objekt) erstellt.

#### <a name="triggers"></a>Trigger

* Unter `typeProperties` werden zwei Eigenschaften parametrisiert. Die erste ist `maxConcurrency`. Diese Eigenschaft besitzt einen Standardwert und ist vom Typ `string`. Der Standardparametername lautet `<entityName>_properties_typeProperties_maxConcurrency`.
* Die Eigenschaft `recurrence` wird ebenfalls parametrisiert. Darunter werden alle Eigenschaften auf dieser Ebene gemäß Angabe als Zeichenfolgen mit Standardwerten und Parameternamen parametrisiert. Eine Ausnahme ist die `interval`-Eigenschaft, für die beim Parametrisieren der Typ `number` verwendet wird. An den Parameternamen ist das Suffix `<entityName>_properties_typeProperties_recurrence_triggerSuffix` angehängt. Analog dazu ist die Eigenschaft `freq` eine Zeichenfolge und wird als Zeichenfolge parametrisiert. Die Eigenschaft `freq` wird jedoch ohne Standardwert parametrisiert. Der Name wird verkürzt und mit einem Suffix versehen. Beispiel: `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Verknüpfte Dienste sind ein Sonderfall. Da verknüpfte Dienste und Datasets eine breite Palette von Typen umfassen, können Sie eine typspezifische Anpassung vornehmen. In diesem Beispiel wird für alle verknüpften Dienste vom Typ `AzureDataLakeStore` eine bestimmte Vorlage angewendet. Für alle anderen Dienste wird eine andere Vorlage angewendet (per `*`).
* Die `connectionString`-Eigenschaft wird als `securestring`-Wert parametrisiert. Sie hat keinen Standardwert. Sie weist einen verkürzten Parameternamen auf, an den das Suffix `connectionString` angehängt ist.
* Die `secretAccessKey`-Eigenschaft ist eine Eigenschaft vom Typ `AzureKeyVaultSecret` (beispielsweise in einem verknüpften Amazon S3-Dienst). Sie wird automatisch als Azure Key Vault-Geheimnis parametrisiert und aus dem konfigurierten Schlüsseltresor abgerufen. Auch der Schlüsseltresor kann parametrisiert werden.

#### <a name="datasets"></a>Datasets

* Für Datasets steht zwar eine typspezifische Anpassung zur Verfügung, aber Sie können die Konfiguration durchführen, ohne dass eine explizite Konfiguration auf der Ebene \* vorhanden sein muss. Im vorherigen Beispiel werden alle Dataseteigenschaften unter `typeProperties` parametrisiert.

### <a name="default-parameterization-template"></a>Standardvorlage für die Parametrisierung

Nachfolgend ist die aktuelle Standardvorlage für die Parametrisierung dargestellt. Falls Sie nur wenige Parameter hinzufügen müssen, ist die direkte Bearbeitung dieser Vorlage ggf. eine gute Idee, weil die vorhandene Parametrisierungsstruktur nicht verloren geht.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "poolName": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

Im folgenden Beispiel wird das Hinzufügen eines einzelnen Werts zur Standardvorlage für die Parametrisierung veranschaulicht. Der Parameterdatei soll lediglich die ID eines vorhandenen interaktiven Azure Databricks-Clusters für einen verknüpften Databricks-Dienst hinzugefügt werden. Beachten Sie, dass diese Datei mit der vorherigen Datei nahezu identisch ist. Die einzige Ausnahme ist, dass unter dem Eigenschaftenfeld von `Microsoft.DataFactory/factories/linkedServices` das `existingClusterId`-Element hinzugefügt wurde.

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/dataflows": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
            "poolName": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

## <a name="linked-resource-manager-templates"></a>Verknüpfte Resource Manager-Vorlagen

Wenn Sie CI/CD für Ihre Data Factorys eingerichtet haben, werden ggf. die Grenzwerte für Azure Resource Manager-Vorlagen überschritten, wenn die Größe Ihrer Factory zunimmt. Ein Beispiel für eine Beschränkung ist die maximale Anzahl von Ressourcen in einer Resource Manager-Vorlage. Um die Verwendung umfangreicher Factorys zu ermöglichen, während die vollständige Resource Manager-Vorlage für eine Factory generiert wird, werden von Data Factory jetzt verknüpfte Resource Manager-Vorlagen generiert. Mit dieser Funktion wird die gesamte Nutzlast einer Factory auf mehrere Dateien aufgeteilt, damit für Sie keine Einschränkungen aufgrund von Grenzwerten bestehen.

Wenn Sie Git konfiguriert haben, werden die verknüpften Vorlagen zusammen mit den vollständigen Resource Manager-Vorlagen im Branch „adf_publish“ in einem neuen Ordner mit dem Namen „linkedTemplates“ generiert und gespeichert:

![Ordner für verknüpfte Resource Manager-Vorlagen](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Verknüpfte Resource Manager-Vorlagen verfügen normalerweise über eine Mastervorlage und eine Reihe von untergeordneten Vorlagen, die mit der Mastervorlage verknüpft sind. Die übergeordnete Vorlage hat den Namen „ArmTemplate_master.json“, und für untergeordnete Vorlagen werden Namen nach dem Muster „ArmTemplate_0.json“, „ArmTemplate_1.json“ usw. verwendet. 

Um anstelle der vollständigen Resource Manager-Vorlage verknüpfte Vorlagen zu verwenden, sollten Sie den CI/CD-Task so aktualisieren, dass dieser nicht auf „ArmTemplateForFactory.json“ (vollständige Resource Manager-Vorlage), sondern auf „ArmTemplate_master.json“ verweist. Für Resource Manager ist es auch erforderlich, dass Sie die verknüpften Vorlagen in ein Speicherkonto hochladen, damit von Azure während der Bereitstellung darauf zugegriffen werden kann. Weitere Informationen finden Sie unter [Bereitstellen von verknüpften Resource Manager-Vorlagen per VSTS](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/).

Denken Sie daran, die Data Factory-Skripts vor und nach dem Bereitstellungstask zu Ihrer CI/CD-Pipeline hinzuzufügen.

Wenn Sie Git nicht konfiguriert haben, können Sie über die Option **Export ARM Template** (ARM-Vorlage exportieren) in der Liste **ARM-Vorlage** auf die verknüpften Vorlagen zugreifen.

## <a name="hotfix-production-branch"></a>Hotfix für den Produktionsbranch

Wenn Sie eine Factory in der Produktionsumgebung bereitstellen und ein sofort zu behebender Fehler vorhanden ist, Sie den aktuellen Kollaborationsbranch aber nicht bereitstellen können, müssen Sie unter Umständen einen Hotfix verwenden. Dieser Ansatz wird als Quick Fix Engineering oder QFE bezeichnet.

1.    Navigieren Sie in Azure DevOps zu dem Release, das für die Produktion bereitgestellt wurde. Suchen Sie nach dem letzten bereitgestellten Commit.

2.    Rufen Sie aus der Commitmeldung die Commit-ID des Kollaborationsbranch ab.

3.    Erstellen Sie aus diesem Commit einen neuen Hotfixbranch.

4.    Navigieren Sie zur Azure Data Factory-Benutzeroberfläche und dort zum neuen Hotfixbranch.

5.    Beheben Sie den Fehler auf der Azure Data Factory-Benutzeroberfläche. Testen Sie die Änderungen.

6.    Wählen Sie nach der Verifizierung der Fehlerbehebung die Option **Export ARM Template** (ARM-Vorlage exportieren) aus, um die Resource Manager-Vorlage für den Hotfix abzurufen.

7.    Checken Sie diesen Build manuell im Branch „adf_publish“ ein.

8.    Wenn Sie die Releasepipeline so konfiguriert haben, dass sie bei Check-Ins in „adf_publish“ automatisch ausgelöst wird, wird automatisch ein neues Release gestartet. Reihen Sie andernfalls manuell ein Release in die Warteschlange ein.

9.    Stellen Sie das Hotfixrelease für die Test- und Produktionsfactorys bereit. Dieses Release enthält die vorherige Nutzlast der Produktionsumgebung sowie die Korrektur, die Sie in Schritt 5 vorgenommen haben.

10.    Fügen Sie dem Entwicklungsbranch die Änderungen aus dem Hotfix hinzu, damit spätere Releases nicht den gleichen Fehler enthalten.

## <a name="best-practices-for-cicd"></a>Bewährte Methoden für CI/CD

Wenn Sie die Git-Integration mit Ihrer Data Factory verwenden und über eine CI/CD-Pipeline verfügen, die Ihre Änderungen aus der Entwicklungs- in die Test- und dann in die Produktionsumgebung verschiebt, empfehlen wir Ihnen diese bewährten Methoden:

-   **Git-Integration**. Sie müssen lediglich Ihre Data Factory für die Entwicklung mit Git-Integration konfigurieren. Änderungen an den Test- und Produktionsumgebungen werden über CI/CD bereitgestellt, und eine Git-Integration wird hierfür nicht benötigt.

-   **Data Factory-CI/CD-Skript**. Bevor Sie den Schritt für die Resource Manager-Bereitstellung für CI/CD ausführen, müssen Sie bestimmte Aufgaben erledigen, z. B. das Beenden und erneute Starten von Triggern und das Durchführen einer Bereinigung. Wir empfehlen Ihnen, vor und nach der Bereitstellung PowerShell-Skripts zu verwenden. Weitere Informationen finden Sie unter [Aktualisieren von aktiven Triggern](#update-active-triggers).

-   **Integration Runtimes und Freigaben**. Integration Runtimes werden nicht sehr häufig geändert und sind in allen Stufen von CI/CD ähnlich. Daher erwartet Data Factory, dass diese in allen Stufen von CI/CD den gleichen Integration Runtime-Namen bzw. -Typ aufweisen. Wenn Sie Integration Runtimes über alle Stufen hinweg freigeben möchten, können Sie eine ternäre Factory verwenden, die nur die freigegebenen Integration Runtimes enthält. Diese freigegebene Factory können Sie in allen Umgebungen als verknüpften Integration Runtime-Typ verwenden.

-   **Key Vault**. Wenn Sie verknüpfte Dienste verwenden, die auf Azure Key Vault basieren, können Sie weitere damit verbundene Vorteile nutzen, indem Sie separate Schlüsseltresore für unterschiedliche Umgebungen verwenden. Sie können auch separate Berechtigungsstufen für jeden Schlüsseltresor konfigurieren. Es kann beispielsweise sein, dass Teammitglieder nicht über Berechtigungen für Produktionsgeheimnisse verfügen sollen. Bei diesem Ansatz empfehlen wir Ihnen, in allen Stufen die gleichen Geheimnisnamen beizubehalten. Wenn Sie die gleichen Namen beibehalten, müssen Sie die Resource Manager-Vorlagen in den CI/CD-Umgebungen nicht ändern, da sich lediglich der Name des Schlüsseltresors ändert. Hierbei handelt es sich um einen der Resource Manager-Vorlagenparameter.

## <a name="unsupported-features"></a>Nicht unterstützte Funktionen

- Standardmäßig ist für Data Factory ein Cherrypicking von Commits oder die selektive Veröffentlichung von Ressourcen nicht zulässig. Veröffentlichungen umfassen alle in der Data Factory vorgenommenen Änderungen.

    - Data Factory-Entitäten sind voneinander abhängig. Beispielsweise hängen Trigger von Pipelines ab, während Pipelines von Datasets und anderen Pipelines abhängig sind. Die selektive Veröffentlichung einer Teilmenge von Ressourcen kann ggf. zu unerwartetem Verhalten und Fehlern führen.
    - In den seltenen Fällen, in denen Sie eine selektive Veröffentlichung durchführen müssen, können Sie die Verwendung eines Hotfix erwägen. Weitere Informationen finden Sie unter [Hotfix für den Produktionsbranch](#hotfix-production-branch).

-   Sie können nicht aus privaten Branches veröffentlichen.

-   Es ist derzeit nicht möglich, Projekte in Bitbucket zu hosten.
