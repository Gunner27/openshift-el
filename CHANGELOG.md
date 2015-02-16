# 0.4.0 (2015-02-16)

This is the last release that will include Vagrant, next release will be standalone ansible playbook that could be used with oh-my-vagrant or standalone...


# 0.3.0 (2015-02-04)

Fixes:
 - updated flannel and kernel RPM to fix a kernel freeze related to vxlan, see https://bugzilla.redhat.com/show_bug.cgi?id=1189050

Known Issues:

 - we need to port that to CentOS7, as a Red Hat internal base box is used.


# 0.2.0 (2015-02-04)

Features:

 - added ansible roles for docker, flannel, etcd and openshift-origin
 - flannel overlay network is set up by ansible tasks

Known Issues:

 - we need to port that to CentOS7, as a Red Hat internal base box is used.
 - kernel seems to freeze on overlay network traffic, see also https://bugzilla.redhat.com/show_bug.cgi?id=1189050


# 0.1.0 (2015-02-02)

Features:

 - this release will start n minions and a master, it will use RHEL7 as a Vagrant base box.

Known Issues:

 - we need to port that to CentOS7, as a Red Hat internal base box is used.
