---
layout: "aviatrix"
page_title: "Aviatrix: aviatrix_transit_gateway"
description: |-
  Creates and Manages the Aviatrix transit network gateways
---

# aviatrix_transit_gateway

The aviatrix_transit_gateway resource creates and manages the Aviatrix transit network gateways.

## Example Usage

```hcl
# Create an Aviatrix AWS Transit Network Gateway
resource "aviatrix_transit_gateway" "test_transit_gateway_aws" {
  cloud_type               = 1
  account_name             = "devops_aws"
  gw_name                  = "transit"
  vpc_id                   = "vpc-abcd1234"
  vpc_reg                  = "us-east-1"
  gw_size                  = "t2.micro"
  subnet                   = "10.1.0.0/24"
  ha_subnet                = "10.1.0.0/24"
  ha_gw_size               = "t2.micro"
  tag_list                 = [
    "name:value",
    "name1:value1",
    "name2:value2",
  ]
  enable_hybrid_connection = true
  connected_transit        = true
}
```
```hcl
# Create an Aviatrix GCP Transit Network Gateway
resource "aviatrix_transit_gateway" "test_transit_gateway_gcp" {
  cloud_type   = 4
  account_name = "devops-gcp"
  gw_name      = "avtxgw-gcp"
  vpc_id       = "vpc-gcp-test"
  vpc_reg      = "us-west2-a"
  gw_size      = "n1-standard-1"
  subnet       = "10.8.0.0/16"
  ha_zone      = "us-west2-b"
  ha_gw_size   = "n1-standard-1"
}
```
```hcl
# Create an Aviatrix ARM Transit Network Gateway
resource "aviatrix_transit_gateway" "test_transit_gateway_azure" {
  cloud_type        = 8
  account_name      = "devops_azure"
  gw_name           = "transit"
  vpc_id            = "vnet1:hello"
  vpc_reg           = "West US"
  gw_size           = "Standard_B1s"
  subnet            = "10.30.0.0/24"
  ha_subnet         = "10.30.0.0/24"
  ha_gw_size        = "Standard_B1s"
  connected_transit = true
}
```
```hcl
# Create an Aviatrix Oracle Transit Network Gateway
resource "aviatrix_transit_gateway" "test_transit_gateway_oracle" {
  cloud_type   = 16
  account_name = "devops-oracle"
  gw_name      = "avtxgw-oracle"
  vpc_id       = "vpc-oracle-test"
  vpc_reg      = "us-ashburn-1"
  gw_size      = "VM.Standard2.2"
  subnet       = "10.7.0.0/16"
}
```

## Argument Reference

The following arguments are supported:

* `cloud_type` - (Required) Type of cloud service provider, requires an integer value. Currently only AWS(1), GCP(4), ARM(8), and OCI(16) are supported.
* `account_name` - (Required) This parameter represents the name of a Cloud-Account in Aviatrix controller.
* `gw_name` - (Required) Name of the gateway which is going to be created.
* `vpc_id` - (Required) VPC-ID/VNet-Name of cloud provider. Required if for aws. Example: AWS: "vpc-abcd1234", GCP: "vpc-gcp-test".
* `vpc_reg` - (Required) Region of cloud provider. Example: AWS: "us-east-1", ARM: "East US 2", Oracle: "us-ashburn-1", GCP: "us-west2-a".
* `gw_size` - (Required) Size of the gateway instance. Example: AWS: "t2.large", ARM: "Standard_B1s", Oracle: "VM.Standard2.2", GCP: "n1-standard-1".
* `subnet` - (Required) A VPC Network address range selected from one of the available network ranges. Example: "172.31.0.0/20".
* `allocate_new_eip` - (Optional) When value is false, reuse an idle address in Elastic IP pool for this gateway. Otherwise, allocate a new Elastic IP and use it for this gateway. Available in controller 4.7 or later release. Valid values: true, false. Default: true. Option not available for GCP, ARM and Oracle gateways, they will automatically allocate new eip's.
* `eip` - (Optional) Required when allocate_new_eip is false. It uses specified EIP for this gateway. Available in controller 4.7 or later release.
* `ha_subnet` - (Optional) HA Subnet CIDR. Required for enabling HA for AWS/ARM gateway. Setting to empty/unset will disable HA. Setting to a valid subnet CIDR will create an HA gateway on the subnet. Example: "10.12.0.0/24".
* `ha_zone` - (Optional) HA Zone. Required for enabling HA for GCP gateway. Setting to empty/unset will disable HA. Setting to a valid zone will create an HA gateway in the zone. Example: "us-west1-c".
* `ha_gw_size` - (Optional) HA Gateway Size. Mandatory if HA is enabled (ha_subnet is set). Example: "t2.micro".
* `ha_eip` - (Optional) Public IP address that you want to assign to the HA peering instance. If no value is given, a new eip will automatically allocated. Only available for AWS.
* `enable_snat` - (Optional) Enable Source NAT for this container. Valid values: true, false.
* `single_az_ha` (Optional) Set to true if this feature is desired. Valid values: true, false.
* `tag_list` - (Optional) Instance tag of cloud provider. Only supported for AWS. Example: ["key1:value1","key2:value2"].
* `enable_hybrid_connection` - (Optional) Sign of readiness for TGW connection. Only supported for AWS. Example: false.
* `enable_firenet` - (Optional) Sign of readiness for FireNet connection. Valid values: true, false. Default value: false.
* `connected_transit` - (Optional) Specify Connected Transit status. If enabled, it allows spokes to run traffics to other spokes via transit gateway. Valid values: true, false. Default value: false.
* `insane_mode` - (Optional) Specify Insane Mode high performance gateway. Insane Mode gateway size must be at least c5 size (AWS) or Standard_D3_v2 (ARM). If enabled, you must specify a valid /26 CIDR segment of the VPC to create a new subnet. Only available for AWS and ARM. Valid values: true, false.
* `insane_mode_az` - (Optional) AZ of subnet being created for Insane Mode Transit Gateway. Required for AWS if insane_mode is enabled. Example: AWS: "us-west-1a".
* `ha_insane_mode_az` - (Optional) AZ of subnet being created for Insane Mode Transit HA Gateway. Required for AWS if insane_mode is enabled and ha_subnet is set. Example: AWS: "us-west-1a".
* `enable_active_mesh` - (Optional) Switch to Enable/Disable Active Mesh Mode for Transit Gateway. Valid values: true, false. Default value: false.
* `enable_vpc_dns_server` - (Optional) Enable VPC DNS Server for Gateway. Currently only supports AWS. Valid values: true, false. Default value: false.
* `enable_advertise_transit_cidr` - (Optional) Switch to Enable/Disable advertise transit VPC network CIDR for a vgw connection. Available as of R2.6.
* `bgp_manual_spoke_advertise_cidrs` - (Optional) Intended CIDR list to advertise to VGW. Example: "10.2.0.0/16,10.4.0.0/16". Available as of R2.6.
* `enable_encrypt_volume` - (Optional) Enable Encrypt EBS Volume feature for Gateway. Only supports AWS. Valid values: true, false. Default value: false.
* `customer_managed_keys` - (Optional and Sensitive) Customer managed key ID.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `eip` - Public IP address assigned to the gateway.
* `ha_eip` - Public IP address assigned to the HA gateway.

The following arguments are deprecated:

* `enable_firenet_interfaces` - (Optional) Sign of readiness for FireNet connection. Valid values: true, false. Default value: false.

-> **NOTE:** `enable_firenet` - If you are using/upgraded to Aviatrix Terraform Provider R2.5+/UserConnect-5.0+ , and an AWS transit_gateway resource with "enable_firenet_interfaces" enabled was created with a provider version < R2.5/ UserConnect-5.0, you must replace "enable_firenet_interfaces" with "enable_firenet" in your configuration file, and do ‘terraform refresh’ to set its value to "enable_firenet" and apply it into the state file.

-> **NOTE:** `enable_advertise_transit_cidr` and `bgp_manual_spoke_advertise_cidrs` functionality has been migrated over to **aviatrix_transit_gateway** as of Aviatrix Terraform Provider R2.6. If you are using/upgraded to Aviatrix Terraform Provider R2.6+, and a vgw_conn resource was originally created with a provider version <R2.6, you must cut and paste these two arguments (and values) into the corresponding transit gateway resource referenced in the **vgw_conn**. A 'terraform refresh' will then successfully complete the migration and rectify the state file.

-> **NOTE:** `subnet` - If `insane_mode` is enabled, you must specify a valid /26 CIDR segment of the VPC specified. This will then create a new subnet to be used for the corresponding gateway. You cannot specify an existing /26 subnet.

## Import

Instance transit_gateway can be imported using the gw_name, e.g.

```
$ terraform import aviatrix_transit_gateway.test gw_name
```
