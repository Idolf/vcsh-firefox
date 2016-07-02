FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel flashplugin-nonfree ca-certificates && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)

# Change to your UID/GID
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user
USER user
ENV HOME /home/user
CMD /usr/bin/firefox -no-remote
