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
```
Login CLI
```sh
az acr login -n myacrname
```
Login PS
```sh
Connect-AzContainerRegistry -Name mbinsightacr 
```

- Example CLI series to push a Docker Image to Azure
- 1 Login to Azure
- 2 Create the resource group
- 3 Create Container registry
- 4 Login to the container registry
- 5 Get a sample image to use
- 6 Test run the docker image

```sh
az login
az group create -n endtoend --location eastus
az acr create -g endtoend -n endtoendcr --sku Basic
az acr login --n endtoendcr
docker pull mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
docker run -it --rm -p 8080:80 mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
```
- Browse to localhost:808 and confirm site is up!
![Screen Shot](/Screenshot%20Nginx.png)
- Confirmation success - Ctrl-c in powershell to stop the run 
- 7 Tag image to be pushed
- 8 Push tagged image to the Container Registry
- 9 List Registry to see image
```sh
docker tag mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine endtoendcr.azurecr.io/samples/nginx
docker push endtoendcr.azurecr.io/samples/nginx
az acr repository list -n endtoendcr --output table
```
> Optional pull back down test
> ```sh docker pull endtoendcr.azurecr.io/samples/nginx ```

**Virtual Machines**

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

**Container Instances**
CLI
> May need to register first.
>  - Check command:
>  ```az provider list --query "[?namespace=='Microsoft.ContainerInstance']" --output table```
>  - Register Command
>  ```az provider register --namespace Microsoft.ContainerInstance```

Create a container
```sh
az container create -g containerRG -n mycontainername123 --image mcr.microsoft.com/azuredocs/aci-helloworld --dns-name-label mycontainername123 --ports 80 --os-type Linux --cpu 1 --memory 2
```

Check Creation Status
```sm
az container show -g containerRG -n mycontainername123 --query "{FQDN: ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

PS
```sh
$port = New-AzContainerInstancePortObject -Port 80 -Protocol TCP
$container = New-AZContainerInstanceObject -Name myazcontainername1234 -Image mcr.microsoft.com/azuredocs/aci-helloworld:latest -RequestCpu 1 -RequestMemoryInGb 1.5 -Port @($port)
New-AzContainerGroup -ResourceGroupName mygroupname -Name myazurecontainergroupname -Container $container -OsType Linux -IPAddressType Public -Location EastUS
```
> Get Information like IpAddress ```Get-AzContainerGroup -ResourceGroupName mygroupname -Name myazurecontainergroupname | Format-List```

Bicep 
```sh
param resourceName string
param location string = resourceGroup().location

resource container 'Microsoft.ContainerInstance/containerGroups@2023-05-01' = {
  name: resourceName
  location: location
  properties: {
    containers: [
      {
        name: resourceName
        properties: {
          image: 'mcr.microsoft.com/azuredocs/aci-helloworld'
          ports: [
            { port: 80, protocol: 'TCP' }
          ]
          resources: {
            requests: {
              cpu: 1
              memoryInGB: 2
            }
          }
        }
      }
    ]
    osType: 'Linux'
    restartPolicy: 'Always'
    ipAddress: {
      type: 'Public'
      ports: [
        {
          port: 80
          protocol: 'TCP'
        }
      ]
    }
  }
}

output containerIP string = container.properties.ipAddress.ip
```
Run command to use:
```sh
az deployment group create -g mygroupname --template-file .\azuredeploy.bicep 
```

CLI get logs
```sh
az container logs -g mygroupname -n mycontainername123
```

**Web App**

Create from Solution Directory
```sh
az webapp up -g myazurerg -n auniquewebappname --sku F1 --os-type windows
```

Update from Solution Directory
```sh
az webapp up
```

