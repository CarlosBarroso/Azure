# storage account 

1. login to azure

Connect-AzAccount

2. get locations

Get-AzLocation | select Location

$location = "northeurope"

3. create resource group

$resourceGroup = "BlobExample"

New-AzResourceGroup -Name $resourceGroup -Location $location

4. create storage account

$storageName= "cstorage123456"

$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `

  -Name $storageName `

  -SkuName Standard_LRS `

  -Location $location `

$ctx = $storageAccount.Context

5. create container

$containerName = "docs"

New-AzStorageContainer -Name $containerName -Context $ctx -Permission blob

6. create queue

$queue=”inputqueue”

New-AzureStorageQueue -Name mensajes -Context $ctx

$sa = Get-AzStorageAccount -StorageAccountName $storageName -ResourceGroupName $resourceGroup

$saKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroup -Name $storageName)[0].Value

7. delete resource group

$job = Remove-AzureRmResourceGroup -Name $resourceGroup -Force -AsJob

$job

Wait-Job -Id $job.Id

