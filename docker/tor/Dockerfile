FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel flashplugin-nonfree
RUN apt-get install -y xz-utils

# Change to your UID/GID
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user

ADD tor-browser-bundle.pub /home/user/tor-browser-bundle.pub 
ADD https://www.torproject.org/dist/torbrowser/5.5/tor-browser-linux64-5.5_en-US.tar.xz /home/user/tor.tar.xz
ADD https://www.torproject.org/dist/torbrowser/5.5/tor-browser-linux64-5.5_en-US.tar.xz.asc /home/user/tor.tar.xz.asc
RUN cd /home/user && chown user:user tor-browser-bundle.pub tor.tar.xz tor.tar.xz.asc

USER user
ENV HOME /home/user
RUN gpg --import /home/user/tor-browser-bundle.pub
RUN gpg --verify /home/user/tor.tar.xz.asc
RUN cd /home/user && tar xJf tor.tar.xz
RUN cd /home/user && rm  tor-browser-bundle.pub tor.tar.xz tor.tar.xz.asc

CMD cd /home/user/tor-browser_en-US && ./start-tor-browser.desktop --verbose
