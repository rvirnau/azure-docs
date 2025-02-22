---
title: Manage IP addresses with Azure Virtual Network Manager
description: Learn how to manage IP addresses with Azure Virtual Network Manager by creating and assigning IP address pools to your virtual networks.
author: mbender-ms
ms.author: mbender
ms.service: azure-virtual-network-manager
ms.topic: how-to
ms.date: 10/2/2024
ms.custom:  references_regions
#customer intent: As a network administrator, I want to learn how to manage IP addresses with Azure Virtual Network Manager so that I can create and assign IP address pools to my virtual networks.
---

# Manage IP addresses with Azure Virtual Network Manager

[!INCLUDE [virtual-network-manager-ipam](../../includes/virtual-network-manager-ipam.md)]

Azure Virtual Network Manager allows you to manage IP addresses by creating and assigning IP address pools to your virtual networks. This article shows you how to create and assign IP address pools to your virtual networks with IP address management in Azure Virtual Network Manager.

## Prerequisites

- An Azure subscription. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.
- An existing network manager instance. If you don't have a network manager instance, see [Create a network manager instance](create-virtual-network-manager-portal.md).
- A virtual network that you want to associate with an IP address pool.
- To manage IP addresses in your network manager, you have the **Network Contributor** role with [role-based access control](../role-based-access-control/quickstart-assign-role-user-portal.md) Classic Admin/legacy authorization isn't supported.

## Create an IP address pool

In this step, you create an IP address pool for your virtual network.

1. In the Azure portal, search for and select **Network managers**.
2. Select your network manager instance.
3. In the left menu, select **IP address pools (Preview)** under **IP address management (Preview)**.
4. Select **+ Create** or **Create** to create a new IP address pool.
5. In the **Create an IP address pool** window, enter the following information:
   
    | Field | Description |
    | --- | --- |
    | **Name** | Enter a name for the IP address pool. |
    | **Description** | Enter a description for the IP address pool. |
    | **Parent pool** | For creating a **root pool**, leave default of **None**. For creating a **child pool**, select the parent pool. |

    :::image type="content" source="media/how-to-manage-ip-addresses/create-root-pool.png" alt-text="Screenshot of Create an ip address pool settings for a root pool." :::   
    
6. Select **Next** or the **IP addresses** tab.
7. Under **Starting address**, enter the IP address range for the pool.

    :::image type="content" source="media/how-to-manage-ip-addresses/set-pool-ip-range-thumb.png" alt-text="Screenshot of IP address range settings for a root pool." lightbox="media/how-to-manage-ip-addresses/set-pool-ip-range.png"::: 

8. Select **Review + create** and then **Create** to create the IP address pool.
9. Repeat these steps for another root or child pool.

## Associate a virtual network with an IP address pool

In this step, you associate an existing virtual network with an IP address pool from the **Allocations** settings page in the IP address pool.  

1. Browse to your network manager instance and select your IP address pool.
2. From the left menu, select **Allocations** under **Settings** or select **Allocate**. 
3. In the **Allocations** window, select **+ Create** > **Associate resources**. The **Associate resources** option allocates a CIDR to an existing virtual network.
   
    :::image type="content" source="media/how-to-manage-ip-addresses/pool-allocation-settings-associate-resource-thumb.png" alt-text="Screenshot of allocations page for associating resources." lightbox="media/how-to-manage-ip-addresses/pool-allocation-settings-associate-resource.png":::

4. In the **Select resources** window, select the virtual networks you want to associate with the IP address pool and then choose **Select**.
   
   :::image type="content" source="media/how-to-manage-ip-addresses/associate-virtual-network-resources-thumb.png" alt-text="Screenshot of associate resources page with virtual networks selected." lightbox="media/how-to-manage-ip-addresses/associate-virtual-network-resources.png":::

5. Verify the virtual network is listed.
   
   :::image type="content" source="media/how-to-manage-ip-addresses/ip-address-pool-allocation-statistics.png" alt-text="Screenshot of IP address pool allocations and statistics.":::

> [!Note]
> In addition to associating resources, you can allocate address spaces to a child pool or a static CIDR block from the a pool's Allocations page.

## Create static CIDR blocks for a pool

In this step, you create a static CIDR block for a pool. This is helpful for allocating a space that is outside of Azure or Azure resources not supported by IP address manager. For example, you can allocate a CIDR in the pool to the address space in your on-premises environment. Likewise, you can also use this for a space that is used by a Virtual WAN hub or Azure VMware Private Cloud.

1. Browse to your IP address pool.
2. Select **Allocate** or **Allocations** under **Settings**.
3. In the **Allocations** window, select **+ Create** > **Allocate static CIDRs**.
4. In the **Allocate static CIDRs from pool** window, enter the following information:
   
    | Field | Description |
    | --- | --- |
    | **Name** | Enter a name for the static CIDR block.|
    | **Description** | Enter a description for the static CIDR block. |
    | **CIDR** | Enter the CIDR block. |

    :::image type="content" source="media/how-to-manage-ip-addresses/create-static-cidr-reservation.png" alt-text="Screenshot of Allocate static CIDR from pool window with address range for CIDR reservation.":::
    
5. Select **Allocate**.

## Review allocation usage

In this step, you review the allocation usage of the IP address pool. This helps you understand how the CIDRs are being used in the pool, along with the percentage of the pool that is allocated and the compliance status of the pool.

1. Browse to your IP address pool.
2. Select **Allocations** under **Settings**.
3. In the **Allocations** window, you can review all of the statistics for the address pool including:
   
    | Field | Description |
    | --- | --- |
    | **Pool address space** | The total address space that is allocated to the pool. |
    | **Allocated address Space** | The address space that is allocated to the pool. |
    | **Available address Space** | The address space that is available for allocation. |
    | **Available address count** | The number of addresses that are allocated to the pool. |
    | **IP allocation** | The set of IP addresses that are allocated from the pool for potential use. |

    :::image type="content" source="media/how-to-manage-ip-addresses/review-ip-address-pool-allocations.png" alt-text="Screenshot of an IP address pool's allocations and statistics for the pool.":::

4. For each allocation, you can review the following:
   
    | Field | Description |
    | --- | --- |
    | **Name** | The name of the allocation. |
    | **Address space** | The address space that is allocated to the pool. |
    | **Address count** | The number of addresses that are allocated to the pool. |
    | **IP allocation** | The set of IP addresses that are allocated from the pool for potential use. |
    | **Status** | The status of the allocation to the pool. |
    
    :::image type="content" source="media/how-to-manage-ip-addresses/review-ip-address-pool-allocations-by-resource.png" alt-text="Screenshot of ip address pool allocations highlighting individual resource information.":::

## Delegating permissions for IP address management

In this step, you delegate permissions to other users to manage IP address pools in your network manager using [Azure role-based access control (RBAC)](../role-based-access-control/check-access.md). This allows you to control access to the IP address pools and ensure that only authorized users can manage the pools.

1. Browse to your IP address pool.
2. In the left menu, select **Access control (IAM)**.
3. In the **Access control (IAM)** window, select **+ Add**>**Add role assignment**.
4. Under **Role**, select **IPAM Pool User** through the search bar under the **Job function roles** tab, and then select **Next**.
5. On the **Members** tab, select how you wish to assign access to the role. You can assign access to a user, group, or service principal, or you can use a managed identity.

    :::image type="content" source="media/how-to-manage-ip-addresses/delegate-ip-address-pool-permissions.png" alt-text="Screenshot of the Add role assignment window with IPAM Pool User selected.":::

6. Choose **+ Select members** and then **Select** the user, group, service principal, or managed identity that you want to assign the role to.
7. Select **Review + assign** and then **Assign** to delegate permissions to the user.


## Create a virtual network with a nonoverlapping CIDR range

In this step, you create a virtual network with a nonoverlapping CIDR range by allowing IP address manager to automatically provide a nonoverlapping CIDR.

1. In the Azure portal, search for and select **Virtual networks**.
2. Select **+ Create**.
3. On the **Basics** tab, enter the following information:
   
    | Field | Description |
    | --- | --- |
    | **Subscription** | Select the subscription managed by a Network Manager management scope. |
    | **Resource group** | Select the resource group for the virtual network. |
    | **Name** | Enter a name for the virtual network. |
    | **Region** | Select the region for the virtual network. IP address pools must be in the same region as your virtual network in order to be associated.|
 
4. Select the **IP addresses** tab or **Next** > **Next**.
5. On the **IP addresses** tab, select **Allocate using IP address pools** checkbox.
   
   :::image type="content" source="media/how-to-manage-ip-addresses/create-virtual-network-ip-address-pool.png" alt-text="Screenshot of create virtual network window with Allocate using IP address setting.":::

6. In the **Select an IP address pool** window, select the IP address pool that you want to associate with the virtual network and then choose **Save**. You can select at most one IPv4 pool and one IPv6 pool for association to a single virtual network.
   
    :::image type="content" source="media/how-to-manage-ip-addresses/virtual-network-create-select-ip-address-pool-thumb.png" alt-text="Screenshot of Select an IP address pool with IP address pool selected." lightbox="media/how-to-manage-ip-addresses/virtual-network-create-select-ip-address-pool.png":::

7. From the dropdown menu next to your IP address pool, select the size for the virtual network.
   
    :::image type="content" source="media/how-to-manage-ip-addresses/virtual-network-create-select-address-space-size.png" alt-text="Screenshot of Create virtual network window with IP address size selection.":::

8. Optionally create subnets referring to the selected pool.
9.  Select **Review + create** and then **Create** to create the virtual network.

## Next steps

> [!div class="nextstepaction"]
> [What is IP address management in Azure Virtual Network Manager](./concept-ip-address-management.md)

