# hadoop-installation
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
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/580e2f64-c9e0-4ed4-95b5-717519d34236/Untitled.png)
    
    Selain itu, juga dapat dengan menjalankan Web UI Hadoop dengan membuka http://{IP Server}:9870/dfshealth.html dan http://{IP Server}:8088/cluster

