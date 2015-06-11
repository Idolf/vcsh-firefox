FROM debian
RUN apt-get update && apt-get install -y iceweasel

# Change to your UID/GID
RUN export uid=1000 gid=1000 && \
    mkdir -p /home/user && \
    echo "user:x:${uid}:${gid}:User,,,:/home/user:/bin/bash" >> /etc/passwd && \
    echo "user:x:${uid}:" >> /etc/group && \
    chown ${uid}:${gid} -R /home/user
USER user
ENV HOME /home/user
CMD /usr/bin/firefox -no-remote
