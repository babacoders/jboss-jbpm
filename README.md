# jboss-jbpm

jBPM refers to Java Business Process Model). It is an open-source workflow engine written in Java that can execute business processes described in BPMN 2.0. It is released under the ASL by the JBoss company.

In essence jBPM takes graphical process descriptions as input. A process is composed of tasks that are connected with sequence flows. Processes represent an execution flow. The graphical diagram (flow chart) of a process is used as the basis for the communication between non-technical users and developers.Each execution of a process definition is called a "process instance". jBPM manages the process instances. Some activities are automatic like sending an e-mail or invoking a service. Some activities act as wait states, like for example human tasks or waiting for an external service to return results. jBPM will manage and persist the state of the process instances at all times.

jBPM is based on the Process Virtual Machine (PVM) which is the JBoss community's foundation to support multiple process languages natively. The JBoss community currently focuses on using the BPMN 2.0 specification for defining business processes.
jBPM also provides various tools, both for developers (Eclipse) and end users (web-based) to create, deploy, execute and manage business processes throughout their life cycle.

# Tested Infrastructure


* Docker Compose (1.17.1)
* Docker 17.11

## Install Docker Compose 

    curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

## Cloning this Repository


    git clone https://github.com/babacoders/jboss-jbpm/
    cd jboss-jbpm

## Building Microservices using Docker Compose

    docker-compose build
   
   
## Running the Microservices

    docker-compose up -d
   
   
## Verify that Microservices are up and running

    docker-compose ps
  
This will show up the below output:

      Name                 Command               State   Ports
   
      kie-server   ./start_jbpm-wb.sh               Up           
      postgresdb   /usr/lib/postgresql/9.3/bi ...   Up  
   
 [Ensure that you run the above command from a directory where you ran docker-compose up]
 
 ## Get ready to open up the WebUI
 
       *  http://<IP>:8080/jbpm-console
 
 Username: admin 
 Password: admin
 
 ## Let's verify the data volume
 
     docker volume ls

    local               myproject_testvols
    
It will show myproject_testvols instead of testvols because I ran it from myproject directory(in my case).

## Verify the persistent data volume

    root@ubuntu1:~/myproject# docker volume inspect myproject_testvols
    [
      {
        "CreatedAt": "2017-12-02T06:36:14Z",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "myproject",
            "com.docker.compose.volume": "testvols"
        },
        "Mountpoint": "/var/lib/docker/volumes/myproject_testvols/_data",
        "Name": "myproject_testvols",
        "Options": {},
        "Scope": "local"
    }
    
    
 It shows that the volume gets mounted at /var/lib/docker/volumes/myproject_testvols/_data
 
 Let's go to this particular directory in the host machine:
 
    root@ubuntu1:/var/lib/docker/volumes/myproject_testvols/_data# tree 9.3|head -n 10
     9.3
      └── main
         ├── PG_VERSION
         ├── base
         │   ├── 1
         │   │   ├── 11765
         │   │   ├── 11765_fsm
         │   │   ├── 11765_vm
         │   │   ├── 11767
         │   │   ├── 11767_fsm
  
  
  Note:
  
  1. While running on Google Cloud Platform, ensure that 8080 port is open
  You can open up all the ports(not recommended for production) using the below command:
  
         gcloud compute firewall-rules create allow-all --allow all  --source-tags=ubuntu1  --source-ranges=0.0.0.0/0 --d
        escription="Be Free"                
