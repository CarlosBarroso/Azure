# vnet
..* Create Network Security Group
..* Create VNet and VSubnet
..* Asociate NSG to VSubNet
..* Create a Virtual Machine

## Azure Cli
..* Create VNet and VSubnet

Az interactive

Az network create –g <resource group> --name <vnet name> --address-prefixes 10.0.0.0/16--location northeurope --subnet-name default –subnet-prefix 10.0.1.0/24

## Powershell / Webshell

### create vnet

$resourceName=”VNET”

$location=”northeurope”

New-AzureRmResourceGroup -Name $resourceName -Location $location

$vnetName=”testvnet”

$vnetAddressPrefix=”10.0.0.0/16”

New-AzureRmVirtualNetwork -ResourceGroupName $resourceName -Name $vnetName -AddressPrefix $vnetAddressPrefix -Location $location

$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceName -Name $vnetName

$subnetDefault=”default”

$subnetDefaultAddressPrefix=”10.0.1.0/24”

$subnet=Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetDefault -VirtualNetwork $vnet -AddressPrefix $subnetDefaultAddressPrefix

$subnetBackend=”Backend”

$subnetBackendAddressPrefix=”10.0.2.0/24”

Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetBackend -VirtualNetwork $vnet -AddressPrefix $subnetBackendAddressPrefix

Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

### Create NSG

$rule1 = New-AzNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `

    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix `

    Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389

$rule2 = New-AzNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `

    -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 -SourceAddressPrefix `

    Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceName  -Location $location -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetDefault -AddressPrefix $subnetDefaultAddressPrefix -NetworkSecurityGroup $nsg

Set-AzureRmVirtualNetwork  -VirtualNetwork $vnet

