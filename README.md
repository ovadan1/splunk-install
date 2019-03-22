# splunk-install
This will install Splunk clustered environment
First set will install Splunk software.
Second set will setup Indexer/SH clusters.
There is no limit on the number of instances, you can add as many instances as you in the instance declaration section of the script.


Steps:

1)
A)
Create “mylibs” folder under  home directory:
Copy these two files into it "~/mylibs"
libinst.sh
remoteinst.sh

B)
Create “myinstalls” sub directory:
~/mylibs/myinstalls

Copy these 4 files:
cli.sh
cluster.sh
mylibrary1.sh
tinist.sh
————

Edit the following files:
1) Edit remoteinst.sh file:

A) Under this section provide all host names or ip address where you want to install Splunk:
E.g.
declare -a arr_hosts=(
"host_server01.xxxxx.com"
"host_server02.xxxxx.com”
)
B) set tar file and license file names:
install_file="splunk-7.1.1-8f0ead9ec3db-Linux-x86_64.tgz"
license_file="Splunk_Enterprise_NFR_Q2FY19.lic”

2) Edit cli.sh file. Set:

IDX_SECRET="idxcluster"
MASTER_URI="https://host_server01.xxxxx.com:8089"
MASTER="host_server01.xxxxx.com"

IDX_REPLICATION_PORT="9100"
SHC_REPLICATION_PORT="8100"
IDX_REPLICATION_FACTOR="2"
IDX_SEARCH_FACTOR="2"


ADMIN_USER="admin"
ADMIN_PSWD="admin"  # this can be used for any pre 7.1.x installs
#ADMIN_PSWD="administrator" # set this for 7.1.x installs any other password that meets requirment

DEPLOYER="https://host_server07.xxxxx.com:8089"
CAPTAIN="host_server06.xxxxx.com"
OS_user="root"

# declare search cluster members
declare -a arr_SHC_members=(
"host_server04.xxxxx.com"
"host_server05.xxxxx.com"
"host_server06.xxxxx.com”

# declare cluster members
declare -a arr_CLUSTER_members=(
"host_server02.xxxxx.com"
"host_server03.xxxxx.com”

3)
Edit tinist.sh file.

Set:
LICENSEPATH="/opt/Splunk_Enterprise_NFR_Q2FY19.lic"
SPL_DIR="splunk"
DEST_DIR="/opt/splunk"
#---- splunk admin password
ADMIN_USER="admin"
ADMIN_PSWD="admin"
#ADMIN_PSWD="administrator" # use this for 7.1.x installs. it has password requirments

fname="splunk-7.0.3-fa31da744b51-Linux-x86_64.tgz”

4)
Copy tar (splunk-7.0.3-fa31da744b51-Linux-x86_64.tgz) and license files onto
"~/mylibs/myinstalls” directories.

5) Run:
Run ~/mylibs/remoteinst.sh
This will install Splunk enteprise on all instances.

6) Run cluster setup script:

~/mylibs/myinstalls/cli.sh


NOTE:
Before running the scripts, make sure to set $SPLUNK_HOME on all machines.
I used SSH command to update .bash_profile. In the below example t.txt file would list of all instance names.

for i in `cat t.txt`; do ssh root@$i " echo "export PATH" >> /root/.bash_profile"; done
for i in `cat t.txt`; do ssh root@$i "hostname; echo "PATH=$PATH:$HOME/bin:/opt/splunk/bin" >> /root/.bash_profile"; done
for i in `cat t.txt`; do ssh root@$i " echo "export SPLUNK_HOME=/opt/splunk" >> /root/.bash_profile"; done
for i in `cat t.txt`; do ssh root@$i " echo "export PATH" >> /root/.bash_profile"; done
