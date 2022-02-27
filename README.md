# Setting up Apache Spark to AWS EC2
### 1- Login to AWS and launch an instance
![1.png](attachment:32ee3602-293b-47a1-b75b-ec80007a47b2.png)

### 2- Select Ubuntu Server 18.04 LTS
![2.png](attachment:2191e541-6bbc-4fcc-8315-5fd76f487786.png)

### 3- Choose an Instance Type (Choose recommended)
![3.png](attachment:61e0377c-3ad9-45b2-a89e-cef900a81201.png)

### 4- The instance Auto-assing Public IP has to be enable
![4.png](attachment:45920560-5957-452e-a5ce-23b27bbc14a6.png)

### 5- For larger size installations we can add additional volume
![5.png](attachment:fb4add6d-ad19-430d-b20b-43a80289abf1.png)

### 6- Make sure the security group type has all traffic allowed to inbound and outbound that you need
![6.png](attachment:a00a0a26-1996-48c7-9c5f-2e837a72f33d.png)

### 7- Create a Private Key file (.pem)
![Ekran Resmi 2022-02-25 01.07.46.png](attachment:1e2790b9-3d15-48a1-b710-3cf6a9d5b7b5.png)

### 8- Launch the Instance
![8.png](attachment:14d00b9b-75d6-4970-a0af-3c52df9696fd.png)

### 9- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
![Ekran Resmi 2022-02-25 01.32.26.png](attachment:3580fb2c-5545-4d99-9e52-0ab546e0d187.png)

### 10- Connect using SSH
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
![Ekran Resmi 2022-02-25 01.32.26.png](attachment:3580fb2c-5545-4d99-9e52-0ab546e0d187.png)


### 11- Add Entries in hosts file
```console
sudo nano /etc/hosts
```
##### Write into
```console
<Your Private IPv4 addresses> master
```
![Ekran Resmi 2022-02-25 01.34.10.png](attachment:174aace2-c4f6-41f9-900a-9210bde28171.png)

### 12- Update the packages
```console
sudo apt update
sudo apt upgrade
```
![Ekran Resmi 2022-02-25 01.37.49.png](attachment:7582999e-f9d0-479b-ba03-6348eaa3686b.png)


### 13- Install Open JDK 8
```console
sudo apt-get install openjdk-8-jdk
```
![Ekran Resmi 2022-02-25 01.39.39.png](attachment:6dc10d0f-12a0-44a8-bb09-57dd23e9a821.png)


### 14- Install Scala
```console
sudo apt-get install scala
```
![Ekran Resmi 2022-02-25 01.40.34.png](attachment:ceca3a70-6941-4aa4-a929-1fa7f8428cc1.png)


### 15- SSH Connection
```console
sudo apt-get install openssh-server openssh-client
ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
![Ekran Resmi 2022-02-25 01.40.34.png](attachment:ceca3a70-6941-4aa4-a929-1fa7f8428cc1.png)


## 16- Install Spark
#### Download Spark
```console
wget https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz 
```
#### Extract the saved archive using tar
```console
tar xvf spark-*
```
![Ekran Resmi 2022-02-25 01.40.34.png](attachment:ceca3a70-6941-4aa4-a929-1fa7f8428cc1.png)

#### Move the unpacked directory spark-3.0.1-bin-hadoop2.7 to the opt/spark directory
```console
sudo mv spark-3.0.1-bin-hadoop2.7 /opt/spark
```
![Ekran Resmi 2022-02-25 01.40.34.png](attachment:ceca3a70-6941-4aa4-a929-1fa7f8428cc1.png)


### 17- Setting up of Environment
```console
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.profile
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.profile
$
```
#### Open .profile file
```console
nano .profile
```
##### Write into
```console
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.profile
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.profile
$
```
#### For reflecting to current session
```console
source ~/.profile
```
![Ekran Resmi 2022-02-25 01.46.52.png](attachment:5b90ab7a-9806-48c8-b54f-079c4ceb4d87.png)

### 18- Start Standalone Spark Master Server
```console
start-master.sh
```
![Ekran Resmi 2022-02-25 02.02.11.png](attachment:8fa3c2e0-b260-483e-9b4d-6e00945879d7.png)

#### Spark Web UI
```console
http://<yourpublicdns>:8080/
```
![Ekran Resmi 2022-02-25 03.01.17.png](attachment:f9ce93a0-6b6e-448f-8294-e31e866c7b90.png)

### 19- Start Spark Slave Server (Start a Worker Process)
```console
start-slave.sh spark://master:port
E.g.--> start-slave.sh spark://ec2-3-121-127-213.eu-central-1.compute.amazonaws.com:7077
```
![Ekran Resmi 2022-02-25 02.02.11.png](attachment:8fa3c2e0-b260-483e-9b4d-6e00945879d7.png)
 
### 20- Specify Resource Allocation for Workers
```console
start-slave.sh -c 1 spark://master:port
```
###### Reload Spark Master’s Web UI to confirm the worker’s configuration
![Ekran Resmi 2022-02-25 02.02.11.png](attachment:8fa3c2e0-b260-483e-9b4d-6e00945879d7.png)

###### For example, to start a worker with 512MB of memory, enter this command:
```console
start-slave.sh -m 512M spark://master:port
```
![Ekran Resmi 2022-02-25 02.02.11.png](attachment:8fa3c2e0-b260-483e-9b4d-6e00945879d7.png)

### 21- Test Spark Shell
```console
spark-shell
```
###### Write:":q" and press "Enter" to exit Scala.
![Ekran Resmi 2022-02-25 03.01.17.png](attachment:f9ce93a0-6b6e-448f-8294-e31e866c7b90.png)

### 22- Test Python in Spark
```console
pyspark
```
###### Write "exit()" and hit "Enter" to exit this shell.
![Ekran Resmi 2022-02-25 03.01.17.png](attachment:f9ce93a0-6b6e-448f-8294-e31e866c7b90.png)


### 23- Start Spark Services
```console
start-master.sh
stop-slave.sh
```
###### Start both master and server
```console
start-all.sh
```

### 24- Before logout, make sure to stop Spark services and AWS Instance
```console
stop-master.sh
stop-slave.sh
```
###### Start both master and server
```console
stop-all.sh
```
![Ekran Resmi 2022-02-25 03.06.39.png](attachment:9829409c-2875-442b-b61e-b979971e04cf.png)



## Seda Atalay
