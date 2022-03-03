# Setting up a docker swarm using vagrant

Clone the repo and run the VM's using ```vagrant up```

SSH into manager01 and initialize the swarm using ```docker swarm init --listen-addr 172.20.20.11:2377 --advertise-addr 172.20.20.11:2377```
From the output, copy the command and token used to join the swarm as a worker and save it for later.

Run ```docker swarm join-token manager``` to get the token to join the manager and save the output once again.

Next, to add the second manager, SSH into it and run the first command you copied from manager01 to join as a manager node.

To add the worker nodes, log into them and run the second command you copied.

To verify the swarm, SSH back into manager01 and run ```docker node ls```
The output should look like this.
![docker node ls](https://i.imgur.com/6sHL70C.png)

# Running a stack on the swarm
As an example we'll use the project in ./node-project.

We're going to deploy the stack based on the description in the compose file.
On one of the manager nodes, run the command ```docker stack deploy nodeapp -c docker-compose.yml```

Then we'll scale it relative to the number of VM's we set up with out Vagrantfile. In this case 4.
```docker service scale nodeapp_web=4```

To verify, feel free to execute the command ```docker service ls``` and ```docker service ps nodeapp_web```.

Now if you look up any of the ip addresses of the VM's we made in your browser you should see the message ```Hello from Swarm 6948c3d68681```.
Refresh a few times and this (container) id should change.

Optionally, if you'd like a better visualization run ```docker service create \
-p 8080:8080 \
--constraint=node.role==manager \
--mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
dockersamples/visualizer```
This will create a service based on the visualizer image. A handy tool to view your swarm and services running on it.

After navigating to the ip the visualizer is running on you should see something like this: 
![docker visualizer](https://imgur.com/a/mUAFL0C.png)
