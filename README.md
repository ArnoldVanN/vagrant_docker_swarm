# Setting up a docker swarm using vagrant

Clone the repo and run the VM's using ```vagrant up```

SSH into manager01 and initialize the swarm using ```docker swarm init --listen-addr 172.20.20.11:2377 --advertise-addr 172.20.20.11:2377```
From the output, copy the command and token used to join the swarm as a worker and save it for later.

Run ```docker swarm join-token manager``` to get the token to join the manager and save the output once again.

Next, to add the second manager, SSH into it and run the first command you copied from manager01 to join as a manager node.

To add the worker nodes, log into them and run the second command you copied.

To verify the swarm, SSH back into manager01 and run ```docker node ls```
The output should look like this.
![docker node ls](https://imgur.com/a/t5c3baG.jpg)
