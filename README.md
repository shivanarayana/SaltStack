# SaltStack
StepByStep Process for SaltStack


https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/ubuntu.html
https://docs.saltproject.io/salt/install-guide/en/latest/topics/configure-master-minion.html#configure-master-minion

**Step 1**
Create an EC2 instance:
Launch instance
Ubuntu
Generate key pair-SSH .PEM file (.ppk for putty)

security groups
– Add inbound rules
– Add new rule
– 4505-4506 PORTS
– 0/0.0.0.0

Click on the instance and Connect

**Step 2**

Install keys from doc
https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/ubuntu.html
From this doc run the below commands:

sudo curl -fsSL -o /etc/apt/keyrings/salt-archive-keyring-2023.gpg https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/SALT-PROJECT-GPG-PUBKEY-2023.gpg
echo "deb [signed-by=/etc/apt/keyrings/salt-archive-keyring-2023.gpg arch=amd64] https://repo.saltproject.io/salt/py3/ubuntu/22.04/amd64/latest jammy main" | sudo tee /etc/apt/sources.list.d/salt.list


**Step 3**

Install master and minions

sudo apt-get install salt-master
sudo apt-get install salt-minion

sudo apt-get install salt-ssh
sudo apt-get install salt-syndic
sudo apt-get install salt-cloud
sudo apt-get install salt-api


**Step 4**
Start services

root@ip-172-31-46-51:/home/ubuntu# sudo systemctl enable salt-master && sudo systemctl start salt-master
root@ip-172-31-46-51:/home/ubuntu# sudo systemctl enable salt-minion && sudo systemctl start salt-minion
root@ip-172-31-46-51:/home/ubuntu# sudo systemctl enable salt-syndic && sudo systemctl start salt-syndic
root@ip-172-31-46-51:/home/ubuntu# sudo systemctl enable salt-api && sudo systemctl start salt-api

**Step 5**
Check salt:
Whereis salt

**Step 6**
Know your ip address

ifconfig

**Step 7**
Go to  /etc/salt

**Step 8**
Edit the files take reference from the https://docs.saltproject.io/salt/install-guide/en/latest/topics/configure-master-minion.html

root@ip-172-31-46-51:/etc/salt# sudo nano master.d/network.conf
root@ip-172-31-46-51:/etc/salt# sudo nano master.d/thread_options.conf
root@ip-172-31-46-51:/etc/salt# sudo nano minion.d/master.conf
root@ip-172-31-46-51:/etc/salt# sudo nano minion.d/id.conf

**Step 9**
sudo systemctl restart salt-master
sudo systemctl restart salt-minion

**Step 10**
root@ip-172-31-46-51:/etc/salt# netstat -aon | grep 450
tcp        0      0 0.0.0.0:4505            0.0.0.0:*               LISTEN      off (0.00/0/0)
tcp        0      0 0.0.0.0:4506            0.0.0.0:*               LISTEN      off (0.00/0/0)

**Step 11**
ubuntu@ip-172-31-33-62:/etc/salt$ sudo salt-key

If there no keys present then do step 9 to see the keys

**Step 12**
root@ip-172-31-46-51:/# sudo salt --version
salt 3006.1

**Step 13**
ubuntu@ip-172-31-33-62:/etc/salt$ sudo salt-key

Check for unaccepted keys

**Step 14**

Accept the unaccepted keys:
ubuntu@ip-172-31-33-62:/etc/salt$ sudo salt-key -a rebel_1

Check the status of master
ubuntu@ip-172-31-33-62:/etc/salt$ systemctl status salt-master

Check the status if the minion
ubuntu@ip-172-31-33-62:/etc/salt$ systemctl status salt-minion


ubuntu@ip-172-31-33-62:/etc/salt$ sudo salt '*' test.version

ubuntu@ip-172-31-33-62:/etc/salt$ sudo salt '*' test.ping

ubuntu@ip-172-31-33-62:/etc/salt$ sudo mkdir /srv/salt

To Debug
ubuntu@ip-172-31-10-151:~$ sudo salt-minion -l debug

**Step 15**

Create a new ec2 minion instance

Follow Step 1- to create EC2 instance
Follow Step 2- to install keys from repo
Follow Step 3- but install only minion - “sudo apt-get install salt-minion”
Follow Step 4- but start only minion “sudo systemctl enable salt-minion && sudo systemctl start salt-minion”
Follow Step 8- but edit only two documents master.conf and id.conf
root@ip-172-31-46-51:/etc/salt# sudo nano minion.d/master.conf
Give public ip of master - Note: PUBLIC IP only
root@ip-172-31-46-51:/etc/salt# sudo nano minion.d/id.conf
name the minion
Follow Step 9- to restart the minions

Goto the Master instance
Follow Step 9- to restart the Master
Follow Step 13- to check for keys
Follow Step 14- to accept the keys

**Step 16**

Commands to retrieves and displays the contents

ubuntu@ip-172-31-33-62:~$ sudo salt '*' cmd.run 'ls -lah /home'
ubuntu@ip-172-31-33-62:~$ sudo salt '*' cmd.run 'uptime'
ubuntu@ip-172-31-33-62:~$ cat /etc/passwd
ubuntu@ip-172-31-33-62:~$ sudo salt '*' cmd.run 'cat /etc/passwd'

**Step 17**

Create a user

ubuntu@ip-172-31-33-62:~$ sudo salt '*' user.add 'testuser01'
rebel_2:
    True
rebel_1:
    True

Now check if user is created with 

ubuntu@ip-172-31-33-62:~$ sudo salt '*' cmd.run 'cat /etc/passwd'

Login to the user
ubuntu@ip-172-31-33-62:~$ sudo su testuser01

Type exit to exit from the user

**Step 18**
Delete the user
ubuntu@ip-172-31-33-62:~$ sudo salt '*' user.delete testuser01 remove=true force=true
rebel_2:
    True
rebel_1:
    True


**Step 19**

Set file roots
https://docs.saltproject.io/en/latest/topics/ssh/index.html
