# Azure Databricks Private Endpoint POC

## Context
This Proof of Concept (POC) outlines the two approaches tested for setting up **Azure Databricks** with **public access disabled**. The configuration involved creating **private endpoints** and modifying network settings for the **managed storage** associated with Databricks.

## Approaches

### Approach 1: Network Configuration Failure

#### Configuration
- **Private Endpoint (PE)** for the **Managed Storage Account** and **Databricks PE** both reside in **VNET-1** (infra-VNET).
- **VNET injection** for Databricks is enabled in a separate **VNET-2** (databricks-VNET).
- Both **infra-VNET** and **databricks-VNET** are peered.

#### Outcome
- **Network configuration failure** due to unknown issues.

#### Notes
- According to the official documentation, the subnet must be in the **same VNet** as the workspace or in a separate VNet accessible to the workspace.
- Subnet size should be a minimum of **/28 in CIDR** notation.
- This approach seems potentially viable, but certain issues need resolution from **Microsoft**, which has been informed and is working on it.

---

### Approach 2: Successful Configuration with Disadvantages

#### Configuration
- **Private Endpoint** for the **Managed Storage Account** and **Databricks workspace** created within the same **VNET** (subnet) as the **Databricks VNET**, as per Microsoft’s recommendation.

#### Outcome
- Everything works as expected.

#### Disadvantages
- Requires **recreating** the entire **Databricks resource**, including:
  - **Databricks workspace**
  - **Managed storage**
  - **Databricks VNET** (due to VNET injection)
  
- **VNET resizing** is not possible without recreating it. Since the VNET has been split in half, this may necessitate recreating the entire Databricks environment. This happens because the **Databricks workspace (control plane)** is linked to the **Databricks VNET** via VNET injection.

---

## Summary Table

| Approach | Configuration | Outcome | Disadvantages |
| --- | --- | --- | --- |
| **1** | - PE for Managed Storage Account and Databricks PE are in **VNET-1 (infra-VNET)**.<br>- VNET injection for Databricks enabled in separate **VNET-2 (databricks-VNET)**.<br>- **VNETs are peered**. | **Network configuration failure.** | - **Potentially viable** but facing network issues.<br>- **Awaiting resolution** from Microsoft. |
| **2** | - PE for Managed Storage Account and Databricks workspace in **same VNET** as the Databricks VNET. | **Successful** configuration. | - Requires **recreation** of the entire Databricks setup.<br>- **VNET resizing limitations**—may require recreating the environment due to VNET injection constraints. |

---

## Conclusion
- **Approach 1**: Potentially viable but currently facing network configuration issues. Awaiting Microsoft's resolution.
- **Approach 2**: Functional but introduces significant overhead in resource recreation and VNET resizing limitations.

Further actions will depend on Microsoft's resolution of the network configuration issue in **Approach 1**.
