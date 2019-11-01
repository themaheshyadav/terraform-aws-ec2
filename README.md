<!-- This file was automatically generated by the `geine`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS EC2
</h1>

<p align="center" style="font-size: 1.2rem;">
    Terraform module to create an EC2 resource on AWS with Elastic IP Addresses and Elastic Block Store.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v0.12-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+EC2&url=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+EC2&url=https://github.com/clouddrove/terraform-aws-ec2'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>


We eat, drink, sleep and most importantly love **DevOps**. We are working towards stratergies for standardizing architecture while ensuring security for the infrastructure. We are strong believer of the philosophy <b>Bigger problems are always solved by breaking them into smaller manageable problems</b>. Resonating with microservices architecture, it is considered best-practice to run database, cluster, storage in smaller <b>connected yet manageable pieces</b> within the infrastructure.

This module is basically combination of [Terraform open source](https://www.terraform.io/) and includes automatation tests and examples. It also helps to create and improve your infrastructure with minimalistic code instead of maintaining the whole infrastructure code yourself.

We have [*fifty plus terraform modules*][terraform_modules]. A few of them are comepleted and are available for open source usage while a few others are in progress.




## Prerequisites

This module has a few dependencies:

- [Terraform 0.12](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)







## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/clouddrove/terraform-aws-ec2/releases).


### Simple Example
Here is an example of how you can use this module in your inventory structure:
```hcl
    module "ec2" {
      source                      = "git::https://github.com/clouddrove/terraform-aws-ec2.git?ref=tags/0.12.2"
      name                        = "ec2-instance"
      application                 = "clouddrove"
      environment                 = "test"
      label_order                 = ["environment", "name", "application"]
      instance_count              = 2
      ami                         = "ami-08d658f84a6d84a80"
      ebs_optimized               = false
      instance_type               = "t2.nano"
      key_name                    = module.keypair.name
      monitoring                  = false
      associate_public_ip_address = true
      tenancy                     = "default"
      disk_size                   = 8
      vpc_security_group_ids_list = [module.ssh.security_group_ids, module.http-https.security_group_ids]
      subnet_ids                  = tolist(module.public_subnets.public_subnet_id)
      assign_eip_address          = true
      ebs_volume_enabled          = true
      ebs_volume_type             = "gp2"
      ebs_volume_size             = 30
      user_data                   = "./_bin/user_data.sh"
      instance_tags               = {"snapshot"=true}
    }
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| ami | The AMI to use for the instance. | string | - | yes |
| application | Application (e.g. `cd` or `clouddrove`). | string | `` | no |
| assign_eip_address | Assign an Elastic IP address to the instance. | bool | `false` | no |
| associate_public_ip_address | Associate a public IP address with the instance. | bool | `true` | no |
| attributes | Additional attributes (e.g. `1`). | list | `<list>` | no |
| availability_zone | Availability Zone the instance is launched in. If not set, will be launched in the first AZ of the region. | list | `<list>` | no |
| cpu_core_count | Sets the number of CPU cores for an instance. | string | `` | no |
| cpu_credits | The credit option for CPU usage. Can be `standard` or `unlimited`. T3 instances are launched as unlimited by default. T2 instances are launched as standard by default. | string | `standard` | no |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | string | `-` | no |
| disable_api_termination | If true, enables EC2 Instance Termination Protection. | bool | `false` | no |
| disk_size | Size of the root volume in gigabytes. | number | `8` | no |
| dns_enabled | Flag to control the dns_enable. | bool | `false` | no |
| dns_zone_id | The Zone ID of Route53. | string | `` | no |
| ebs_block_device | Additional EBS block devices to attach to the instance. | list | `<list>` | no |
| ebs_device_name | Name of the EBS device to mount. | list(string) | `<list>` | no |
| ebs_iops | Amount of provisioned IOPS. This must be set with a volume_type of io1. | number | `0` | no |
| ebs_optimized | If true, the launched EC2 instance will be EBS-optimized. | bool | `false` | no |
| ebs_volume_enabled | Flag to control the ebs creation. | bool | `false` | no |
| ebs_volume_size | Size of the EBS volume in gigabytes. | number | `30` | no |
| ebs_volume_type | The type of EBS volume. Can be standard, gp2 or io1. | string | `gp2` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | string | `` | no |
| ephemeral_block_device | Customize Ephemeral (also known as Instance Store) volumes on the instance. | list | `<list>` | no |
| host_id | The Id of a dedicated host that the instance will be assigned to. Use when an instance is to be launched on a specific dedicated host. | string | `` | no |
| hostname | DNS records to create. | string | `` | no |
| iam_instance_profile | The IAM Instance Profile to launch the instance with. Specified as the name of the Instance Profile. | string | `` | no |
| instance_count | Number of instances to launch. | number | `1` | no |
| instance_enabled | Flag to control the instance creation. | bool | `true` | no |
| instance_initiated_shutdown_behavior | Shutdown behavior for the instance. | string | `` | no |
| instance_profile_enabled | Flag to control the instance profile creation. | bool | `false` | no |
| instance_tags | Instance tags. | map | `<map>` | no |
| instance_type | The type of instance to start. Updates to this field will trigger a stop/start of the EC2 instance. | string | - | yes |
| ipv6_address_count | Number of IPv6 addresses to associate with the primary network interface. Amazon EC2 chooses the IPv6 addresses from the range of your subnet. | number | `0` | no |
| ipv6_addresses | List of IPv6 addresses from the range of the subnet to associate with the primary network interface. | list | `<list>` | no |
| key_name | The key name to use for the instance. | string | `` | no |
| label_order | Label order, e.g. `name`,`application`. | list | `<list>` | no |
| monitoring | If true, the launched EC2 instance will have detailed monitoring enabled. (Available since v0.6.0). | bool | `false` | no |
| name | Name  (e.g. `app` or `cluster`). | string | `` | no |
| network_interface | Customize network interfaces to be attached at instance boot time. | list(map(string)) | `<list>` | no |
| placement_group | The Placement Group to start the instance in. | string | `` | no |
| root_block_device | Customize details about the root block device of the instance. See Block Devices below for details. | list | `<list>` | no |
| source_dest_check | Controls if traffic is routed to the instance when the destination address does not match the instance. Used for NAT or VPNs. | bool | `true` | no |
| subnet | VPC Subnet ID the instance is launched in. | string | `` | no |
| subnet_ids | A list of VPC Subnet IDs to launch in. | list(string) | `<list>` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | map | `<map>` | no |
| tenancy | The tenancy of the instance (if the instance is running in a VPC). An instance with a tenancy of dedicated runs on single-tenant hardware. The host tenancy is not supported for the import-instance command. | string | `` | no |
| ttl | The TTL of the record to add to the DNS zone to complete certificate validation. | string | `300` | no |
| type | Type of DNS records to create. | string | `CNAME` | no |
| user_data | The Base64-encoded user data to provide when launching the instances. | string | `` | no |
| vpc_security_group_ids_list | A list of security group IDs to associate with. | list(string) | `<list>` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The ARN of the instance. |
| az | The availability zone of the instance. |
| instance_count | The count of instances. |
| instance_id | The instance ID. |
| ipv6_addresses | A list of assigned IPv6 addresses. |
| key_name | The key name of the instance. |
| placement_group | The placement group of the instance. |
| private_ip | Private IP of instance. |
| public_ip | Public IP of instance (or EIP). |
| subnet_id | The EC2 subnet ID. |
| vpc_security_group_ids | The associated security groups in non-default VPC. |




## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system.

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```



## Feedback
If you come accross a bug or have any feedback, please log it in our [issue tracker](https://github.com/clouddrove/terraform-aws-ec2/issues), or feel free to drop us an email at [hello@clouddrove.com](mailto:hello@clouddrove.com).

If you have found it worth your time, go ahead and give us a ★ on [our GitHub](https://github.com/clouddrove/terraform-aws-ec2)!

## About us

At [CloudDrove][website], we offer expert guidance, implementation support and services to help organisations accelerate their journey to the cloud. Our services include docker and container orchestration, cloud migration and adoption, infrastructure automation, application modernisation and remediation, and performance engineering.

<p align="center">We are <b> The Cloud Experts!</b></p>
<hr />
<p align="center">We ❤️  <a href="https://github.com/clouddrove">Open Source</a> and you can check out <a href="https://github.com/clouddrove">our other modules</a> to get help with your new Cloud ideas.</p>

  [website]: https://clouddrove.com
  [github]: https://github.com/clouddrove
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://twitter.com/clouddrove/
  [email]: https://clouddrove.com/contact-us.html
  [terraform_modules]: https://github.com/clouddrove?utf8=%E2%9C%93&q=terraform-&type=&language=