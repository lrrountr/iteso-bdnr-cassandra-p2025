# iteso-bdnr-cassandra

A place to share cassandra app code

### Setup a python virtual env with python cassandra installed
```
# If pip is not present in you system
sudo apt update
sudo apt install python3-pip

# Install and activate virtual env (Linux/MacOS)
python3 -m pip install virtualenv
python3 -m venv ./venv
source ./venv/bin/activate

# Install and activate virtual env (Windows)
python3 -m pip install virtualenv
python3 -m venv ./venv
.\venv\Scripts\Activate.ps1

# Install project python requirements
pip install -r requirements.txt
```


### Launch cassandra container
```
# To start a new container
docker run --name node01 -p 9042:9042 -d cassandra

# If container already exists just start it
docker start node01
```

### Start a Cassandra cluster with multiple nodes
```
# Create a dedicated docker network for the cassandra application
docker network create --subnet=192.168.1.0/24 cassandra-net

# Launch containers with static IPs and attached to the network
docker run --name node01 -d --net  cassandra-net --ip 192.168.1.2 cassandra
docker run --name node02 -d --net  cassandra-net --ip 192.168.1.3 -e CASSANDRA_SEEDS="192.168.1.2" cassandra
docker run --name node03 -d --net  cassandra-net --ip 192.168.1.4 -e CASSANDRA_SEEDS="192.168.1.2" cassandra

# Wait for containers to be fully initialized, verify node status
docker exec -it node01 nodetool status
```

### Cleanup Cassandra Cluster with multiple nodes
```
docker stop node01 node02 node03
docker rm node01 node02 node03
docker network rm cassandra-net
```
