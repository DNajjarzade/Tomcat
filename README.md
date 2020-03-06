![tomcat](tomcat.png)
# documentation on how to run and where to start.
To start Tomcat10 with jdk12 the only requirment is docker. all the other software runs isolated inside container.
## quick start:
Asuming docker-compose and ansible are installed also, ssh key authentication to local ip should work too, run:

```
git clone https://github.com/DNajjarzade/Tomcat.git SamanPR &&\
cd SamanPR &&\
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
with: ```docker-compose -f local.yml build ``` and asure that the images are succesfully created. ```docker images```.
start the container ```dockr-compose -f local.yml up -d```

### Play the act
Standard ansible command to start the play is inside ```cd spr/playbooks/``` folder run:

```
ansible-playbook -i inventory playbook.yml --ask-become-pass
```

you can bundle the playbooks into docker images ```ansible-silo --bundle spr ``` and run:

```
docker run --interactive --tty --rm --volume "$HOME/bin:/silo_install_path" spr:latest --install
```

Now you can simply call spr to run your playbooks.

```
spr -i inventory playbook.yml --extra-vars "ansible_become_pass=YourPasswordForDockerHost"
```
visit <Host IP>:8090

