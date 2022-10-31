# hadoop-installation (Single Node)
Hadoop installation

Step :

1. sudo apt install default-jre
2. export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd6
3. wget https://archive.apache.org/dist/hadoop/core/stable/hadoop-3.3.4.tar.gz
4. tar -xvf hadoop-3.3.4.tar.gz

## Konfigurasi Instalasi Hadoop

Sebelum menggunakan Hadoop, terdapat beberapa file yang harus dikonfigurasikan.

- Pertama, yaitu melakukan Setting Environtment Variables. Buka file .bashrc yang berada pada direktori /home/{nama user} menggunakan text editor di Server (vim, vi, nano, gedit, dll). Contoh perintah,
`vi .bashrc`

- Masukkan konfigurasi environtment variable berikut ke dalam file .bashrc
    
    ```bash
    #User specific aliases and functions
    export HADOOP_HOME=$HOME/hadoop-3.3.4
    export HADOOP_CONF_DIR=$HOME/hadoop-3.3.4/etc/hadoop
    export HADOOP_MAPRED_HOME=$HOME/hadoop-3.3.4
    export HADOOP_COMMON_HOME=$HOME/hadoop-3.3.4
    export HADOOP_HDFS_HOME=$HOME/hadoop-3.3.4
    export YARN_HOME=$HOME/hadoop-3.3.4
    export PATH=$PATH:$HOME/hadoop-3.3.4/bin
    
    #Set JAVA HOME
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    export PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin:$PATH
    ```
    
    Setelah memasukkan konfigurasi di atas, jalankan perintah berikut untuk menerapkan perubahannya pada Server,
    `source ~/.bashrc`
    
    Periksa apakah hadoop telah terinstall dengan benar atau belum, dengan memeriksa versi hadoop,
    `hadoop version`

Kedua melakukan perubahan terhadap beberapa file configuration, yang terdapat pada folder hadoop-3.3.4/etc/hadoop

**core-site.xml**

```xml
<configuration>
	<property>
		<name>fs.default.name</name>
		<value>hdfs://localhost:9000</value>
	</property>
</configuration>
```

**hdfs-site.xml**

```xml
<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>
	<property>
		<name>dfs.permission</name>
		<value>false</value>
	</property>
	<property>
	    <name>dfs.namenode.name.dir</name>
	    <value>/home/(server name)/hadoop/data/namenode</value>
	</property>
	<property>
	    <name>dfs.datanode.data.dir</name>
	    <value>/home/(server name)/hadoop/data/datanode</value>
	</property>
</configuration>
```

**mapred-site.xml**

```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
    </property>
</configuration>
```

**yarn-site.xml**

```xml
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
	<property>
		<name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
		<value>org.apache.hadoop.mapred.ShuffleHandler</value>
	</property>
</configuration>
```

**hadoop-env.sh**

```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```

****Setup passphraseless ssh****

```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

## Menjalankan Hadoop

- Sebelum menjalankan Hadoop, perlu dilakukan format pada hdfs yang akan digunakan, dengan menjalankan
`hdfs namenode -format`

- Menjalankan NameNode
`hdfs --daemon start namenode`

- Menjalankan DataNode
`hdfs --daemon start datanode`

- Menjalankan Resource Manager
`yarn --daemon start resourcemanager`

- Menjalankan Node Manager
`yarn --daemon start nodemanager`

- Periksa status hadoop yang sudah dijalankan dengan perintah
`jps`
    
    
Web UI Hadoop : -   http://{IP Server}:9870/dfshealth.html 
                -   http://{IP Server}:8088/cluster




# hadoop-installation (Single Node)

```bash
Pre-requirements
Ubuntu 18.04 installed on a virtual machine.
What are we going to install in order to create the Hadoop Multi-Node Cluster?
Java 8;
SSH;
PDSH;
1st Step: Configuring our Network
Go to the Network Settings of your Virtual Machine and Enable Adapter 2. Then, instead of NAT, chose Virtual Host-Only Adapter and where it says “Promiscuous Mode” select the option “Allow All”.
```


-   2nd Step:
    ```bash
    Install SSH using the following command:

    sudo apt install ssh

    It will ask you for the password. When it asks for confirmation, just give it.
    ```


-   3rd Step:
    ```bash
    Install PDSH using the following command:

    sudo apt install pdsh

    Just as before, give confirmation when needed.
    ```

-   4th Step:
    ```bash
    Open the .bashrc file with the following command:

    nano .bashrc

    At the end of the file just write the following line:

    export PDSH_RCMD_TYPE=ssh
    ```

-   5th Step:
    ```bash
    Now let’s configure SSH. Let’s create a new key using the following command:

    ssh-keygen -t rsa -P ""

    Just press Enter everytime that is needed.
    ```

-   6th Step:
    ```bash
    Now we need to copy the public key to the authorized_keys file with the following command:

    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    ```

-   7th Step:
    ```bash
    Now we can verify the SSH configuration by connecting to the localhost:

    ssh localhost

    Just type “yes” and press Enter when needed.
    ```

-   8th Step:
    ```bash
    This is the step where we install Java 8. We use this command:

    sudo apt install openjdk-8-jdk

    Just as previously, give confirmation when needed.
    ```

-   9th Step:
    ```bash
    This step isn’t really a step, it’s just to check if Java is now correctly installed:

    java -version
    ```

-   10th Step:
    ```bash
    Download Hadoop using the following command:

    sudo wget -P ~ https://mirrors.sonic.net/apache/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
    ```

11th Step:
```bash
We need to unzip the hadoop-3.2.1.tar.gz file with the following command:

tar xzf hadoop-3.2.1.tar.gz
```

12th Step:
```bash
Change the hadoop-3.2.1 folder name to hadoop (this maked it easier to use). Use this command:

mv hadoop-3.2.1 hadoop
```

13th Step:
```bash
Open the hadoop-env.sh file in the nano editor to edit JAVA_HOME:

nano ~/hadoop/etc/hadoop/hadoop-env.sh

Paste this line to JAVA_HOME:

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
(I forgot to take a screenshot for this step, but it’s really easy to find. Once you find it just remove the # commentary tag and do what I said, copy it).
```

14th Step:
```bash
Change the hadoop folder directory to /usr/local/hadoop. This is the command:

sudo mv hadoop /usr/local/hadoop

Provide the password when needed.
```

15th Step:
```bash
Open the environment file on nano with this command:

sudo nano /etc/environment

Then, add the following configurations:

PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/local/hadoop/bin:/usr/local/hadoop/sbin"JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre"
```


16th Step:
```bash
Now we will add a user called hadoopuser, and we will set up it’s configurations:

sudo adduser hadoopuser

Provide the password and you can leave the rest blank, just press Enter.


Now type these commands:

sudo usermod -aG hadoopuser hadoopuser
sudo chown hadoopuser:root -R /usr/local/hadoop/
sudo chmod g+rwx -R /usr/local/hadoop/
sudo adduser hadoopuser sudo

```

17th Step:
```bash
Now we need to verify the machine ip address:

ip addr

Now, as you can see, my IP is 192.168.205.7, just remember this will be different for you, you need to act accordingly when the IP addresses are used later.

My network will be as follows:

master: 192.168.205.7

slave1: 192.168.205.8

slave2: 192.168.205.9

In your case, just keep adding 1 to the last number of the IP you get on your machine, just as I did for mine.
```

18th Step:
```bash
Open the hosts file and insert your Network configurations:

sudo nano /etc/hosts
```

19th Step:
```bash
Now is the time to create the Slaves.

Shut Down your Master Virtual Machine and clone it twice, naming one Slave1 and the Other Slave2.

Make sure the “Generate new MAC addresses for all network adapters” option is chosen.

Also, make a Full Clone.
```


Clone for Slave1, do the same for Slave2.
20th Step:
```bash
On the master VM, open the hostname file on nano:

sudo nano /etc/hostname

Insert the name of your master virtual machine. (note, it’s the same name you entered previously on the hosts file)


Now do the same on the slaves:
Also, you should reboot all of them so this configuration taked effect:

sudo reboot

```

21st Step:
```bash
Configure the SSH on hadoop-master, with the hadoopuser. This is the command:

su - hadoopuser
```

22nd Step :
```bash
Create an SSH key:

ssh-keygen -t rsa
```


23rd Step:
```bash
Now we need to copy the SSH key to all the users. Use this command:

ssh-copy-id hadoopuser@hadoop-master
ssh-copy-id hadoopuser@hadoop-slave1
ssh-copy-id hadoopuser@hadoop-slave2
```


24th Step:
```bash
On hadoop-master, open core-site.xml file on nano:

sudo nano /usr/local/hadoop/etc/hadoop/core-site.xml

Then add the following configurations:
```
    ```xml
    <configuration>
    <property>
    <name>fs.defaultFS</name>
    <value>hdfs://hadoop-master:9000</value>
    </property>
    </configuration>
    ```


25th Step:
```bash
Still on hadoop-master, open the hdfs-site.xml file.

sudo nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml

Add the following configurations:
```
    ```xml
    <configuration>
    <property>
    <name>dfs.namenode.name.dir</name><value>/usr/local/hadoop/data/nameNode</value>
    </property>
    <property>
    <name>dfs.datanode.data.dir</name><value>/usr/local/hadoop/data/dataNode</value>
    </property>
    <property>
    <name>dfs.replication</name>
    <value>2</value>
    </property>
    </configuration>
    ```

26th Step:
```bash
We’re still on hadoop-master, let’s open the workers file:

sudo nano /usr/local/hadoop/etc/hadoop/workers

Add these two lines: (the slave names, remember the hosts file?)

hadoop-slave1
hadoop-slave2
```


27th Step:
```bash
We need to copy the Hadoop Master configurations to the slaves, to do that we use these commands:

scp /usr/local/hadoop/etc/hadoop/* hadoop-slave1:/usr/local/hadoop/etc/hadoop/
scp /usr/local/hadoop/etc/hadoop/* hadoop-slave2:/usr/local/hadoop/etc/hadoop/

Copying information to Slave1

Copying Information to Slave2
```

28th Step:
```bash
Now we need to format the HDFS file system. Run these commands:

source /etc/environment
hdfs namenode -format
```

29th Step:
```bash
Start HDFS with this command:

start-dfs.sh

To check if this worked, run the follwing command. This will tell you what resources have been initialized:

jps

Now we need to do the same in the slaves:
```


30th Step:
```bash
Let’s see if this worked:

Open your browser and type hadoop-master:9870.

This is what mine shows, hopefully yours is showing the same thing!


As you can see, both nodes are operational!
```


31st Step:
```bash
Let’s configure yarn, just execute the following commands:

export HADOOP_HOME="/usr/local/hadoop"
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
```

32nd Step:
```bash
In both slaves, open yarn-site.xml on nano:

sudo nano /usr/local/hadoop/etc/hadoop/yarn-site.xml
You have to add the following configurations on both slaves:
```
    ```xml
    <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>hadoop-master</value>
    </property>
    ```

33rd Step:
```bash
On the master, let’s start yarn. Use this command:

start-yarn.sh
```

34th Step:
```bash
Open your browser. Now you will type http://hadoop-master:8088/cluster


As you can see, the cluster shows 2 active nodes!
```