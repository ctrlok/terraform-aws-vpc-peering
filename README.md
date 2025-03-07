AWS VPC Peering Connection Module
=================================

| Branch           | Build Status                                                                                                                                                |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Master**       | [![Build Status](https://travis-ci.org/grem11n/terraform-aws-vpc-peering.svg?branch=master)](https://travis-ci.org/grem11n/terraform-aws-vpc-peering)       |
| **terraform011** | [![Build Status](https://travis-ci.org/grem11n/terraform-aws-vpc-peering.svg?branch=terraform011)](https://travis-ci.org/grem11n/terraform-aws-vpc-peering) |

Terraform module, which creates a peering connection between two VPCs and adds routes to the local VPC.
Routes on the Peer VPC side should be configured separately.

This module is designed to work with [VPC](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/) module from the Terraform Registry

Preamble
----
**Always make sure you pinned the module version!**
Please, be aware that any new code in `master` may intorduce some regressions. Moreover, sometimes I can easily miss some of them because I personally doen't use all the VPC peering features on daily basis.

### Terraform 12
Current `master` has the code, which works with Terraform 0.11.

However, [Terraform 0.12](https://www.hashicorp.com/blog/announcing-terraform-0-12) is here. Thus, I'm going to update this module to work with it. All existing work would be migrated to the `terraform011` branch.

Changelog
----
Changelog is in the [CHANGELOG.md](CHANGELOG.md)


Note
----

These types of resources are supported:

* [Peering Connection](https://www.terraform.io/docs/providers/aws/d/vpc_peering_connection.html)
* [AWS Route](https://www.terraform.io/docs/providers/aws/r/route.html)
* [AWS VPC Peering Connection Accepter](https://www.terraform.io/docs/providers/aws/r/vpc_peering_accepter.html)
* [AWS VPC Peering Connection Options](https://www.terraform.io/docs/providers/aws/r/vpc_peering_options.html)

Usage
-----

### Examples
Sample configuration is located in [examples](examples/) directory. There are not many of them right now, but I'll add more soon.

### Single Region Peering
**Notice**: You need to declare both providers even with single region peering.

```hc1
module "vpc_single_region_peering" {
  source = "./terraform-aws-vpc-peering"

  providers = {
    aws.this = "aws"
    aws.peer = "aws"
  }

  this_vpc_id             = "vpc-00000000"
  peer_vpc_id             = "vpc-11111111"
  auto_accept_peering     = true

  tags = {
    Name        = "my-peering-connection"
    Environment = "prod"
  }
}
```

Usage with already created peering connection:
```hc1
module "vpc_single_region_peering" {
  source = "./terraform-aws-vpc-peering"

  providers = {
    aws.this = "aws"
    aws.peer = "aws"
  }

  this_vpc_id             = "vpc-00000000"
  peer_vpc_id             = "vpc-11111111"
  auto_accept_peering     = true
}
```

### Cross Region Peering

```hc1
module "vpc_cross_region_peering" {
  source = "github.com/grem11n/terraform-aws-vpc-peering?ref=cross-region-peering"

  providers = {
    aws.this = "aws.src"
    aws.peer = "aws.dst"
  }

  this_vpc_id             = "vpc-00000000"
  peer_vpc_id             = "vpc-11111111"
  auto_accept_peering     = true

  tags = {
    Name        = "my-peering-connection"
    Environment = "prod"
  }
}
```

### Cross Account Peering
In order to make a cross-account peering connection, you must setup both `owner` and `peer` providers accordingly. Also, you have to provide a valid ID of the peer account. Example:

```hc1
providers = {
  aws.this = "main" // Alias to the main AWS account
  aws.peer = "peer" // Alias to the peer AWS account
}

```

Examples
--------
Complete example is shown above

Authors
-------
Module managed by [Yurii Rochniak](https://github.com/grem11n)

License
-------
Apache 2 Licensed. See LICENSE for full details.
