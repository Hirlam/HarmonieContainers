# HarmonieContainers

A docker wrap for automatic testing of the Harmonie using GCC.

Harmonie containers have been developed using rootless podman (v1.6.4 on RHEL7 and v3.2.3 on CentOS 8). 

For v1.6.4 (RHEL7) you may need to increase the limits on the number of open files (ulimit -n). This can be set (by your sys-admin) in /etc/security/limits.conf:
```
username         soft    nofile          2048
username         hard    nofile          20480
```

## CentOS 8 Harmonie (with ecFlow) container

For Harmonie we can create a CentOS 8 container and compile the code using GCC+OpenMPI.

Points to note:
 * The user ID is used to define the container user ID
 * A single port (based on user ID) is exposed so that Harmonie can be monitored via an ecFlow-5 viewer on your PC
 * Set some environment variables:
```
USER_ID="$(id -u)"
CONTAINER_ECF_PORT=`expr ${USER_ID} + 1500`
HOST_ECF_PORT=`expr ${USER_ID} + 11500`
```
 * Build and start the container
```
podman build --build-arg SSH_KEY="$(cat ~/.ssh/id_rsa)" --build-arg USER_ID="${USER_ID}" --tag centos8_harmonie -f docker/centos8_harmonie.docker
podman run -dit -p ${HOST_ECF_PORT}:${CONTAINER_ECF_PORT} --name c8_harm localhost/centos8_harmonie
```
 * Login (or attach to the new container)
```
podman attach c8_harm
```
 * Login again
 ```
 podman start c8_harm
 podman attach c8_harm
 ```

## CentOS 8 MUSC (without ecFlow)
```
podman build --build-arg SSH_KEY="$(cat ~/.ssh/id_rsa)" --tag centos8_musc -f docker/centos8_musc.docker
podman run -dit -p ${HOST_ECF_PORT}:${CONTAINER_ECF_PORT} --name c8_musc localhost/centos8_musc
 * Login (or attach to the new container)
```
podman attach c8_musc
```
 * Login again
 ```
 podman start c8_musc
 podman attach c8_musc
 ```
