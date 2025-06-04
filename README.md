# My Azure Lab Resource Group Storage Automation
Hands-on lab using Azure CLI and Bash to automate resource group and storage account setup in Microsoft Azure. Built as part of my personal cloud engineering practice to strengthen CLI proficiency, infrastructure planning, and real-world deployment skills.


Languages & Tools

- Microsoft Azure  
- Bash Shell (macOS + Azure Cloud Shell)  
- Azure CLI  
- Azure Resource Manager  
- Storage Account Services  

---

## Environments Used

- Azure Cloud Shell  
- macOS Terminal  

---

##  Program Walk-through

## 🔍 Viewing Active Azure Account & Available Regions

\```bash
az account show --output table
az account list-locations --output table
\```

These commands verified that I was working in an active subscription and helped me pick `westus2` as my preferred region for deployment.

---

## 📁 Creating a Resource Group with Dynamic Naming

\```bash
let "randomIdentifier=$RANDOM*$RANDOM"
location="westus2"
resourceGroup="clinique-rg-$randomIdentifier"

az group create --name $resourceGroup --location $location --output json
\```

This script dynamically generated a unique resource group name using my name as a prefix. This avoids conflicts and keeps my resources organized during testing.

---

## 💾 Creating a Storage Account via Azure CLI

\```bash
let "randomIdentifier=$RANDOM*$RANDOM"
storageAccount="cliniquestorage$randomIdentifier"

echo "Creating storage account $storageAccount in resource group $resourceGroup"

az storage account create \
  --name $storageAccount \
  --resource-group $resourceGroup \
  --location $location \
  --sku Standard_RAGRS \
  --kind StorageV2 \
  --output json
\```

Storage account names must be globally unique—so I used the `$randomIdentifier` variable again to ensure no naming collisions.

---

## 📋 Verifying Storage Account Deployment

\```bash
az storage account list --output table
\```

This command confirmed that my storage account was deployed correctly. It returned key details like location, redundancy type, and status (`Succeeded`).

---

## 🧹 Post-Project Cleanup

### 🔥 Delete a single resource group:

\```bash
az group delete --name $resourceGroup --yes --no-wait
\```

### 🔄 Delete all test resource groups with my prefix:

\```bash
for rg in $(az group list --query "[?starts_with(name, 'clinique-rg-')].name" -o tsv); do
  echo "Deleting resource group: $rg"
  az group delete --name $rg --yes --no-wait
done
\```

Cleaning up ensures I don’t get charged for unused resources after testing.

---

## 💵 Azure Cost Management Tips

- 🗑️ Always delete unused resources after testing  
- 📊 Use `az resource list` to monitor what’s active  
- 💸 Choose LRS for non-critical storage to reduce cost  
- 🔔 Set budget alerts in Azure Portal  
- 🧮 Use the [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) before scaling up

---

## ✅ Tasks Completed

- ☑️ Verified subscription and active region using CLI  
- ☑️ Created resource group using personalized naming convention  
- ☑️ Deployed geo-redundant StorageV2 account  
- ☑️ Verified deployment through Azure CLI  
- ☑️ Deleted all test resources using automation

---

## 🧠 Final Thoughts

This lab gave me real-world practice using Azure CLI and Bash to deploy, verify, and clean up cloud infrastructure. I applied best practices like dynamic naming, cost control, and CLI scripting—all essential skills in DevOps and cloud workflows.

---

## 📘 Try It Yourself

You can run these commands directly in the [Azure Cloud Shell](https://shell.azure.com). No setup required—just sign in with your Azure subscription and follow the steps above.

---

## 📘 Try It Yourself

