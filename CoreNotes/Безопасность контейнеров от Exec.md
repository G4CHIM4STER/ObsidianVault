___
Теги: #idea
Дата создания: 2023-08-24 14:12 
Последнее изменение: четверг 24-го августа 2023 14:12:48
<< [[2023-08-23]] | [[2023-08-25]] >> 
___
## Скопированная статья

As David mentions, once someone has access to the docker socket (either via API or with the `docker` CLI), that typically means they have root access to your host. It's trivial to use that access to run a privileged container with host namespaces and volume mounts that let the attacker do just about anything.

When you need to initialize a container with steps that run as root, I do recommend [gosu](https://stackoverflow.com/a/37931896/596285) over something like `su` since `su` was not designed for containers and will leave a process running as the root pid. Make sure that you `exec` the call to `gosu` and that will eliminate anything running as root. However, the user you start the container as is the same as the user used for `docker exec`, and since you need to start as root, your exec will run as root unless you override it with a `-u` flag.

There are additional steps you can take to lock down docker in general:

1. Use [user namespaces](https://docs.docker.com/engine/security/userns-remap/). These are defined on the entire daemon, require that you destroy all containers, and pull images again, since the uid mapping affects the storage of image layers. The user namespace offsets the uid's used by docker so that root inside the container is not root on the host, while inside the container you can still bind to low numbered ports and run administrative activities.
    
2. Consider [authz plugins](https://docs.docker.com/engine/extend/plugins_authorization/). Open policy agent and Twistlock are two that I know of, though I don't know if either would allow you to restrict the user of a `docker exec` command. They likely require that you give users a certificate to connect to docker rather than giving them direct access to the docker socket since the socket doesn't have any user details included in API requests it receives.
    
3. Consider [rootless docker](https://docs.docker.com/engine/security/rootless/). This is still experimental, but since docker is not running as root, it has no access back to the host to perform root activities, mitigating many of the issues seen when containers are run as root.

___
### Zero-Links
- 

### Links
- 
