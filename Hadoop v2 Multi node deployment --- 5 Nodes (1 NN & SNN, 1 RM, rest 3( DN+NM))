***********************NAME NODE ( NN ) ***************************************************************

# Create and login to VM


sudo apt update && sudo apt dist-upgrade     # updating + ONLY distribution update not whole OS Update

***** OPEN NEW TERMINAL
# Copy public key on to the DataCenter main server (Name node)

scp -i yournamenode.pem yournamenode.pem ubuntu@public_dns:~/.ssh/



# OPEN THE FIRST TERMINAL AGAIN AND use :

ls
cd .ssh
ls

# Create a Hadoop user for accessing HDFS
sudo addgroup hadoop
sudo adduser hduser --ingroup hadoop 
sudo adduser hduser sudo
sudo su hduser
cd




# Create local key
ssh-keygen
cd .ssh
ls
cat id_rsa.pub >> authorized_keys
cd
ssh localhost
exit


# Copy the instance public key (yournamenode.pem) to hduser's directory
cd .ssh
ls
exit
cd .ssh
ls
cd
sudo su


**cp /home/ubuntu/.ssh/multi.pem /home/hduser/.ssh/
cp /home/ubuntu/.ssh/tejashree17.pem /home/hduser/.ssh/
**chown hduser:hadoop /home/hduser/.ssh/multi.pem
chown hduser:hadoop /home/hduser/.ssh/tejashree17.pem
exit
su hduser
cd
cd .ssh/
ls
cd



# Install Java 8 (Open-JDK)
sudo apt install default-jdk -y
java -version


# Download and Install Hadoop
wget -c https://www-us.apache.org/dist/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz
ls
tar -xzf hadoop-2.9.2.tar.gz
ls
sudo mv hadoop-2.9.2 /usr/local/hadoop
sudo chown -R hduser:hadoop /usr/local/hadoop

# Set Enviornment Variable
readlink -f $(which java)
nano ~/.bashrc

# -- HADOOP ENVIRONMENT VARIABLES START -- #
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export PATH=$PATH:/usr/local/hadoop/bin/
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export PDSH_RCMD_TYPE=ssh
# -- HADOOP ENVIRONMENT VARIABLES END -- #

source ~/.bashrc
echo $PATH
cd /usr/local/hadoop/etc/hadoop/
ls

#Update hadoop-env.sh
nano hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_LOG_DIR=/var/log/hadoop
cd
sudo mkdir /var/log/hadoop/
sudo chown -R hduser:hadoop /var/log/hadoop

#Disable IPV6
sudo nano /etc/sysctl.conf
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
sudo sysctl -p

#Disable FireWall iptables
sudo iptables -L -n
sudo ufw status
sudo ufw disable






#Disabling Transparent Hugepage Compaction
--->> #Red Hat/CentOS: /sys/kernel/mm/redhat_transparent_hugepage/defrag
--->> #Ubuntu/Debian, OEL, SLES: /sys/kernel/mm/transparent_hugepage/defrag

cat /sys/kernel/mm/transparent_hugepage/defrag
sudo sed -i '/exit 0/d' /etc/rc.local

sudo su -c 'cat >>/etc/rc.local <<EOL
if test -f /sys/kernel/mm/transparent_hugepage/enabled; then
  echo never > /sys/kernel/mm/transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/transparent_hugepage/defrag; then
   echo never > /sys/kernel/mm/transparent_hugepage/defrag 
fi
exit 0
EOL'

sudo -i
source /etc/rc.local

