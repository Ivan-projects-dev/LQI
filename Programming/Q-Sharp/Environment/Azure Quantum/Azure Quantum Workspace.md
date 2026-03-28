#Quantum #Cloud #Azure
 **[[Azure Quantum]] workspace** is the top-level resource for working with [[Azure Quantum]]. It ties together Azure subscription, resource group, storage account, & selected quantum providers in $1$ place.

To create workspace you need active Azure subscription & **Owner** (or **Contributor**) role on it. Select **region**, **resource group**, & **storage account** in the same group. [[Azure Quantum Providers]] such as are added during setup or later from the portal.

Workspaces can be created via the **Azure portal**, the **Azure CLI** (`az quantum workspace`), or **ARM/Bicep template** for infrastructure-as-code workflows. Once created, you connect to it in code by providing the workspace's resource ID & region, then use the `azure-quantum` Python package or Q# to submit [[Azure Quantum Jobs|jobs]].

Access control follows standard **Azure RBAC** - you can grant **Contributor** or **Reader** roles to team members on the workspace resource.
