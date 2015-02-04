# openshift-el This is OpenShift on a Enterprise Linux

and it will run OpenShift Origin 3 on CentOS 7
and we follow A successful Git branching model [1]

## dependencies

By now Vagrant required the registration plugin [2] to register and subscribe RHEL. Obviously a Red Hat subscription is required for this to work.

## Installation and Usage

### Prerequisites
```
$ vagrant plugin list
vagrant-libvirt (0.0.24)
vagrant-registration (0.0.6)
```

```
git clone https://github.com/goern/openshift-el.git
cd openshift-el
export SUB_USERNAME=user@example.com
export SUB_PASSWORD=password
vagrant up
vagrant ssh master
```

## TODO

* migrate everything to CentOS
* get interface name from ansible for /etc/sysconfig/flanneld

[1] - http://nvie.com/posts/a-successful-git-branching-model/

[2] - https://github.com/whitel/vagrant-registration
