## Azure CLI and PS Commands
**Login**

CLI
```sh
az login
```
PS
```sh 
Connect-AzAccount
```
**Resource Groups**

Create CLI
```sh
az group create -n mygroupname --location eastus
```
Create PS
```sh
New-AzResourceGroup -Name mygroupname -Location EastUS
```
Delete CLI
```sh
az group delete --name mygroupname --no-wait --yes --verbose
```
Delete PS
```sh
Remove-AzResourceGroup -Name mygroupname
```
List CLI
```sh
az group list
```
**Container Registries**

Create CLI
```sh
az acr create -g mygroupname -n myacrname --sku Basic
```
Create PS
```sh
New-AzContainerRegistry -ResourceGroupName mygroupname -Name myacrname -EnableAdminUser -Sku Basic
```sh
Login CLI
```
az acr login -n myacrname
```
Login PS
```sh
Connect-AzContainerRegistry -Name mbinsightacr 
```

***Virtual Machines***

Create CLI
```sh
az vm create --resource-group mygroupname --name myvmname --image Debian11 --admin-username azureuser
```
Create PS
```sh
New-AzVm -ResourceGroupName mbinsight -Name mbaz204 -Location EastUS -Image Debian11 -Size Standard_B22s -PublicIpAddressName mbaz204 -OpenPorts 80 -GenerateSshKey -SshKeyName mbaz204 
```
Open-Port CLI
```sh
az vm open-port --resource-group mbinsight --name mbaz204linux --port 80
```
                                              
                                                                                      
