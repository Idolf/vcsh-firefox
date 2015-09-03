FROM debian
RUN apt-get update && apt-get install -y iceweasel
RUN apt-get install -y xz-utils

# Change to your UID/GID
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user

ADD https://www.torproject.org/dist/torbrowser/5.0.1/tor-browser-linux64-5.0.1_en-US.tar.xz /home/user/tor.tar.xz
RUN cd /home/user && tar xJf /home/user/tor.tar.xz  && rm /home/user/tor.tar.xz

USER user
ENV HOME /home/user
CMD cd /home/user/tor-browser_en-US && ./start-tor-browser.desktop --verbose
