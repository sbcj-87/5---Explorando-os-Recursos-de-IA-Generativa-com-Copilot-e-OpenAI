# Explorando os Recursos de IA Generativa com Copilot e OpenAI

Pedi para o Copilot criar para mim um script em que eu possa subir uma infraestrutura na Azure e me explique com detalhes e ele me respondeu desta maneira:

Primeiro, você precisará ter o Azure CLI instalado e configurado em seu computador.

Aqui está um exemplo de script:

```bash
# Autenticar no Azure
az login

# Criar um grupo de recursos
az group create --name MeuGrupoDeRecursos --location eastus

# Criar um arquivo de template JSON para a infraestrutura
cat <<EOF > template.json
{
  "schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2021-05-01",
      "name": "MinhaRedeVirtual",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "10.0.0.0/16"
          ]
        }
      }
    },
    {
      "type": "Microsoft.Compute/virtualMachines",
      "apiVersion": "2021-07-01",
      "name": "MinhaMaquinaVirtual",
      "location": "[resourceGroup().location]",
      "properties": {
        "hardwareProfile": {
          "vmSize": "Standard_DS1_v2"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "18.04-LTS",
            "version": "latest"
          },
          "osDisk": {
            "createOption": "FromImage"
          }
        },
        "osProfile": {
          "computerName": "MinhaMaquinaVirtual",
          "adminUsername": "azureuser",
          "adminPassword": "SenhaSegura123!"
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', 'MinhaInterfaceDeRede')]"
            }
          ]
        }
      }
    }
  ]
}
EOF

# Implantar o template
az deployment group create --resource-group MeuGrupoDeRecursos --template-file template.json
