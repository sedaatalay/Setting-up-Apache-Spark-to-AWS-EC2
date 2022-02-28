# Setting up Apache Spark to AWS EC2
### 1- Login to AWS and launch an instance
<img width="1234" alt="Ekran Resmi 2022-02-27 20 40 21" src="https://user-images.githubusercontent.com/91700155/155900685-8e6362b9-c33b-46e3-aa73-eb1f055f87f8.png">

### 2- Select Ubuntu Server 18.04 LTS
<img width="1407" alt="Ekran Resmi 2022-02-27 20 41 24" src="https://user-images.githubusercontent.com/91700155/155900701-9728d47c-8905-41f1-b72c-108ed8c4f33b.png">

### 3- Choose an Instance Type (Choose recommended)
<img width="1429" alt="Ekran Resmi 2022-02-27 20 41 58" src="https://user-images.githubusercontent.com/91700155/155900718-151b29a2-e149-4c28-8b02-c5857aebf1b6.png">

### 4- The instance Auto-assing Public IP has to be enable
<img width="1431" alt="Ekran Resmi 2022-02-27 20 42 23" src="https://user-images.githubusercontent.com/91700155/155900732-48b5faad-8d83-4319-974a-fa72fcb4637d.png">

### 5- For larger size installations we can add additional volume
<img width="1428" alt="Ekran Resmi 2022-02-27 20 42 58" src="https://user-images.githubusercontent.com/91700155/155900738-9e03df14-ff0d-4dd5-837d-dc1d18a480cc.png">

### 6- Make sure the security group type has all traffic allowed to inbound and outbound that you need
<img width="1116" alt="Ekran Resmi 2022-02-27 20 44 45" src="https://user-images.githubusercontent.com/91700155/155900748-2f0c6a14-eec5-4a9d-8a15-11fabcc79c10.png">

### 7- Create a Private Key file (.pem)
<img width="1421" alt="Ekran Resmi 2022-02-27 20 46 18" src="https://user-images.githubusercontent.com/91700155/155900755-7e00a8c4-3c11-4e8d-a272-9360948cf862.png">

### 8- Launch the Instance
<img width="1423" alt="Ekran Resmi 2022-02-27 20 46 38" src="https://user-images.githubusercontent.com/91700155/155900765-25266061-7b5b-40cf-90d9-c3711367a78b.png">

### 9- Change the permission of Private Key
```console
chmod 400 <pem file name>.pem
```
### 10- Connect using SSH
```console
ssh -i <pem file name> ubuntu@public-dns-name
```
<img width="1308" alt="Ekran Resmi 2022-02-27 20 51 14" src="https://user-images.githubusercontent.com/91700155/155900818-3217fab3-ed72-4940-953b-c30040330a80.png">

### 11- Add Entries in hosts file
```console
sudo nano /etc/hosts
```
##### Write into
```console
<Your Private IPv4 addresses> master
```
<img width="1309" alt="Ekran Resmi 2022-02-27 20 52 35" src="https://user-images.githubusercontent.com/91700155/155900853-3185c408-c2c2-4367-84d5-0abcd4402050.png">

### 12- Update the packages
```console
sudo apt update
sudo apt upgrade
```
<img width="1391" alt="Ekran Resmi 2022-02-27 20 54 28" src="https://user-images.githubusercontent.com/91700155/155900857-034474f3-fe3f-4f0c-b109-946d4f9abcf2.png">

### 13- Install Open JDK 8
```console
sudo apt-get install openjdk-8-jdk
```
<img width="1302" alt="Ekran Resmi 2022-02-27 20 56 34" src="https://user-images.githubusercontent.com/91700155/155900861-7a782821-a440-4474-b184-7e324caca6cb.png">

### 14- Install Scala
```console
sudo apt-get install scala
```
<img width="1258" alt="Ekran Resmi 2022-02-27 20 57 35" src="https://user-images.githubusercontent.com/91700155/155901617-c3d3a2f2-e5d6-43e8-8a3d-f2c995329eb5.png">

### 15- SSH Connection
```console
sudo apt-get install openssh-server openssh-client
ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
<img width="1325" alt="Ekran Resmi 2022-02-27 20 58 51" src="https://user-images.githubusercontent.com/91700155/155900873-c4c0c4d8-8da4-43e2-9511-57a21392a7d6.png">

## 16- Install Spark
#### Download Spark
```console
wget https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz 
```
#### Extract the saved archive using tar
```console
tar xvf spark-*
```
<img width="1293" alt="Ekran Resmi 2022-02-27 20 59 44" src="https://user-images.githubusercontent.com/91700155/155901651-5c0a0a6b-7895-4793-9ae6-e1c117f163c6.png">

#### Move the unpacked directory spark-3.0.1-bin-hadoop2.7 to the opt/spark directory
```console
sudo mv spark-3.0.1-bin-hadoop2.7 /opt/spark
```
<img width="1101" alt="Ekran Resmi 2022-02-27 21 00 41" src="https://user-images.githubusercontent.com/91700155/155900911-9dffc093-5810-447e-8a7c-c8fce2912dd4.png">

### 17- Setting up of Environment
```console
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.profile
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.profile
```
<img width="1309" alt="Ekran Resmi 2022-02-27 21 02 27" src="https://user-images.githubusercontent.com/91700155/155901678-1d9e097c-3f77-4529-9f61-2eb56f1a01cb.png">                
                                   
#### Open .profile file
```console
nano .profile
```
##### Write into
```console
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo "export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin" >> ~/.profile
echo "export PYSPARK_PYTHON=/usr/bin/python3" >> ~/.profile
```
<img width="1294" alt="Ekran Resmi 2022-02-27 21 01 35" src="https://user-images.githubusercontent.com/91700155/155900944-b69f9037-3fb0-4f7e-b5a3-095676c66dc7.png">

#### For reflecting to current session
```console
source ~/.profile
```
<img width="1309" alt="Ekran Resmi 2022-02-27 21 02 27" src="https://user-images.githubusercontent.com/91700155/155900949-0167d5a5-b372-442c-bdc1-43bdec4e73f4.png">

### 18- Start Standalone Spark Master Server
```console
start-master.sh
```
<img width="1287" alt="Ekran Resmi 2022-02-27 21 04 18" src="https://user-images.githubusercontent.com/91700155/155900976-39ddad85-f7f9-4ab4-b4d3-def3070bc2d9.png">

#### Spark Web UI
```console
http://<yourpublicdns>:8080/
```
<img width="1434" alt="Ekran Resmi 2022-02-27 21 04 03" src="https://user-images.githubusercontent.com/91700155/155900994-6720a141-9b5d-4e7b-a6bd-1d5e61549168.png">

### 19- Start Spark Slave Server (Start a Worker Process)
```console
start-slave.sh spark://master:port
E.g.> start-slave.sh spark://ec2-3-121-127-213.eu-central-1.compute.amazonaws.com:7077
```
<img width="1422" alt="Ekran Resmi 2022-02-27 21 07 02" src="https://user-images.githubusercontent.com/91700155/155901457-03712ef7-4dff-4b02-9cfc-d6564ea83432.png">

### 20- Specify Resource Allocation for Workers
```console
start-slave.sh -c 1 spark://master:port
```
###### Reload Spark Master’s Web UI to confirm the worker’s configuration
<img width="1426" alt="Ekran Resmi 2022-02-27 21 07 54" src="https://user-images.githubusercontent.com/91700155/155901471-085734a9-df2a-401e-99e5-6c5491a8c2d6.png">

###### For example, to start a worker with 512MB of memory, enter this command:
```console
start-slave.sh -m 512M spark://master:port
```
<img width="1421" alt="Ekran Resmi 2022-02-27 21 08 48" src="https://user-images.githubusercontent.com/91700155/155901483-41d629c0-24d2-42b3-af04-b9dd4bd6ffdf.png">

### 21- Test Spark Shell
```console
spark-shell
```
###### Write:":q" and press "Enter" to exit Scala.

### 22- Test Python in Spark
```console
pyspark
```
###### Write "exit()" and hit "Enter" to exit this shell.
<img width="1277" alt="Ekran Resmi 2022-02-27 21 11 43" src="https://user-images.githubusercontent.com/91700155/155901198-cbef39fb-6c7f-4110-8023-df9cc92632b1.png">

### 23- Start Spark Services
```console
start-master.sh
start-slave.sh
```
###### Start both master and server
```console
start-all.sh
```
<img width="1285" alt="Ekran Resmi 2022-02-27 21 09 11" src="https://user-images.githubusercontent.com/91700155/155901499-2dbafc2c-cda3-4c16-8709-b2367bf69185.png">

### 24- Before logout, make sure to stop Spark services and AWS Instance
```console
stop-master.sh
stop-slave.sh
```
###### Stop both master and server
```console
stop-all.sh
```
<img width="1254" alt="Ekran Resmi 2022-02-27 21 12 30" src="https://user-images.githubusercontent.com/91700155/155901531-284faa95-a456-4e2f-b401-f46d4894e1ab.png">



## Seda Atalay
