# Education AMIs

This folder contains various Cloud Init config files for generating the AMI images
that we use. Generally speaking, you'll simply add these to a base image and then
create an AMI from that.

## Building an AMI

1. Start with the standard CentOS 7 image.
    * https://aws.amazon.com/marketplace/pp/B00O7WM7QW
1. Configure appropriately and start it up.
    * Choose reasonable defaults for disk, etc.
    * Tag the instance following company standards.
        * https://confluence.puppetlabs.com/display/SRE/Amazon+Cloud+Management+Standards+-+Tags+and+Tagging
    * If this running instance will be used to generate a new standard AMI for virtual classroom use, the only required tag at this time is `termination_date` with a sufficiently long lifetime. Once the new AMI is created and launched by `vcmanager` in the future, all other required tags will be added as specified in the SRE document.
    * Generally you'll want to use the `training` keypair.
1. SSH to the new node.
    * `ssh -i ~/.ssh/training.pem centos@[address]`
1. Become root with `sudo su`
1. Configure Cloud Init
    * Install one or more `.cfg` files to `/etc/cloud/cloud.cfg.d/` (including https://github.com/puppetlabs/education-builds/blob/master/cloud/aws/ami/99_provisioning.cfg if generating a new virtual classroom AMI - see below)
1. Perform other customizations as needed, e.g. `yum update`, etc.
1. Right click in the AWS Console and create an image.
1. Please name it descriptively.

## Cloud Init configs

* `99_provisioning.cfg`
    * This script configures an AMI to self-provision on first bootup.
    * The result will be an instance ready to deliver a virtual classroom training
    * https://confluence.puppetlabs.com/display/EDU/Self-provisioned+training+VMs
