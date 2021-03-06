Vagrant AWS Plugin and Ansible
===========

Just an example of how to use vagrant-aws and ansible


Requirements
------------

  - [Vagrant](https://www.packer.io/intro/getting-started/install.html)
  - [Ansible](http://docs.ansible.com/ansible/intro_installation.html)
  - [Ansible Roles](https://galaxy.ansible.com/)
  
Installation
------------

    $ git clone https://github.com/erozario/vagrant-aws-example.git
    $ vagrant plugin install vagrant-aws
    $ vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
    
Configure AWS credentials
--------------

Create a file called ‘aws-credentials’ with following content:

    export AWS_KEY='your-key'
    export AWS_SECRET='your-secret'
    export AWS_KEYNAME='your-keyname'
    export AWS_KEYPATH='your-keypath'
    
Write credentials into Environment variables
    
    $ source aws-credentials

Example Vagrantfile
--------------
    Vagrant.configure("2") do |config|

      config.vm.box = "dummy"

      config.vm.provider :aws do |aws, override|
        aws.access_key_id = ENV['AWS_KEY']
        aws.secret_access_key = ENV['AWS_SECRET']
        aws.keypair_name = ENV['AWS_KEYNAME']
        aws.ami = "ami-ae7bfdb8"
        aws.region = "us-east-1"
        aws.availability_zone = "us-east-1e"
        aws.instance_type = "t2.micro"
        aws.security_groups = "sg-123abc1a"
        aws.subnet_id = "subnet-123abcd12"
        aws.associate_public_ip = true

        aws.tags = {
          'Name'    => 'grafana'
        }

        override.ssh.username = "centos"
        override.ssh.private_key_path = ENV['AWS_KEYPATH']
      end

      config.vm.provision :ansible do |ansible|
        ansible.playbook = "playbook.yml"
      end

    end


Example Playbook
----------------

    - hosts: all
      become: yes
      roles:
        - erozario.grafana 
      vars:
        grafana_admin_user: admin
        grafana_admin_password: grafana
        grafana_plugins_install: [alexanderzobnin-zabbix-app]

Execute
----------------
 
    $ vagrant up --provider aws 

License
-------

MIT

Author Information
------------------
[Linkedin](https://www.linkedin.com/in/eduardo-rozario/)
