# Setup Guide for Azure Databricks with VNet Injection

## 1. Prerequisites

Ensure you have the following:

- An Azure Databricks Workspace set up.
- Permissions to create and manage VNets, Subnets, and NAT Gateways in Azure.
- VNet Injection enabled in Databricks.

## 2. Create a Virtual Network (VNet)

1. In the Azure portal, go to **Virtual Networks** and click **+ Create**.
2. Configure the fields:
   - **Name**: Your VNet name (e.g., `DatabricksVNet`).
   - **Region**: Same region as your Databricks workspace.
   - **Address Space**: Set a CIDR range (e.g., `10.0.0.0/16`).
3. Click **Next: IP Addresses** and add two subnets:
   - **Control Plane Subnet**: (e.g., `10.0.1.0/24`) — Subnet for communication with the Databricks Control Plane.
   - **Data Plane Subnet**: (e.g., `10.0.2.0/24`) — Subnet for the clusters.

## 3. Associate the VNet with Azure Databricks

1. Navigate to the Azure Databricks Workspace resource in the Azure portal.
2. Click **Networking** and select **VNet Injection**.
3. Choose the VNet and associate the created subnets:
   - **Control Plane Subnet**: `10.0.1.0/24`.
   - **Data Plane Subnet**: `10.0.2.0/24`.
4. Save the changes.

## 4. Create and Configure the NAT Gateway

1. In the Azure portal, go to **NAT Gateways** and click **+ Create**.
2. Configure the fields:
   - **Name**: Resource name (e.g., `DatabricksNATGateway`).
   - **Region**: Same region as the Databricks Workspace.
3. Associate Public IP Addresses:
   - Create or select a static public IP address.
   - These IPs will be used as egress for the clusters.
4. Associate the NAT Gateway with the Data Plane Subnet:
   - Go to **Subnet Association** in the NAT Gateway resource.
   - Select the subnet `10.0.2.0/24`.

## 5. Update the Firewall

1. Add the static public IPs configured in the NAT Gateway to your firewall:
   - In the Azure portal, go to the firewall resource.
   - Create a rule to allow outbound traffic using the configured IPs.

## 6. Validate the Configuration

1. Create a Job Cluster in Databricks and run a simple task (e.g., reading a file from Azure Storage).
2. Check the firewall logs to confirm that traffic is using the NAT Gateway's static IPs.

## 7. Manage IPs and Monitor

- Whenever new public IPs are needed, associate them with the NAT Gateway.
- Monitor connectivity directly using Azure Network Watcher.


# Setup for configuring nat gateway in azure

https://www.youtube.com/watch?v=2SJ4kxN-D5g&t=129s
