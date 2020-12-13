## To start new Vagrant Env
```sh
    vagrant init <boxname>
```
example 
```sh
    vagrant init centos/7
```
This will place a Vagrantfile in your current/working directory. Vagrant uses Vagrantfile to understand your configuration for env.

## To start VM 
```bash 
    vagrant up 
```
Run this command from the same directory where you have done the 
vagrant init

if you want to start specfic machine use command 
```bash
    vagrant up <machine name configured in Vagrant file>
```
sample vagrant file 
```ruby
    # -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"

  
  config.vm.define "pragra-ansible" do |pa|
    pa.vm.hostname = "ansible-master"
    pa.vm.network "private_network", ip: "192.168.56.101"
    pa.vm.synced_folder "./ansible-bootcamp-code", "/app-data"
    config.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "2048"]
       vb.customize ["modifyvm", :id, "--cpus", "2"]
    end  
  end
  config.vm.define "pragra-app1" do |app1|
    app1.vm.hostname = "app1"
    app1.vm.network "private_network", ip: "192.168.56.102"
    #app1.vm.network "forwarded_port" , guest:8080 , host: 5001 
    config.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "512"]
    end  
  end
  config.vm.define "pragra-app2" do |app2|
    app2.vm.hostname = "app2"
    app2.vm.network "private_network", ip: "192.168.56.202"
    #app2.vm.network "forwarded_port" , guest:8080 , host: 5002 
    config.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "512"]
    end  
  end
  config.vm.define "pragra-app3" do |app3|
    app3.vm.hostname = "app3"
    app3.vm.network "private_network", ip: "192.168.56.201"
    #app2.vm.network "forwarded_port" , guest:8080 , host: 5002 
    config.vm.provider :virtualbox do |vb|
       vb.customize ["modifyvm", :id, "--memory", "512"]
    end  
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update -y 
    apt-get install -y python-minimal
  SHELL
end

```

## Check status of machines 
```sh
    vagrant status 
```

## Save machine with current work 
```sh 
    vagrant suspend 
```
or 
```sh
    vagrant suspend <machine name>
    ex - vagrant suspend pragra-app1
```

## Resume machine back to execution 
```sh
vagrant resume  <machine names seprated by space>
```
example 
```sh
vagrant resume 
# resumes all the mahiness
or 
vagrant resume pragra-app1
# resumes pragra-app1 machine
or 
vagrant resume pragra-app1 pragra-app2 
# resumes pragra-app1 and pragra-app2
```

## Power off machine 
```sh vagrant halt <machine names speated by spance>
# example 
vagrant halt
#or 
vagrant halt pagra-app1
```

## If you change config in Vagrant file , you must used
```sh 
vagrant reload 
# reload === restart machine
```

## Login /SSH machine 
```sh 
vagrant ssh  <Machine name>
# if you are running a single machine you can do
vagrant ssh 
# example 
vagrant ssh pragra-app1
```
## Destoying the vagrant enviroment
```sh
vagrant destroy 
```
