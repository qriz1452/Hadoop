# Create and connect to VM.

sudo apt upgrade                                # upgrading packages
sudo apt update                                 # updating OS to newest
sudo apt install default-jdk -y                 # Installing open source jdk
java -version                                   # to check java versio

# Create hadoop user

sudo addgroup hadoop                            # group for hadoop user
sudo adduser hduser --ingroup hadoop            # give password to user , rest are optional
sudo adduser hduser sudo                        # giving sudo privilege to hduser
su hduser                                       # ALL TASK RELATED TO HADOOP SHOULD BE PERFORMED BY hduser

ifconfig                                        # to check localhost ip
sudo nano /etc/hosts                            # to check loalhost ip


sudo apt install openssh-server -y
ssh-keygen                                          # for passwordless loclhost connection Should bt in ~ directory and user -> hduser
cd .ssh
cat id_rsa.pub >> authorized_keys                    # authorizing keys and user 
ssh localhost                                        # checking localhost paswordless connection
exit                                                 # should be returned to same deirectory


sudo nano /etc/sysctl.conf                           # Hadoop 2 only supports ipv4 in private network, diasbling ipv6 as it causes performance degradation   ............ all config files are in /etc/

net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1                   # Add all 3 in sysctl.conf CTRL+O -> Enter -> CTRL+X to save and exit

sudo sysctl -p                                      # to check updated or not

wget -c https://downloads.apache.org/hadoop/common/stable2/hadoop-2.10.2.tar.gz                 # download hadoop
tar -xzvf hadoop-2.10.1.tar.gz                                                                #Extract and Install Hadoop tar ball
sudo mv hadoop-2.10.1 /usr/local/hadoop                                                       # moving hadoop to its dircetory
sudo chown hduser:hadoop -R /usr/local/hadoop                                                 # transferring permission to hduser


readlink -f $(which java)                                       # Set Enviornment Variable , copy path of java except last two (/bin/java ) then put in java_home variable
nano ~/.bashrc                                                   # to set environment variable going to file



export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64               # copy path from readlink above command
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export PATH=$PATH:/usr/local/hadoop/bin/
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop

source ~/.bashrc                                              # restarting / refreshing bashrc


cd /usr/local/hadoop/etc/hadoop/

nano hadoop-env.sh                                            #Update hadoop-env.sh

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64          # copy path from readlink above command
export HADOOP_LOG_DIR=/var/log/hadoop

sudo mkdir /var/log/hadoop
sudo chown hduser:hadoop -R /var/log/hadoop




#Update core-site.xml                                           # ALL XML CHANGE MUST BE IN BETWEEN <configuration>   ADD YOUR CHANGE  HERE i.e <prop... tags> </configuration> 
nano core-site.xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
 </property>
 <property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:54310</value>
</property>

sudo mkdir -p /app/hadoop/tmp
sudo chown hduser:hadoop /app/hadoop/tmp


#Update mapred-site.xml
cp mapred-site.xml.template mapred-site.xml
nano mapred-site.xml
<property>
  <name>mapreduce.jobtracker.address</name>
  <value>localhost:54311</value>
   </property>
<property>
   <name>mapreduce.framework.name</name>
   <value>yarn</value>
 </property>

                
 #Update hdfs-site.xml
sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
sudo chown -R hduser:hadoop /usr/local/hadoop_store
nano hdfs-site.xml
<property> 
<name>dfs.replication</name>
  <value>1</value>
 </property>
 <property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
 </property>
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
 </property>


#Update yarn-site.xml
nano yarn-site.xml
<property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
   </property>


#Format Namenode
hdfs namenode -format

start-all.sh 
or start-dfs.sh
start-yarn.sh


jps                                 # shows java based running process , to check all ri=unning process ... ps aux ... command 


=======================================DONE=========================================================


to access name node : public-IP-of-VM:50070

Name node runs on 50070 port

