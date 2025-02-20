import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Management VPC

Deploy a VPC  on AWS for administrative and management functions.

<a href="https://github.com/gruntwork-io/terraform-aws-service-catalog/tree/master/modules/networking/vpc-mgmt" className="link-button">View on GitHub</a>

### Reference

<Tabs>
<TabItem value="inputs" label="Inputs" default>

<a name="apply_default_nacl_rules" className="snap-top"></a>

* [**`apply_default_nacl_rules`**](#apply_default_nacl_rules) &mdash; If true, will apply the default NACL rules in [`default_nacl_ingress_rules`](#default_nacl_ingress_rules) and [`default_nacl_egress_rules`](#default_nacl_egress_rules) on the default NACL of the VPC. Note that every VPC must have a default NACL - when this is false, the original default NACL rules managed by AWS will be used.

<a name="associate_default_nacl_to_subnets" className="snap-top"></a>

* [**`associate_default_nacl_to_subnets`**](#associate_default_nacl_to_subnets) &mdash; If true, will associate the default NACL to the public, private, and persistence subnets created by this module. Only used if [`apply_default_nacl_rules`](#apply_default_nacl_rules) is true. Note that this does not guarantee that the subnets are associated with the default NACL. Subnets can only be associated with a single NACL. The default NACL association will be dropped if the subnets are associated with a custom NACL later.

<a name="availability_zone_exclude_ids" className="snap-top"></a>

* [**`availability_zone_exclude_ids`**](#availability_zone_exclude_ids) &mdash; List of excluded Availability Zone IDs.

<a name="availability_zone_exclude_names" className="snap-top"></a>

* [**`availability_zone_exclude_names`**](#availability_zone_exclude_names) &mdash; List of excluded Availability Zone names.

<a name="availability_zone_state" className="snap-top"></a>

* [**`availability_zone_state`**](#availability_zone_state) &mdash; Allows to filter list of Availability Zones based on their current state. Can be either "available", "information", "impaired" or "unavailable". By default the list includes a complete set of Availability Zones to which the underlying AWS account has access, regardless of their state.

<a name="aws_region" className="snap-top"></a>

* [**`aws_region`**](#aws_region) &mdash; The AWS region to deploy into

<a name="cidr_block" className="snap-top"></a>

* [**`cidr_block`**](#cidr_block) &mdash; The IP address range of the VPC in CIDR notation. A prefix of /16 is recommended. Do not use a prefix higher than /27. Examples include '10.100.0.0/16', '10.200.0.0/16', etc.

<a name="create_flow_logs" className="snap-top"></a>

* [**`create_flow_logs`**](#create_flow_logs) &mdash; If you set this variable to false, this module will not create VPC Flow Logs resources. This is used as a workaround because Terraform does not allow you to use the 'count' parameter on modules. By using this parameter, you can optionally create or not create the resources within this module.

<a name="create_network_acls" className="snap-top"></a>

* [**`create_network_acls`**](#create_network_acls) &mdash; If set to false, this module will NOT create Network ACLs. This is useful if you don't want to use Network ACLs or you want to provide your own Network ACLs outside of this module.

<a name="custom_tags" className="snap-top"></a>

* [**`custom_tags`**](#custom_tags) &mdash; A map of tags to apply to the VPC, Subnets, Route Tables, and Internet Gateway. The key is the tag name and the value is the tag value. Note that the tag 'Name' is automatically added by this module but may be optionally overwritten by this variable.

<a name="custom_tags_vpc_only" className="snap-top"></a>

* [**`custom_tags_vpc_only`**](#custom_tags_vpc_only) &mdash; A map of tags to apply just to the VPC itself, but not any of the other resources. The key is the tag name and the value is the tag value. Note that tags defined here will override tags defined as [`custom_tags`](#custom_tags) in case of conflict.

<a name="default_nacl_egress_rules" className="snap-top"></a>

* [**`default_nacl_egress_rules`**](#default_nacl_egress_rules) &mdash; The egress rules to apply to the default NACL in the VPC. This is the security group that is used by any subnet that doesn't have its own NACL attached. The value for this variable must be a map where the keys are a unique name for each rule and the values are objects with the same fields as the egress block in the [`aws_default_network_acl`](#aws_default_network_acl) resource: [`https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_network_acl`](#https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_network_acl).

<a name="default_nacl_ingress_rules" className="snap-top"></a>

* [**`default_nacl_ingress_rules`**](#default_nacl_ingress_rules) &mdash; The ingress rules to apply to the default NACL in the VPC. This is the NACL that is used by any subnet that doesn't have its own NACL attached. The value for this variable must be a map where the keys are a unique name for each rule and the values are objects with the same fields as the ingress block in the [`aws_default_network_acl`](#aws_default_network_acl) resource: [`https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_network_acl`](#https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_network_acl).

<a name="default_security_group_egress_rules" className="snap-top"></a>

* [**`default_security_group_egress_rules`**](#default_security_group_egress_rules) &mdash; The egress rules to apply to the default security group in the VPC. This is the security group that is used by any resource that doesn't have its own security group attached. The value for this variable must be a map where the keys are a unique name for each rule and the values are objects with the same fields as the egress block in the [`aws_default_security_group`](#aws_default_security_group) resource: [`https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_security_group`](#https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_security_group)#egress-block.

<a name="default_security_group_ingress_rules" className="snap-top"></a>

* [**`default_security_group_ingress_rules`**](#default_security_group_ingress_rules) &mdash; The ingress rules to apply to the default security group in the VPC. This is the security group that is used by any resource that doesn't have its own security group attached. The value for this variable must be a map where the keys are a unique name for each rule and the values are objects with the same fields as the ingress block in the [`aws_default_security_group`](#aws_default_security_group) resource: [`https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_security_group`](#https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/default_security_group)#ingress-block.

<a name="enable_default_security_group" className="snap-top"></a>

* [**`enable_default_security_group`**](#enable_default_security_group) &mdash; If set to false, the default security groups will NOT be created.

<a name="kms_key_arn" className="snap-top"></a>

* [**`kms_key_arn`**](#kms_key_arn) &mdash; The ARN of a KMS key to use for encrypting VPC the flow log. A new KMS key will be created if this is not supplied.

<a name="kms_key_user_iam_arns" className="snap-top"></a>

* [**`kms_key_user_iam_arns`**](#kms_key_user_iam_arns) &mdash; VPC Flow Logs will be encrypted with a KMS Key (a Customer Master Key). The IAM Users specified in this list will have access to this key.

<a name="nat_gateway_custom_tags" className="snap-top"></a>

* [**`nat_gateway_custom_tags`**](#nat_gateway_custom_tags) &mdash; A map of tags to apply to the NAT gateways, on top of the [`custom_tags`](#custom_tags). The key is the tag name and the value is the tag value. Note that tags defined here will override tags defined as [`custom_tags`](#custom_tags) in case of conflict.

<a name="num_availability_zones" className="snap-top"></a>

* [**`num_availability_zones`**](#num_availability_zones) &mdash; How many AWS Availability Zones (AZs) to use. One subnet of each type (public, private app) will be created in each AZ. Note that this must be less than or equal to the total number of AZs in a region. A value of null means all AZs should be used. For example, if you specify 3 in a region with 5 AZs, subnets will be created in just 3 AZs instead of all 5. Defaults to 3.

<a name="num_nat_gateways" className="snap-top"></a>

* [**`num_nat_gateways`**](#num_nat_gateways) &mdash; The number of NAT Gateways to launch for this VPC. The management VPC defaults to 1 NAT Gateway to save on cost, but to increase redundancy, you can adjust this to add additional NAT Gateways.

<a name="private_subnet_bits" className="snap-top"></a>

* [**`private_subnet_bits`**](#private_subnet_bits) &mdash; Takes the CIDR prefix and adds these many bits to it for calculating subnet ranges.  MAKE SURE if you change this you also change the CIDR spacing or you may hit errors.  See cidrsubnet interpolation in terraform config for more information.

<a name="private_subnet_cidr_blocks" className="snap-top"></a>

* [**`private_subnet_cidr_blocks`**](#private_subnet_cidr_blocks) &mdash; A map listing the specific CIDR blocks desired for each private subnet. The key must be in the form AZ-0, AZ-1, ... AZ-n where n is the number of Availability Zones. If left blank, we will compute a reasonable CIDR block for each subnet.

<a name="private_subnet_custom_tags" className="snap-top"></a>

* [**`private_subnet_custom_tags`**](#private_subnet_custom_tags) &mdash; A map of tags to apply to the private Subnet, on top of the [`custom_tags`](#custom_tags). The key is the tag name and the value is the tag value. Note that tags defined here will override tags defined as [`custom_tags`](#custom_tags) in case of conflict.

<a name="public_subnet_bits" className="snap-top"></a>

* [**`public_subnet_bits`**](#public_subnet_bits) &mdash; Takes the CIDR prefix and adds these many bits to it for calculating subnet ranges.  MAKE SURE if you change this you also change the CIDR spacing or you may hit errors.  See cidrsubnet interpolation in terraform config for more information.

<a name="public_subnet_cidr_blocks" className="snap-top"></a>

* [**`public_subnet_cidr_blocks`**](#public_subnet_cidr_blocks) &mdash; A map listing the specific CIDR blocks desired for each public subnet. The key must be in the form AZ-0, AZ-1, ... AZ-n where n is the number of Availability Zones. If left blank, we will compute a reasonable CIDR block for each subnet.

<a name="public_subnet_custom_tags" className="snap-top"></a>

* [**`public_subnet_custom_tags`**](#public_subnet_custom_tags) &mdash; A map of tags to apply to the public Subnet, on top of the [`custom_tags`](#custom_tags). The key is the tag name and the value is the tag value. Note that tags defined here will override tags defined as [`custom_tags`](#custom_tags) in case of conflict.

<a name="subnet_spacing" className="snap-top"></a>

* [**`subnet_spacing`**](#subnet_spacing) &mdash; The amount of spacing between the different subnet types

<a name="vpc_name" className="snap-top"></a>

* [**`vpc_name`**](#vpc_name) &mdash; The name of the VPC. Defaults to mgmt.

</TabItem>
<TabItem value="outputs" label="Outputs">

<a name="nat_gateway_public_ips" className="snap-top"></a>

* [**`nat_gateway_public_ips`**](#nat_gateway_public_ips) &mdash; The public IP address(es) of the NAT gateway(s) of the mgmt VPC.

<a name="num_availability_zones" className="snap-top"></a>

* [**`num_availability_zones`**](#num_availability_zones) &mdash; The number of availability zones used by the mgmt VPC.

<a name="private_subnet_arns" className="snap-top"></a>

* [**`private_subnet_arns`**](#private_subnet_arns) &mdash; The private subnet ARNs of the mgmt VPC.

<a name="private_subnet_cidr_blocks" className="snap-top"></a>

* [**`private_subnet_cidr_blocks`**](#private_subnet_cidr_blocks) &mdash; The private subnet CIDR blocks of the mgmt VPC.

<a name="private_subnet_ids" className="snap-top"></a>

* [**`private_subnet_ids`**](#private_subnet_ids) &mdash; The private subnet IDs of the mgmt VPC.

<a name="private_subnet_route_table_ids" className="snap-top"></a>

* [**`private_subnet_route_table_ids`**](#private_subnet_route_table_ids) &mdash; The ID of the private subnet route table of the mgmt VPC.

<a name="public_subnet_arns" className="snap-top"></a>

* [**`public_subnet_arns`**](#public_subnet_arns) &mdash; The public subnet ARNs of the mgmt VPC.

<a name="public_subnet_cidr_blocks" className="snap-top"></a>

* [**`public_subnet_cidr_blocks`**](#public_subnet_cidr_blocks) &mdash; The public subnet CIDR blocks of the mgmt VPC.

<a name="public_subnet_ids" className="snap-top"></a>

* [**`public_subnet_ids`**](#public_subnet_ids) &mdash; The public subnet IDs of the mgmt VPC.

<a name="public_subnet_route_table_id" className="snap-top"></a>

* [**`public_subnet_route_table_id`**](#public_subnet_route_table_id) &mdash; The ID of the public subnet route table of the mgmt VPC.

<a name="vpc_cidr_block" className="snap-top"></a>

* [**`vpc_cidr_block`**](#vpc_cidr_block) &mdash; The CIDR block of the mgmt VPC.

<a name="vpc_id" className="snap-top"></a>

* [**`vpc_id`**](#vpc_id) &mdash; The ID of the mgmt VPC.

<a name="vpc_name" className="snap-top"></a>

* [**`vpc_name`**](#vpc_name) &mdash; The name of the mgmt VPC.

<a name="vpc_ready" className="snap-top"></a>

* [**`vpc_ready`**](#vpc_ready) &mdash; Indicates whether or not the VPC has finished creating

</TabItem>
</Tabs>


<!-- ##DOCS-SOURCER-START
{"sourcePlugin":"service-catalog-api","hash":"6eeb435a6c66a038beb515fc47058640"}
##DOCS-SOURCER-END -->
