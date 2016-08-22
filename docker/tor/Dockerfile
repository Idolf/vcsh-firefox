# Base
FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel flashplugin-nonfree ca-certificates && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)
RUN apt-get install -y xz-utils

# Change to your UID/GID
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user

# Get TBB
ADD tor-browser-bundle.pub /home/user/tor-browser-bundle.pub
ADD https://www.torproject.org/dist/torbrowser/6.0.4/tor-browser-linux64-6.0.4_en-US.tar.xz /home/user/tor.tar.xz
ADD https://www.torproject.org/dist/torbrowser/6.0.4/tor-browser-linux64-6.0.4_en-US.tar.xz.asc /home/user/tor.tar.xz.asc
RUN cd /home/user && chown user:user tor-browser-bundle.pub tor.tar.xz tor.tar.xz.asc

# Change to user
USER user
ENV HOME /home/user

# Verify and install TBB
RUN gpg --import /home/user/tor-browser-bundle.pub
RUN gpg --verify /home/user/tor.tar.xz.asc
RUN cd /home/user && tar xJf tor.tar.xz
RUN cd /home/user && rm  tor-browser-bundle.pub tor.tar.xz tor.tar.xz.asc

# Disable startup prompt
RUN echo 'pref("extensions.torlauncher.prompt_at_startup", false);' >> /home/user/tor-browser_en-US/Browser/TorBrowser/Data/Browser/profile.default/preferences/extension-overrides.js
# Set high security as default
RUN echo 'pref("extensions.torbutton.security_slider", 2);' >> /home/user/tor-browser_en-US/Browser/TorBrowser/Data/Browser/profile.default/preferences/extension-overrides.js
RUN echo 'pref("extensions.torbutton.show_slider_notification", false);' >> /home/user/tor-browser_en-US/Browser/TorBrowser/Data/Browser/profile.default/preferences/extension-overrides.js
# XMonad tiling fix
RUN echo 'pref("extensions.torbutton.resize_new_windows", 0);' >> /home/user/tor-browser_en-US/Browser/TorBrowser/Data/Browser/profile.default/preferences/extension-overrides.js
# Don't preload hovered links
RUN echo 'pref("network.http.speculative-parallel-limit", 0);' >> /home/user/tor-browser_en-US/Browser/TorBrowser/Data/Browser/profile.default/preferences/extension-overrides.js

# It's go time
CMD cd /home/user/tor-browser_en-US && ./start-tor-browser.desktop --verbose
