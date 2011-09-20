This page outlines what you need to do to get a Storm development environment set up. In summary, the steps are:

1. Install native dependencies ([ZeroMQ](http://download.zeromq.org/) and [JZMQ](https://github.com/nathanmarz/jzmq))
2. Download a [Storm release](http://github.com/nathanmarz/storm/downloads) , unpack it, and put the unpacked `bin/` directory on your PATH
3. To be able to start and stop topologies on a remote cluster, put the cluster information in `~/.storm/storm.yaml`

More detail on each of these steps is below.

### What is a development environment?

Storm has two modes of operation: local mode and remote mode. In local mode, you can develop and test topologies completely in process on your local machine. In remote mode, you submit topologies for execution on a cluster of machines.

A Storm development environment has everything installed so that you can develop and test Storm topologies in local mode, package topologies for execution on a remote cluster, and submit/kill topologies on a remote cluster.

Let's quickly go over the relationship between your machine and a remote cluster. A Storm cluster is managed by a master node called "Nimbus". Your machine communicates with Nimbus to submit code (packaged as a jar) and topologies for execution on the cluster, and Nimbus will take care of distributing that code around the cluster and assigning workers to run your topology. Your machine uses a command line client called `storm` to communicate with Nimbus. The `storm` client is only used for remote mode; it is not used for developing and testing topologies in local mode.

### Install native dependencies

Installing ZeroMQ and JZMQ is usually straightforward. Sometimes, however, people run into issues with autoconf and get strange errors. If you run into any issues, please email the [Storm mailing list](http://groups.google.com/group/storm-user) or come get help in the #storm-user room on freenode. 

Storm has been tested with ZeroMQ 2.1.7, and this is the recommended ZeroMQ release that you install. You can download a ZeroMQ release [here](http://download.zeromq.org/). Installing ZeroMQ should look something like this:

```
wget http://download.zeromq.org/historic/zeromq-2.1.7.tar.gz
tar -xzf zeromq-2.1.7.tar.gz
cd zeromq-2.1.7
./configure
make
sudo make install
```

JZMQ is the Java bindings for ZeroMQ. JZMQ doesn't have any releases (we're working with them on that), so there is risk of a regression if you always install from the master branch. To prevent a regression from happening, you should instead install from [this fork](http://github.com/nathanmarz/jzmq) which is tested to work with Storm. Installing JZMQ should look something like this:

```
#install jzmq
git clone https://github.com/nathanmarz/jzmq.git
cd jzmq
./autogen.sh
./configure
make
sudo make install
```

To get the JZMQ build to work, you may need to do one or all of the following:

1. Set JAVA_HOME environment variable appropriately
2. Install Java dev package (more info [here](http://goo.gl/D8lI) for Mac OSX users)
3. Upgrade autoconf on your machine

If you run into any errors when running `./configure`, [this thread](http://stackoverflow.com/questions/3522248/how-do-i-compile-jzmq-for-zeromq-on-osx) may provide a solution.

### Installing a Storm release locally

If you want to be able to submit topologies to a remote cluster from your machine, you should install a Storm release locally. Installing a Storm release will give you the `storm` client that you can use to interact with remote clusters. To install Storm locally, download a release [from here](https://github.com/nathanmarz/storm/downloads) and unzip it somewhere on your computer. Then add the unpacked `bin/` directory onto your `PATH` and make sure the `bin/storm` script is executable.

Installing a Storm release locally is only for interacting with remote clusters. For developing and testing topologies in local mode, it is recommended that you use Maven to include Storm as a dev dependency for your project. You can read more about using Maven for this purpose on [[Maven]]. 

### Starting and stopping topologies on a remote cluster

The previous step installed the `storm` client on your machine which is used to communicate with remote Storm clusters. Now all you have to do is tell the client which Storm cluster to talk to. To do this, all you have to do is put the host address of the master in the `~/.storm/storm.yaml` file. It should look something like this:

```
nimbus.host: "123.45.678.890"
```

Alternatively, if you use the [storm-deploy](https://github.com/nathanmarz/storm-deploy) project to provision Storm clusters on AWS, it will automatically set up your ~/.storm/storm.yaml file. You can manually attach to a Storm cluster (or switch between multiple clusters) using the "attach" command, like so:

```
lein run :deploy --attach --name mystormcluster
```

More information is on the storm-deploy [wiki](https://github.com/nathanmarz/storm-deploy/wiki)