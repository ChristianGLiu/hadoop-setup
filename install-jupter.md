Step 02: Connect via ssh and install needed libraries
You may connect via ssh: root@127.0.0.1:2222 / password: hadoop
install needed libraries:
yum install nano centos-release-scl zlib-devel \
bzip2-devel openssl-devel ncurses-devel \
sqlite-devel readline-devel tk-devel \
gdbm-devel db4-devel libpcap-devel xz-devel \
libpng-devel libjpg-devel atlas-devel
yum groupinstall "Development tools"
Step 03: Install Python 2.7
Since HDP 2.4 Sandbox only comes with Python 2.6 installed, but Jupyter requires at least python 2.7, we have to install it manually. To do without recompiling it ourselves, we can use the CentOS Software Collections Repository, which we installed in the previews step (centos-release-SCL). So just type

yum install python27
to get python 2.7.

Now we have to activate it for this session:

source /opt/rh/python27/enable
Step 04: Install pip for Python 2.7
To get Jupyter and some more python libraries, we will use pip - the python package manager. So let's install pip and upgrade pip to the latest version:

wget https://bootstrap.pypa.io/ez_setup.py -O - | python
easy_install-2.7 pip
pip install --upgrade pip
Step 05: Install Jupyter (IPython Notebook)
First, we will install some more "standard" python libraries for data scientist to have something to toy around with. Afterwards, we will install Jupyter (IPython Notebook). This might take a while.

pip install --upgrade numpy scipy \
pandas scikit-learn tornado pyzmq \
pygments matplotlib jinja2 jsonschema

pip install jupyter
Step 06: Create Jupyter startup script
Create a new file in your home directory

nano ~/start_jupyter.sh
and add the following content to it.

#!/bin/bash
source /opt/rh/python27/enable
IPYTHON_OPTS="notebook --port 8889 \
--notebook-dir='/usr/hdp/current/spark-client/' \
--ip='*' --no-browser" pyspark
Afterwards make this script executable

chmod +x ~/start_jupyter.sh
Step 07: Start IPython Notebook
You're done! You may start Jupyter from your home directory with the command

./start_jupyter.sh
