# MPI on docker nodes

MPI or Message Passing Interface is a standard for message passing between processes. It is used for parallel programming in C, C++, Fortran and other languages. It is a st andardized and portable message-passing system designed by a group of researchers from academia and industry to function on a wide variety of parallel computers. The interface consists of a small number of routines that can be used by a programmer to send and receive messages, perform collective operations, and determine properties of the communication environment. The MPI standard is maintained by the [MPI Forum](https://www.open-mpi.org/), an international group of computer scientists and engineers from academia and industry.

## Using MPI on docker nodes

### Running example ping-pong

In this example we will run a ping-pong program on multiple docker nodes. The program will send a random number between 0 and 180 to a worker node. The worker node will then send the number back to the main node. The main node will then add the number to a sum and print the sum. The program will run until the sum is between 270.505 and 270.515. The program will then print the total number of ping-pongs.

```bash
NUMBER_OF_NODES=8

rm -rf scripts
mkdir scripts
echo '
#include <string.h>
#include <mpi.h>
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <time.h>
void worker(int N) {
    printf("worker %d started \n", N);
    MPI_Status status;
    while (1) {
        float ping;
        MPI_Recv(&ping, 1, MPI_FLOAT, 0, 0, MPI_COMM_WORLD, &status);
        printf("(%d) recived ping %.3f \n", N, ping);
        if (ping == -99.0){return;}
        MPI_Send(&ping, 1, MPI_FLOAT, 0, 0, MPI_COMM_WORLD);
    }
}
void main_container(int number_of_processes) {
    printf("main container started \n");
    MPI_Status status;
    float sum = 0.0f;
    int count = 0;
    //FILE *f = fopen("/root/RESULT.txt", "w");
    while (1) {
        for (int i = 1; i <= number_of_processes; i++) {
            float pingSend = (float)(rand()%180000);
            pingSend /= 1000.0f;
            MPI_Send(&pingSend, 1, MPI_FLOAT, i, 0, MPI_COMM_WORLD);
            float pong;
            MPI_Recv(&pong, 1, MPI_FLOAT, i, 0, MPI_COMM_WORLD, &status);
            printf("(0) recived pong %.3f \n",pong);
            //fprintf(f,"recived from %d count: %d sum: %.3f\n", i, count, sum);
            count++;
            sum += pong;
            if (sum > 360.0f) sum -= 360.0f;
            if (sum >= 270.505f && sum <= 270.515f) {
                printf("total number of ping-pongs: %d\n", count);
                float pingSend = -99.0f; //send to all -99.0f is a signal to stop
                for (int j = 1; i <= number_of_processes; j++){
                    MPI_Send(&pingSend, 1, MPI_FLOAT, j, 0, MPI_COMM_WORLD);
                }
                //fclose(f);
                return;
            }
            //printf("(0)Received %f sum: %.3f", pong, sum);
        }
    }
}
int main(int argc, char* argv[]) {
    int rank;
    int number_of_containers = 0;
    for (int i = 0; i < argc; i++) {
        if (strcmp(argv[i], "-number") == 0) {
            number_of_containers = atoi(argv[i + 1]);
        }
    }
    printf("number of containers: %d\n", number_of_containers);
    MPI_Init(&argc, &argv);
    MPI_Comm_set_errhandler(MPI_COMM_WORLD, MPI_ERRORS_RETURN); // return info about errors
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    if (rank == 0){main_container(number_of_containers);}
    else{worker(rank);}
    MPI_Finalize();
    return 0;
}
' > scripts/ping.c

echo "
echo 'compiling on all nodes'
parallel-ssh -i -h ~/nodes mpicc ping.c -o ./ping
mpicc ping.c -o ./ping
echo 'run ping-pong'
mpirun -np $NUMBER_OF_NODES --hostfile hosts --allow-run-as-root ping -number $NUMBER_OF_NODES > RESULT.txt
" > scripts/start.sh

echo 'ustvarjam dockerfile za sliko nasega kontainerja'

: > Dockerfile
echo '
FROM ubuntu:latest

ARG ssh_pub_key

RUN apt-get update
RUN apt-get install gcc -y
RUN apt-get install pssh -y
RUN apt-get install net-tools -y
RUN apt-get install openssh-server openssh-client -y
RUN apt-get install sshpass wget -y
RUN apt-get install openmpi-bin openmpi-common libopenmpi-dev libgtk2.0-dev -y

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

docker build --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub )" -t ubuntu_mpi .

echo 'ustvarjam docker omerzje ubntu_net ce ze ne obstaja'
docker network create -d bridge ubuntu_net

for (( c=1; c<=$NUMBER_OF_NODES; c++ )) do
    echo "ustvarjam node$c"
    docker run -it --rm -d --name node$c ubuntu_mpi
done

CONTAINERS_IPS=($(docker inspect ubuntu_net --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -q)))
echo "saving ips of containers to file"
echo "${CONTAINERS_IPS[@]}" > scripts/ips

echo "hranim nazive vseh nodeov v datoteko nodes"
: > scripts/nodes
: > scripts/hosts
for i in "${CONTAINERS_IPS[@]}"; do
    echo "root@$i" >> scripts/nodes
    echo "$i" >> scripts/hosts
done

docker run -it -p 2222:22 --rm -d --name main  ubuntu_mpi

echo "adding ssh keys of all nodes to all nodes"
KEYS_ARRAY=()

for (( c=1; c<=$NUMBER_OF_NODES; c++ )) do
    echo "pridobivam kljuc iz node$c"
    #docker exec -w /root/.ssh node$c ls
    KEY=$(docker exec -w /root/.ssh node$c cat id_rsa.pub)
    #echo "key: $KEY"
    KEYS_ARRAY+=("$KEY")
    
done

echo "kopiranje datotek v vozlisca"
for (( c=1; c<=$NUMBER_OF_NODES; c++ )) do
    docker cp scripts/. node$c:/root/
done
docker cp scripts/. main:/root/

for (( c=1; c<=$NUMBER_OF_NODES; c++ )) do
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

echo "povezovanje na main in poganjanje skripte start.sh"
ssh-keyscan -p 2222 -H localhost >> ~/.ssh/known_hosts
#scp -P 2222 ips root@localhost:/root/ips
ssh root@localhost -p 2222 "bash start.sh"
#copy results to local machine
scp -P 2222 root@localhost:/root/RESULT.txt RESULT.txt

for (( c=1; c<=$NUMBER_OF_NODES; c++ )) do
    echo "ustavljam node$c"
    docker stop node$c
done
echo "ustavljam main"
docker stop main

```