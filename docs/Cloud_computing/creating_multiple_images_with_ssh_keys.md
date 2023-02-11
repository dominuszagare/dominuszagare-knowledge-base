# SSH between multiple docker images

1. Create docker image with ssh service and prohibit password login
```bash
echo '
FROM ubuntu:latest
ARG ssh_pub_key
RUN apt-get update && apt-get install -y openssh-server
RUN apt-get install pssh -y
RUN mkdir ~/.ssh
RUN chmod 700 ~/.ssh
RUN echo "$ssh_pub_key" > ~/.ssh/authorized_keys
RUN chmod 600 ~/.ssh/authorized_keys
RUN sed -i "s/#PermitEmptyPasswords no/PermitEmptyPasswords yes/g" /etc/ssh/sshd_config
RUN sed -i "s/#PermitRootLogin prohibit-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN service ssh start
RUN cd ~/.ssh && ssh-keygen -t rsa -f id_rsa -N ""

EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
' > Dockerfile
```

2. Build docker image with your ssh public key 
```bash 
docker build --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub )" -t ubuntu_with_ssh_service .
```

## Final bash script
```bash
echo '
FROM ubuntu:latest

ARG ssh_pub_key

RUN apt-get update && apt-get install -y openssh-server
RUN apt-get install pssh -y
RUN mkdir ~/.ssh
RUN chmod 700 ~/.ssh
RUN echo "$ssh_pub_key" > ~/.ssh/authorized_keys
RUN chmod 600 ~/.ssh/authorized_keys
RUN sed -i "s/#PermitEmptyPasswords no/PermitEmptyPasswords yes/g" /etc/ssh/sshd_config
RUN sed -i "s/#PermitRootLogin prohibit-password/PermitRootLogin yes/g" /etc/ssh/sshd_config
RUN service ssh start
RUN cd ~/.ssh && ssh-keygen -t rsa -f id_rsa -N ""

EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
' > Dockerfile

docker build --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub )" -t ubuntu_with_ssh_service .

echo 'ustvarjam docker omerzje ubntu_net ce ze ne obstaja'
docker network create -d bridge ubuntu_net

NUMBER_OF_NODES=3

for (( c=1; c<=$NUMBER_OF_NODES; c++ ))do
    echo "ustvarjam node$c"
    docker run -it --rm -d --name node$c ubuntu_with_ssh_service
done

CONTAINERS_IPS=($(docker inspect ubuntu_net --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)))
echo "saving ips of containers to file"
echo "${CONTAINERS_IPS[@]}" > ips

echo "hranim nazive vseh nodeov v datoteko nodes"
: > nodes
for i in "${CONTAINERS_IPS[@]}"; do
    echo "root@$i" >> nodes
done

docker run -it -p 2222:22 --rm -d --name main  ubuntu_with_ssh_service

echo "adding ssh keys of all nodes to all nodes"
KEYS_ARRAY=()

for (( c=1; c<=$NUMBER_OF_NODES; c++ ))do
    echo "pridobivam kljuc iz node$c"
    #docker exec -w /root/.ssh node$c ls
    KEY=$(docker exec -w /root/.ssh node$c cat id_rsa.pub)
    #echo "key: $KEY"
    KEYS_ARRAY+=("$KEY")
    
done

echo "kopiranje datotek v vozlisca"
for (( c=1; c<=$NUMBER_OF_NODES; c++ ))do
    docker cp nodes node$c:/root/nodes
done
docker cp nodes main:/root/nodes

for (( c=1; c<=$NUMBER_OF_NODES; c++ ))do
    #copy keys to node
    for i in "${KEYS_ARRAY[@]}"; do
        docker exec -w /root/.ssh -e KEY="$i" node$c bash -c 'echo "$KEY" >> authorized_keys'
    done
    # add all nodes to known_hosts
    for i in "${CONTAINERS_IPS[@]}"; do
        docker exec -w /root -e ip="$i" node$c bash -c 'ssh-keyscan -H $ip >> ~/.ssh/known_hosts'
    done
    #docker exec -w /root/.ssh node$c cat authorized_keys
done

for i in "${CONTAINERS_IPS[@]}"; do
    docker exec -w /root -e ip="$i" main bash -c 'ssh-keyscan -H $ip >> ~/.ssh/known_hosts'
done


echo "copying ips to main and conecting to main node"
ssh-keyscan -p 2222 -H localhost >> ~/.ssh/known_hosts
scp -P 2222 ips root@localhost:/root/ips
ssh root@localhost -p 2222


for (( c=1; c<=$NUMBER_OF_NODES; c++ ))do
    echo "ustavljam node$c"
    docker stop node$c
done
echo "ustavljam main"
docker stop main

#example comands to run inside container
# parallel-ssh -i -h ~/nodes hostname -I
# parallel-ssh -i -h ~/nodes parallel-ssh -h ~/nodes hostname -I
```