# openshift-el This is OpenShift on a Enterprise Linux

and it will run OpenShift Origin 3 on CentOS 7
and we follow A successful Git branching model [1]

## dependencies

By now Vagrant required the registration plugin [2] to register and subscribe RHEL. Obviously a Red Hat subscription is required for this to work.

## Installation and Usage

### Prerequisites

This Vagrant Environment is based on RHEL7, so you will need a RH Account to subscribe your VM.

```
$ vagrant plugin list
vagrant-libvirt (0.0.24)
vagrant-registration (0.0.6)
```

### Installation

```
git clone https://github.com/goern/openshift-el.git
cd openshift-el
```

### Usage
```
export SUB_USERNAME=user@example.com
export SUB_PASSWORD=password
vagrant up
vagrant ssh master
```

`vagrant provision` will ensure that a given stats is configured, one ansible play will install a new kernel, some please be sure to `vagrant reload` the environment after first provisioning.

### Configuration

In general the Vagrantfile carries some meaningful default values, these will be written to .vagrant-openshift.json and reread on each vagrant run. Some variables may be overwritten by command line arguments. 

####`--number-of-minions
The number of VM configured to be a kubernetes minion, or openshift node.

## TODO

* migrate everything to CentOS
* get interface name from ansible for /etc/sysconfig/flanneld

[1] - http://nvie.com/posts/a-successful-git-branching-model/

[2] - https://github.com/whitel/vagrant-registration
