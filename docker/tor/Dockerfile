FROM debian
RUN apt-get update && apt-get install -y iceweasel
RUN apt-get install -y xz-utils
RUN apt-get install -y strace

# Change to your UID/GID
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user

ADD https://www.torproject.org/dist/torbrowser/4.5.1/tor-browser-linux64-4.5.1_en-US.tar.xz /home/user/tor.tar.xz
RUN cd /home/user && tar xJf /home/user/tor.tar.xz  && rm /home/user/tor.tar.xz

USER user
ENV HOME /home/user
CMD cd /home/user/tor-browser_en-US && ./start-tor-browser.desktop --verbose
