![tomcat](spr/playbooks/tomcat.png)
# documentation on how to run and where to start.

To start Tomcat10 with jdk12 the only requirment is docker. all the other software runs isolated inside container.

#### Note: regarding Step2 

Task 6. Then run Tomcat for start website from package ImportantProject.war and start it as a service on cli1.local  

no idea which ImportantProject.war file you have in mind, but the manager-gui is configured with the same credentials given in the task only the password laks the & signe.  
Have a look at git log to see why. feel free to deploy it from gui.

another difficulty i had was the task 7.  
Task 7. The site must be available on 8080 port from other systems on your network.  
But in step3 task 3  
task 3. Make sure the port 8080 is mapped to the port 8090 on host  

so i decided to bind these ports:
* cli1 8090
* cli2 8091
* cli3 8092

## quick start:
Asuming docker-compose and ansible are installed also, ssh key authentication to local ip should work too, run:

```
git clone https://github.com/DNajjarzade/Tomcat.git SamanPR &&\
cd SamanPR
```

edit spr/playbooks/inventory and change ans.local ip address

```
docker-compose -f local.yml build &&\
docker-compose up -d &&\
cd ./spr/playbooks/ &&\
ansible-playbook -i inventory playbook.yml --extra-vars "ansible_become_pass=DockerHostPass" 
```

### visit DockerHostIP:8090

# under the hood:
## install docker-compose
I prefer to run docker-compose as a container so one thing less to install on the docker system. 

```
sudo curl -L --fail https://github.com/docker/compose/releases/download/1.25.4/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## install ansible-silo
I also prefer not install ansible a second thing less on the docker server. install ansible-silo as ansible replacement:

```
docker run --interactive --tty --rm --volume "/usr/local/bin:/silo_install_path" grpn/ansible-silo:2.2.0 --install
```

### enable pw less ssh to localhost
because ansible now runs inside a container, filesystem manipulation on the docker host needs to to be done over ssh to localhost

```
ssh-copy-id username@localhost
```  

### Build cli{1..3} docker images 
with: ```docker-compose -f local.yml build``` and asure that the images are succesfully created. ```docker images```.
start the container ```dockr-compose -f local.yml up -d```

### Play the act
Standard ansible command to start the play is inside ```cd spr/playbooks/``` folder run:

```
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

visit <Host IP>:8090

