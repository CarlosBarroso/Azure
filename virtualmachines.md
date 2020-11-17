# create virtual machine

1. New-AzureRmResourceGroup

New-AzureRmResourceGroup -Name TutorialResources -Location northeurope

2. Get-Credential

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

3. New-AzureRmVM

$vmParams = @{
  ResourceGroupName = 'virtualMachines'
  Name = 'VM1'
  Location = 'eastus'
  ImageName = 'Win2016Datacenter'
  PublicIpAddressName = 'PIP'
  Credential = $cred
  OpenPorts = 3389
}

$newVM1 = New-AzureRmVM @vmParams

$newVM1


4. Select-Object

$newVM1.OSProfile | Select-Object ComputerName,AdminUserName

$newVM1 | Get-AzureRmNetworkInterface |
  Select-Object -ExpandProperty IpConfigurations |
    Select-Object Name,PrivateIpAddress


5. Get-AzureRmPublicIpAddress

$publicIp = Get-AzureRmPublicIpAddress -Name tutorialPublicIp -ResourceGroupName TutorialResources

$publicIp | Select-Object Name,IpAddress,@{label='FQDN';expression={$_.DnsSettings.Fqdn}}

6. Connect to virtualmachine

mstsc.exe /v <PUBLIC_IP_ADDRESS>

7. Remove-AzureRmResourceGroup

$job = Remove-AzureRmResourceGroup -Name TutorialResources -Force -AsJob

$job

8. Wait-Job

Wait-Job -Id $job.Id

