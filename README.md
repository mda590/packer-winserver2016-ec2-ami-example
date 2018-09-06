# Example of building a Windows Server 2016 AMI for EC2 using Packer

Packer makes building Linux AMIs extremely easy - it uses standard SSH under the hood to interact with a server throughout the build process and can seamlessly copy files, execute commands, etc. Windows, on the other hand, is a bit of a pain to get right with Packer, especially on AWS. You have to make sure you have everything configured just right, or things will not work and you'll be left wondering why.

The code in this repository is meant to give an example of a basic Windows Server 2016 AMI build for use on EC2 instances in AWS.

## Details

* server2016_template.json - this is actual Packer template JSON which will define what Packer should do when actually building out an AMI.
  * Note within this template that the last Provisioner step is to run specific EC2Launch scripts to SysPrep the instance. These scripts are available on all Windows Server 2016 AMIs provided by AWS, but EC2Launch is out of the scope of this example.
* user_data.ps1 - this will run natively within the EC2 instance on first boot, exactly the same as if you defined UserData for an EC2 instance in the console, CloudFormation, or Terraform. This will be responsible for configuring WinRM and Windows Firewall to allow Packer to actually connect in to the EC2 instance and run the remaining steps in the process.
* bootstrap.ps1 - this is an example PowerShell script which can be used in a Provisioner step of the Packer AMI build process. In order for this to work, you must first have successfully enabled and configured WinRM security via UserData execution.

## Gotchas

* Make sure that the proper ports are open between your network / computer and the EC2 instance which Packer is building to create your AMI. Specifically, you will need to make sure that ports required by WinRM are available.

## Resources

There are a few different sites and resources I came across which helped me figure this out initially. They are listed here in no particular order:

* [Getting Packer to work for Windows on AWS](http://blog.petegoo.com/2016/05/10/packer-aws-windows/)
* [Packer Getting Started: A Windows Example](https://www.packer.io/intro/getting-started/build-image.html#a-windows-example)
* [Building images on AWS with Packer - the new way](https://david-obrien.net/2016/12/packer-and-aws-windows-server-2016/)
