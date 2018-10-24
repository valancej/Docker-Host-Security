# Securing the Docker Host

## Introduction

A short while ago we published a blog on Docker security called [Docker Security Best Practices: Part 1](https://anchore.com/blog/docker-security-best-practices-part-1/). We structured it by briefly discussing a comprehensive approach to security the entire container stack from top to bottom. This involves securing the underlying host operating system, the container images themselves, and the container runtime. In this post, we will discuss securing the host operating system in a bit more detail. In short, containerized applications are only as secure as the underlying host, as containers share the operating system kernel. There are some important operating system security best approaches that will strength this layer of the container stack and improve the overall security posture.

## Choosing an OS

Put simply, if you are running containers on a general-purpose operating system, a good question to ask is, am I able to use a container specific operating system instead? Minimizing the attack surface will greatly reduce the security threats to the hosts running containers. If possible, organizations should strive to use container-specific operating systems. These operating systems are specifically designed and optimized for hosting containers. A great place to start is [Alpine Linux](https://alpinelinux.org/). Why should you use these OSs? For the most part, they typically feature read-only file systems and employ other hardening best practices by default. These are key things to consider when managing the these pieces of infrastructure. With a general-purpose OS, these are more practices and features organizations would need to manage on their own. If a general-purpose operating system is being used, attack surface still needs to be reduced, even more so. Host that run containers should not run any unnecessary system services or non-containerized applications. Lastly, every host operating system should consistently be scanned and monitored for vulnerabilies. If vulnerabilites are found, patches should be applied, and the OS should be updated. 

## OS vulnerabilites and updates

Once an operating system has been chosen, it is important for organizations to standardize on best practices and tooling to validate the versioning of packages/components contained within the base OS. Regardless of whether or not a container-specific OS has been chosen, they will still contain components that may become vulnerable and require triage. Organizations should use tools provided by the OS vendor or other trusted organizations to regularly scan and check for updates to components. Even though security vulnerabiles may not be present in a particular OS packages, if the vendor recommends an update to a component, they should be updated. If it is simpler for an organization to redeploy and up to date OS, that is also an option. Since we are dealing with containized applications, the host should remain immutable in the same manner containers should be. Organizations should not be persisting data uniquely within the OS. This will greatly reduce the attack surface as well, and avoid any drift. Lastly, Docker frequently updates it's software with fixes and features. By staying up to date, vulnerabilites are mitigated, and attackers who know of common exploits are kept in check.  

## User access rights

All authentication directly to the OS should be audited and logged. System administrators should only grant access to the appropriate users, along with using keys for remote logins. Additionally, firewalls should be implemented, and access should only be allowed on trusted networks. Organizations should implement a strong log monitoring and manages process that terminates in a dedicated log storage host, with restricted access. Additionally, the Docker daemon currently requires 'root' privileges. A user need to be added to the 'docker' group to gain access rights. Organizations should remove any users from the 'docker' group that are not trusted, or do not need privileges. 

## Host file system

Organizations should be make sure containers are run with the minimal set of file system permission required. Containers should not be able to mount sensitive directories on a host's file system, especially conatining configuration settings for the OS. This is a bad practice and should be avoided. An attack would then be able to execute any command that the docker service can run, which typically provides access to the whole host system as the docker service runs as root (see above).

## Audit considerations

Audits should be conducted on the following: 

- Docker daemon activities should be audited
- Docker files and directories should be audited, if applicable.
    - `/var/lib/docker`
    - `/etc/docker`
    - `docker.service`
    - `docker.socket`
    - `/etc/default/docker`
    - `/etc/docker/daemon.json`
    - `/usr/bin/docker-containerd`
    - `/usr/bin/docker-runc`

## Conclusion

When running containerized applications, specifically Docker, the considerations above will greatly mitigate the threats to any host operating system. As mentioned in the introduction, containers are only as secure as the underlying host, so security teams need to follow up on the outlined considersations and apply the appropriate countermeasures.

