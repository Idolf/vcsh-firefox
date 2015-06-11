FROM debian
RUN apt-get update && apt-get install -y iceweasel

# Change to your UID/GID
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user
USER user
ENV HOME /home/user
CMD /usr/bin/firefox -no-remote
