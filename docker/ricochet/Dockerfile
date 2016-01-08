FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel flashplugin-nonfree
RUN apt-get install -y bzip2

# Change to your UID/GID
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user

ADD ricochet.pub /home/user/ricochet.pub
ADD https://ricochet.im/releases/1.1.1/ricochet-1.1.1-static-x86_64.tar.bz2 /home/user/ricochet.tar.bz2
ADD https://ricochet.im/releases/1.1.1/ricochet-1.1.1-static-x86_64.tar.bz2.asc /home/user/ricochet.tar.bz2.asc
RUN cd /home/user && chown user:user ricochet.pub ricochet.tar.bz2 ricochet.tar.bz2.asc

USER user
ENV HOME /home/user
RUN gpg --import /home/user/ricochet.pub
RUN gpg --verify /home/user/ricochet.tar.bz2.asc
RUN cd /home/user && tar xjf ricochet.tar.bz2
RUN cd /home/user && rm  ricochet.pub ricochet.tar.bz2 ricochet.tar.bz2.asc

CMD cd /home/user/ricochet && ./ricochet
