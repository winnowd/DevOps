Step 1 :  Create Docker machines (to act as nodes for Docker Swarm)   Create one machine as manager and others as workers
    docker-machine create --driver hyperv manager1    docker-machine create --driver virtualbox manager1

   docker-machine:Error with pre-create check: “exit status 126”
   https://stackoverflow.com/questions/3...
   brew cask install virtualbox;

   Create one manager machine
   and other worker machines

Step 2 :  Check machine created successfully
    docker-machine ls
    docker-machine ip manager1

Step 3 :  SSH (connect) to docker machine
    docker-machine ssh manager1

Step 4 :  Initialize Docker Swarm    docker swarm init --advertise-addr MANAGER_IP
    docker node ls
    (this command will work only in swarm manager and not in worker)

Step 5 :  Join workers in the swarm
    Get command for joining as worker
    In manager node run command
    docker swarm join-token worker
    This will give command to join swarm as worker

    docker swarm join-token manager
    This will give command to join swarm as manager

    SSH into worker node (machine) and run command to join swarm as worker
   
    In Manager Run command - docker node ls to verify worker is registered and is ready
  
    Do this for all worker machines

Step 6 :  On manager run standard docker commands
    docker info
    check the swarm section 
    no of manager, nodes etc

    Now check docker swarm command options 
    docker swarm 

Step 7 :  Run containers on Docker Swarm
    docker service create --replicas 3 -p 80:80 --name serviceName nginx
    Check the status:
    docker service ls
    docker service ps serviceName
    Check the service running on all nodes
    Check on the browser by giving ip for all nodes

Step 8 :  Scale service up and down
   On manager node 
   docker service scale serviceName=2

Inspecting Nodes (this command can run only on manager node)
docker node inspect nodename
docker node inspect self
docker node inspect worker1

Step 9 : Shutdown node
   docker node update --availability drain worker1

Step 10 :  Update service
   docker service update --image imagename:version web
   docker service update --image nginx:1.14.0 serviceName

Step 11 :  Remove service
   docker service rm serviceName
docker swarm leave : to leave the swarm
docker-machine stop machineName : to stop the machine
docker-machine rm machineName : to remove the machine
