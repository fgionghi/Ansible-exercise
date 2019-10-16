# Ansible-exercise

  
**How the vagrant process work**:
When ansible is defined as the provisioner in the Vagrantfile it use the mentioned playbook to config the guest machine.

  

So it's vagrant (not ansible) that execute that playbook on guest machines.

When hit `vagrant up` it creates an inventory for ansible:

    ➜ Ansible-exercise git:(master) ✗ cat .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory
    
     # Generated by Vagrant
    
    default ansible_host=127.0.0.1 ansible_port=2200 ansible_user='vagrant' ansible_ssh_private_key_file='/home/giogio/Ansible-exercise/.vagrant/machines/default/virtualbox/private_key'

Expose docker api:

1. Create `daemon.json` file in `/etc/docker`:

        {"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}

2. Add `/etc/systemd/system/docker.service.d/override.conf`

        [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd


3. Reload the systemd daemon:

        systemctl daemon-reload

4. Restart docker:

        systemctl restart docker.service

https://docs.docker.com/engine/security/https/

vagrant scp node1:/home/vagrant/key.pem .

https://gist.github.com/kekru/4e6d49b4290a4eebc7b597c07eaf61f2

docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem \
  -H=$HOST:2376 version

It works, but expose the same port as the guest, not the one specified in the Vagrantfile. 