Vagrant AWS + Ansible Example
===========

Just an example of how to use vagrant-aws + ansible


Requirements
------------

  - Vagrant (https://www.packer.io/intro/getting-started/install.html)
  - Ansible (http://docs.ansible.com/ansible/intro_installation.html)
  - Ansible roles used in playbook.yml previously installed
  
Installation
------------

    $ git clone https://github.com/erozario/vagrant-aws-example.git
    $ vagrant plugin install vagrant-aws
    $ vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box

Example Vagrantfile
--------------
    Vagrant.configure("2") do |config|

      config.vm.box = "aws-dummy"

      config.vm.provider :aws do |aws, override|
        aws.access_key_id = "XXXXXXXXXXXXXXXXXXXX"
        aws.secret_access_key = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
        aws.keypair_name = "vagrant"

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
        override.ssh.private_key_path = "./vagrant.pem"
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
        - geerlingguy.nginx
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
https://www.linkedin.com/in/eduardo-rozario/
