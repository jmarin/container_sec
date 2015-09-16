# Container Security with OpenSCAP

## Introduction

## Running

This project requires [Vagrant](https://www.vagrantup.com/), [Virtualbox](https://www.virtualbox.org/wiki/Downloads) and [Ansible](http://www.ansible.com) to run. It has been tested with `Vagrant 1.7.2`,  `VirtualBox 4.3.20` and `Ansible 1.8.3`, but newer versions should work. Older versions may or may not work. 

In order to bring up the sample VM, from the root directory of this repository: 

```
vagrant up
```

This will boot the virtual machine, which is configured with CentOS 7 as the host operating system. The ansible build script will install some dependencies as well as the OpenSCAP tools, including the OpenSCAP Security Guide. It will also copy recent files from building the Scap Security Guide from source code (see reference down below)

Once the virtual machine is done building, you can connect to it with `vagrant ssh` and make sure the proper environment path is set up:

```
sudo su
export PATH:$PATH:/usr/local/bin
```

To test compliance of the host operating system, you can run the following:

```
oscap xccdf eval --profile stig-rhel7-server-upstream ssg-centos7-xccdf.xml
```

You will see many `fail` messages since this virtual machine has not been hardened according to the Red Hat Draft STIG for RHEL7


The process for scanning Docker images is very similar. The rest of the commands need to be run as root by default (to be able to access docker commands).

To test for image/container compliance, first pull down a CentOS/RHEL based image:

```
docker pull rhel7.1
```

Confirm that the image has been pulled by checking the output of `docker images`

Scan the image by issuing the following command. 
```
oscap-docker image rhel7.1 xccdf eval --profile stig-rhel7-server-upstream --report /tmp/rhel7syslog.html ssg-rhel7-xccdf.xml
```

This checks agains the Pre-release Draft STIG for Red Hat Enterprise Linux 7 Server profile. In order to see the profiles available for this definition you can run:

```
oscap info "ssg-rhel7-xccdf.xml"
```


## References

The following resources have been helpful in putting together this proof of concept
 
* [SCAP](http://scap.nist.gov/)
* [OpenSCAP Documentation](http://www.open-scap.org/page/Documentation)
* [OpenSCAP Container Compliance](https://github.com/OpenSCAP/container-compliance) 
* [Scanning container images on Red Hat Enterprise Linux Atomic Host using oscap-docker and scap-security-guide](https://jlieskov.wordpress.com/2015/07/17/scanning-container-images-on-red-hat-enterprise-linux-atomic-host-v-7-using-oscap-docker-and-scap-security-guide/)
* [GovReady CentOS OpenSCAP example](https://github.com/GovReady/centos-openscap)
* [OpenSCAP scap-security-guide source code](https://github.com/OpenSCAP/scap-security-guide)
* [Secure RHEL6 with OpenSCAP](http://mrbluecoat.blogspot.com/2014/06/secure-rhel6-with-openscap.html)
* [SCAP CVE Audit](https://dazdaztech.wordpress.com/2014/02/07/scap-cve-audit/)
* [Red Hat Linux 7 Security Guide](https://linux.web.cern.ch/linux/centos7/docs/rhel/Red_Hat_Enterprise_Linux-7-Security_Guide-en-US.pdf)
* [Red Hat Container Images](https://access.redhat.com/search/#/container-images)
