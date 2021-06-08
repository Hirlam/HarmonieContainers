# gcc-harmonie-test
A docker wrap for automatic testing of the Harmonie benchmark using GCC

# CentOS 8 container

For Harmonie we can create a CentOS 8 container and compile the code using GCC+OpenMPI.

Points to note:
 * The user ID is used to define the container user ID
 * A single port (based on user ID) is exposed so that Harmonie can be monitored via an ecFlow-5 viewer on your PC

```
USER_ID="$(id -u)"
CONTAINER_ECF_PORT=`expr ${USER_ID} + 1500`
HOST_ECF_PORT=`expr ${USER_ID} + 11500`
podman build --build-arg SSH_KEY="$(cat ~/.ssh/id_rsa)" --build-arg USER_ID="${USER_ID}" --tag centos8_harmonie -f docker/centos8_harmonie.docker
podman run -dit -p ${HOST_ECF_PORT}:${CONTAINER_ECF_PORT} --name c8_harm localhost/centos8_harmonie
podman attach c8_harm
```
