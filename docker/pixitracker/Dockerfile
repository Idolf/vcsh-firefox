FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel ca-certificates pulseaudio gnupg && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)
RUN apt-get install bzip2 curl -y
RUN apt-get install libsdl2-2.0-0 -y
#RUN mkdir /opt/firefox && cd /opt/firefox && curl -L -v 'https://download.mozilla.org/?product=firefox-latest-ssl&os=linux64&lang=en-US' | tar jxf - --strip=1
RUN apt-get purge iceweasel firefox-esr -y

# Setting up user environment
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user

ADD pixitracker.tar.bz2 /opt/pixitracker

USER user
ENV HOME /home/user
#CMD pulseaudio & exec firefox -no-remote about:blank
CMD exec /opt/pixitracker/pixitracker/START_LINUX_X86_64
