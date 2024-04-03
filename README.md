# ansible_project
Creating Host Inventory

Since you are going to create a environment specific inventory, create a environments directory and a file inside it called prod

    mkdir environments

Create inventory file

file: environments/prod

Let's create three groups as follows,

    [local]
    localhost ansible_connection=local  
    [lb] 
    lb  
    [app] 
    app1 
    app2   
    [db] 
    db  
    [prod:children] 
    lb 
    app 
    db 

    First group contains the localhost, the control host. Since it does not need to be connected over ssh, it mandates we add ansible_connection=local option

    Second group contains Application Servers. We will add two app servers to this group.

    Third group holds the information about the database servers.

The inventory file should look like below.
Setup passwordless SSH between ansible controller and remote machine

on Host machine

Ansible uses passwordless ssh.

Generate ssh keypair if not present already using the following command.

    ssh-keygen -t rsa
     
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/ubuntu/.ssh/id_rsa.
    Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub.
    The key fingerprint is:
    SHA256:yC4Tl6RYc+saTPcLKFdGlTLOWOIuDgO1my/NrMBnRxA ubuntu@node1
    The key's randomart image is:
    +---[RSA 2048]----+
    |   E    ..       |
    |  . o +..        |
    | . +o*+o         |
    |. .o+Bo+         |
    |. .++.X S        |
    |+ +ooX .         |
    |.=.OB.+ .        |
    | .=o*= . .       |
    |  .o.   .        |
    +----[SHA256]-----+

Just leave the fields to defaults. This command will generate a public key and private key for you.

Copy over the public key to all remote machine.

Example, assuming ubuntu as the user which has a privileged access on the remote machie

    ssh-copy-id devops@lb
    ssh-copy-id devops@app1  
    ssh-copy-id devops@app2  
    ssh-copy-id devops@db

This will copy our newly generated public key to the remote machine. After running this command you will be able to SSH into the machine directly without using a password.

Ansible Ping

We will use Ansible to make sure all the hosts are reachable

    ansible all -m ping

[Output]

    lb | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    app1 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    app2 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }
    db | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }


