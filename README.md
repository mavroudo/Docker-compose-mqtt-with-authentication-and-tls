# Mqtt Broker using both Authentication and TLS

This is a custom docker image to automatically generate the necessary files that enable security.
More specifically, the Dockerfile will generate the authorized users based on the environmental
variable **USERS**. The format of the variable is follows the format of the file used from the 
mosquitto_passwd, e.g.
```
username:password\nusername2:password2
```
Next the generation of the certifications is using the openssl. The password for the ca and the name
of the server are both stored in variables, so they can be easily changed.

## Deploy mqtt service
```
docker-compose build
docker-compose up
```

This will automatically copy the ca.pem file and paste it in mqtt/ca/ folder.

### Example of mosquitto_sub and mosquitto_pub
```
mosquitto_sub -t test -m 'test' -u user2 -P pass2 -p 8883 --cafile mqtt/ca/ca.pem --insecure
```
```
mosquitto_pub -t test -u user2 -P pass2 -p 8883 --cafile mqtt/ca/ca.pem --insecure
```

PS. The insecure command at the end means that the ca will not be authenticated, which is ok since 
the certificates are self-signed. \
PS2. The ca.pem file will be owned by the root user, in order to ensure that every file will have access
add a line in docker-compose (bellow the line that changes the mode), that will change the owner to you.