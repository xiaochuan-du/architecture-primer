# Infrastructure As Code (IAC)

## Tools

- [Chef](https://www.chef.io/)
- [Puppet](https://puppet.com/)
- [Ansible](https://www.ansible.com/)
- [SaltStack](https://saltstack.com/)
- [CloudFormation](https://aws.amazon.com/cloudformation/)
- [Terraform](https://www.terraform.io/)



### Comparison

#### Configuration Management vs Orchestration

configuration management, install and manage software on existing servers

- Chef
- Puppet
- Ansible
- SaltStack 

orchestration tools: provision the servers themselves.

- CloudFormation
- Terraform 

Using docker or packer makes it less necessary to leverage configuration management tools.

#### Mutable Infrastructure vs Immutable Infrastructure

Immutable Infrastructure (Terraform, cloudformation) is better because it can avoid "configuration drift"



#### Procedural vs Declarative

Declarative (Terraform, CloudFormation, SaltStack, and Puppet) method highlights the final conditions

When dealing with procedural code

- the state of the infrastructure is *not* fully captured in the code, eg order
- The reusability of procedural code is inherently limited because you have to manually to take into account the current state of the codebase.

Declarative:

- creating generic, reusable code can be tricky 
- expressive power is limited

**Terraform** uses cloud provider APIs to provision infrastructure, so there are no new authentication mechanisms beyond what you’re using with the cloud provider already, and there is no need for direct access to your servers. We found this as the best option in terms of ease-of-use, security, and maintainability.

Ansible over ssh, and cloudformation uses hosted server.



#### Conclusion

Teeraform is better since it is cloud-agnostic. [Tutorial](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c)



![](https://cdn-images-1.medium.com/max/1000/1*bVC97LGrOY4R5E4WwzMLzw.png)