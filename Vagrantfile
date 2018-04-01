Vagrant.configure("2") do |config|

  config.vm.box = "dummy"

  config.vm.provider :aws do |aws, override|
    aws.access_key_id = "YOUR_ACCES_KEY"
    aws.secret_access_key = "YOUR_SECRET_ACCES_KEY"
    aws.keypair_name = "NAME_KEYPAR"

    aws.ami = "AMI_ID"
    aws.region = "AWS_REGION"
    aws.availability_zone = "AWS_AZ"
    aws.instance_type = "AWS_INSTANCE_TYPE"
    aws.security_groups = "SG_ID"
    aws.subnet_id = "SUBNET_ID"
    aws.associate_public_ip = true


    aws.tags = {
      'Name'    => ''
    }

    override.ssh.username = "SSH_USER"
    override.ssh.private_key_path = "PATH_KEYPAR"
  end

  config.vm.provision :ansible do |ansible|
	  ansible.playbook = "ANSIBLE_PLAYBOOK"
  end

end
