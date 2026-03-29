#Quantum #Cloud #Azure
 **[[Azure Quantum]] workspace** is the top-level resource for working with [[Azure Quantum]]. It ties together Azure subscription, resource group, storage account, & selected quantum providers in $1$ place.

To create workspace you need active Azure subscription & **Owner** (or **Contributor**) role on it. Select **region**, **resource group**, & **storage account** in the same group. [[Azure Quantum Providers]] such as are added during setup or later from the portal.

Workspaces can be created via the **Azure portal**, the **Azure CLI** (`az quantum workspace`), or **ARM/Bicep template** for infrastructure-as-code workflows. Once created, you connect to it in code by providing the workspace's resource ID & region, then use the `azure-quantum` Python package or Q# to submit [[Azure Quantum Jobs|jobs]].

Access control follows standard **Azure RBAC** - you can grant **Contributor** or **Reader** roles to team members on the workspace resource.

## Connecting via Python SDK

Install: `pip install azure-quantum[qsharp]` (or `[qiskit]`, `[cirq]`). Connect using resource ID:

```python
from azure.quantum import Workspace

workspace = Workspace(
    resource_id="/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Quantum/Workspaces/<name>",
    location="eastus"
)
```

Alternatively specify `subscription_id`, `resource_group`, and `workspace_name` as separate params. Auth uses **DefaultAzureCredential** (Azure CLI login, managed identity, or env vars).

## Azure CLI Quick Reference

```bash
az extension add --name quantum
az quantum workspace create -g <rg> -w <name> -l eastus --storage-account <sa>
az quantum workspace set -g <rg> -w <name>
az quantum target list          # list available providers & targets
az quantum job submit --job-name <n> --target-id <id> --shots 1000
az quantum job output -o table  # view results
```

## Notes
- Workspace is region-bound - choose a region where your desired providers are available (check [global availability](https://learn.microsoft.com/en-us/azure/quantum/provider-global-availability)).
- Storage account in the same resource group holds job input/output blobs; billed at standard Azure Storage rates.
- You can tag workspaces for cost management & use **Azure Policy** to enforce allowed providers.
- Multiple workspaces can share the same storage account but each has an independent job queue.

## Sources
- [Create an Azure Quantum workspace](https://learn.microsoft.com/en-us/azure/quantum/how-to-create-workspace)
- [Connect to your workspace](https://learn.microsoft.com/en-us/azure/quantum/how-to-connect-workspace)
- [Manage workspaces with Azure CLI](https://learn.microsoft.com/en-us/azure/quantum/how-to-manage-quantum-workspaces-with-the-azure-cli)
- [azure-quantum Python package (PyPI)](https://pypi.org/project/azure-quantum/)
- [Global provider availability](https://learn.microsoft.com/en-us/azure/quantum/provider-global-availability)
