# Docker cluster

```bash
NUMBER_OF_NODES_IN_CLUSTER=$1

echo 'ustvarjam bash scripte za preiskovanje clusterja'
mkdir scripts
: > scripts/hosts
for (( c=1; c<=$NUMBER_OF_NODES_IN_CLUSTER; c++ )) do
    x=$c"_1"
    echo -e "test@n02_cluster_node$x" >> scripts/hosts
done

: > scripts/conections.sh
echo 'echo "pridobi ip noslove vseh vozlisc"' >> scripts/conections.sh
#echo 'ssh-keyscan -H localhost >> ~/.ssh/known_hosts' >> scripts/test.sh
echo 'parallel-ssh -i -o ~/logs/ -O StrictHostKeyChecking=no -h ~/hosts hostname -I' >> scripts/conections.sh

: > scripts/test.sh
echo 'mkdir logs' >> scripts/test.sh
echo 'echo "vsako vozlisce pridobi ip naslove vsakega"' >> scripts/test.sh
#echo 'ssh-keyscan -H localhost >> ~/.ssh/known_hosts' >> scripts/test.sh
echo 'parallel-ssh -i -o ~/logs/ -O StrictHostKeyChecking=no -h ~/hosts bash conections.sh' >> scripts/test.sh

echo 'ustvarjam docker omerzje ubntu_net ce ze ne obstaja'
docker network create -d bridge ubuntu_net

echo 'ustvarjam dockerfile za sliko nasega kontainerja'
#create dockerfile
: > Dockerfile
echo 'FROM ubuntu:latest' >> Dockerfile
echo 'RUN apt-get update && apt-get install -y openssh-server' >> Dockerfile
echo 'RUN mkdir /var/run/sshd' >> Dockerfile
echo 'RUN useradd -m test' >> Dockerfile
echo 'RUN passwd -d test' >> Dockerfile
echo "RUN sed -i'' -e's/^#PermitRootLogin prohibit-password$/PermitRootLogin yes/' /etc/ssh/sshd_config \
        && sed -i'' -e's/^#PasswordAuthentication yes$/PasswordAuthentication yes/' /etc/ssh/sshd_config \
        && sed -i'' -e's/^#PermitEmptyPasswords no$/PermitEmptyPasswords yes/' /etc/ssh/sshd_config" >> Dockerfile
echo 'RUN service ssh start' >> Dockerfile
echo 'RUN apt-get install pssh -y' >> Dockerfile
echo 'COPY ./scripts /home/test/' >> Dockerfile
echo 'EXPOSE 22' >> Dockerfile
echo 'CMD ["/usr/sbin/sshd","-D"]' >> Dockerfile

echo 'ustvarjam sliko'
docker build -t sshd_ubuntu . #<---------odkomentiraj------------------------------------------------

echo 'ustvarjam docker-compose konfiguracijo'
: > docker-compose.yml
echo "version: '3.8'" >> docker-compose.yml
echo "" >> docker-compose.yml
echo 'services:' >> docker-compose.yml

for (( c=1; c<=$NUMBER_OF_NODES_IN_CLUSTER; c++ ))
do
    echo "  node$c:" >> docker-compose.yml
    echo '      image: sshd_ubuntu' >> docker-compose.yml
    echo '      networks:' >> docker-compose.yml
    echo "          - cluster" >> docker-compose.yml
    echo '      ports:' >> docker-compose.yml
    echo "          - '2222$c:2222$c'" >> docker-compose.yml
    echo "          - '5000$c:5000$c'" >> docker-compose.yml
    echo '      environment:' >> docker-compose.yml
    echo "          - APPID=$c" >> docker-compose.yml
done
echo 'networks:' >> docker-compose.yml
echo '  cluster:' >> docker-compose.yml
echo '      name: ubuntu_net' >> docker-compose.yml

docker-compose up&

docker run -it --rm -d -p 2222:22 --name ubuntu_main --network ubuntu_net sshd_ubuntu

echo 'povezovanje v bash lupino ustvarjenega kontainerja prek ssh'
ssh-keyscan -p 2222 -H localhost >> ~/.ssh/known_hosts
ssh -p 2222 test@127.0.0.1

docker stop ubuntu_main
docker-compose down
```