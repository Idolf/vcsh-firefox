FROM debian
RUN apt-get update && apt-get install -y iceweasel

RUN export uid=10000 gid=10000 && \
    mkdir -p /home/user && \
    echo "user:x:${uid}:${gid}:User,,,:/home/user:/bin/bash" >> /etc/passwd && \
    echo "user:x:${uid}:" >> /etc/group && \
    chown ${uid}:${gid} -R /home/user
USER user
ENV HOME /home/user
CMD /usr/bin/firefox -no-remote
