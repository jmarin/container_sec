# Container Security with OpenSCAP

## Introduction

## Running

This project requires [Vagrant](https://www.vagrantup.com/), [Virtualbox](https://www.virtualbox.org/wiki/Downloads) and [Ansible](http://www.ansible.com) to run. It has been tested with `Vagrant 1.7.2`,  `VirtualBox 4.3.20` and `Ansible 1.8.3`, but newer versions should work. Older versions may or may not work. 

In order to bring up the sample VM, from the root directory of this repository: 

```
vagrant up
```

This will boot the virtual machine, which is configured with CentOS 7 as the host operating system. The ansible build script will install some dependencies as well as the OpenSCAP tools, including the OpenSCAP Security Guide. For a newer version of the security guide, see the source code repository listed further down below in the references section. 

To test a container, first pull down a CentOS/RHEL based image:

```
docker pull rhel7/rsyslog
```

Confirm that the image has been pulled by checking the output of `docker images`

Scan the image by issuing the following command (streaming mode). 
```
oscap-docker image rhel7/rsyslog xccdf eval --profile xccdf_org.ssgproject.content_profile_rht-ccp --report /tmp/rhel7syslog.html /usr/share/xml/scap/ssg/content/ssg-rhel7-ds.xml
```

This checks agains the rht_ccp profile of the scap-security-guide. In order to see the profiles available for this definition you can run:

```
oscap info "/usr/share/xml/scap/ssg/content/ssg-rhel7-ds.xml"
```


## References

The following resources have been helpful in putting together this proof of concept
 
* [OpenSCAP Documentation](http://www.open-scap.org/page/Documentation)
* [OpenSCAP Container Compliance](https://github.com/OpenSCAP/container-compliance) 
* [Scanning container images on Red Hat Enterprise Linux Atomic Host using oscap-docker and scap-security-guide](https://jlieskov.wordpress.com/2015/07/17/scanning-container-images-on-red-hat-enterprise-linux-atomic-host-v-7-using-oscap-docker-and-scap-security-guide/)
* [GovReady CentOS OpenSCAP example](https://github.com/GovReady/centos-openscap)
* [OpenSCAP scap-security-guide source code](https://github.com/OpenSCAP/scap-security-guide)
* [Secure RHEL6 with OpenSCAP](http://mrbluecoat.blogspot.com/2014/06/secure-rhel6-with-openscap.html)
* [SCAP CVE Audit](https://dazdaztech.wordpress.com/2014/02/07/scap-cve-audit/)
* [Red Hat Linux 7 Security Guide](https://linux.web.cern.ch/linux/centos7/docs/rhel/Red_Hat_Enterprise_Linux-7-Security_Guide-en-US.pdf)

