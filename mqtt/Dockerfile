FROM ansi/mosquitto

#it will automatically create the passwordfile, can be moved to a env file in docker compose's level
ENV USERS='username:password\nuser2:pass2'
USER root
ENV capass='12345'
ENV mqttaddr='127.0.0.1'
RUN mkdir -p /etc/mosquitto/ &&\
    mkdir -p /etc/mosquitto/seq &&\
    mkdir -p /etc/mosquitto/ca_certificate \
USER mosquitto
VOLUME /etc/mosquitto/ca_certificate
RUN echo $USERS > users &&\
    mosquitto_passwd -U users &&\
    mv users /etc/mosquitto/users
COPY mosquitto.conf /etc/mosquitto/mosquitto.conf


#next are reffering to the creation of openssl
RUN echo '$capass\n$capass' | openssl genrsa -des3 -out /etc/mosquitto/seq/ca.key 2048 &&\
    echo '$capass\n\n\n\n\n\n\n' | openssl req -new -x509 -days 1826 -key /etc/mosquitto/seq/ca.key -out /etc/mosquitto/seq/ca.crt &&\
    openssl genrsa -out /etc/mosquitto/seq/server.key 2048 &&\
    echo '\n\n\n\n\n$mqttaddr\n\n\n' | openssl req -new -out /etc/mosquitto/seq/server.csr -key /etc/mosquitto/seq/server.key &&\
    echo '$capass\n' | openssl x509 -req -in /etc/mosquitto/seq/server.csr -CA /etc/mosquitto/seq/ca.crt -CAkey /etc/mosquitto/seq/ca.key -CAcreateserial -out /etc/mosquitto/seq/server.crt -days 360 && \
    openssl x509 -in /etc/mosquitto/seq/ca.crt -out /etc/mosquitto/seq/ca.pem -outform PEM








#ENTRYPOINT ["/usr/local/sbin/mosquitto","-c","/etc/mosquitto/mosquitto.conf"]