# documentation on how to run and where to start.
To start Tomcat10 with jdk12 the only requirment is docker. all the other software runs solated inside inside container.
## quick start:
```
docker-compose build -f local.yml && docker-compose -f local.yml up -d && cd ./spr/playbooks/ && ansible-playbook -i inventory playbook.yml --extra-vars "ansible_become_pass=DockerHostPass" 
```
visit DockerHostIP:8090


##under the hood:

I prefer to run docker-compose as a container so one thing less to install on the docker system. 
## install docker-compose
```
sudo curl -L --fail https://github.com/docker/compose/releases/download/1.25.4/run.sh -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

I also prefer not install ansible a second thing less on the docker server.

By running ansible as a docker container you can call playbooks by name. install ansible-silo:
## install ansible-silo
```
docker run --interactive --tty --rm --volume "/usr/local/bin:/silo_install_path" grpn/ansible-silo:2.2.0 --install
```
because ansible now runs inside a container, filesystem manipulation on the docker host needs to to be done over ssh to localhost
## enable pw less ssh to localhost
```
ssh-copy-id username@localhost
```  

Build cli{1..3} images with: ```docker-compose -f local.yml build ``` and asure that the images are succesfully created. ```docker images```.
start the container ```dockr-compose -f local.yml up -d```

Standard ansible command to start the play is:
```
ansible-playbook -i hosts spr/playbooks/tc-playbook.yml --ask-become-pass
```
or if you want to run by name you can bundle the playbooks into docker images ```ansible-silo --bundle spr ``` and run:
```
docker run --interactive --tty --rm --volume "$HOME/bin:/silo_install_path" spr:latest --install
```
Now you can simply call spr to run your playbooks.
```
spr -i hosts spr/playbooks/tc-playbook.yml --extra-vars "ansible_become_pass=YourPasswordForAnsibleHost"
```
visit <Host IP>:8090

