# Base
FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel ca-certificates pulseaudio gnupg && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)
RUN apt-get install -y xz-utils

RUN echo 'flat-volumes = yes' > /etc/pulse/daemon.conf
RUN echo 'autospawn = yes' > /etc/pulse/client.conf
RUN echo IyBSZXBsYWNlIHRoZSAqZW50aXJlKiBjb250ZW50IG9mIHRoaXMgZmlsZSB3aXRoIHRoZXNlIGZldyBsaW5lcyBhbmQKIyByZWFkIHRoZSBjb21tZW50cwoKLmZhaWwKICAgICMgU2V0IHRzY2hlZD0wIGhlcmUgaWYgeW91IGV4cGVyaWVuY2UgZ2xpdGNoeSBwbGF5YmFjay4gVGhpcyB3aWxsCiAgICAjIHJldmVydCBiYWNrIHRvIGludGVycnVwdC1iYXNlZCBzY2hlZHVsaW5nIGFuZCBzaG91bGQgZml4IGl0LgogICAgIwogICAgIyBSZXBsYWNlIHRoZSBkZXZpY2U9IHBhcnQgaWYgeW91IHdhbnQgcHVsc2UgdG8gdXNlIGEgc3BlY2lmaWMgZGV2aWNlCiAgICAjIHN1Y2ggYXMgImRtaXgiIGFuZCAiZHNub29wIiBzbyBpdCBkb2Vzbid0IGxvY2sgYW4gaHc6IGRldmljZS4KICAgIAogICAgIyBJTlBVVC9SRUNPUkQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1hbHNhLXNvdXJjZSBkZXZpY2U9ImRlZmF1bHQiIHRzY2hlZD0xCiAgICAKICAgICMgT1VUUFVUL1BMQVlCQUNLCiAgICBsb2FkLW1vZHVsZSBtb2R1bGUtYWxzYS1zaW5rIGRldmljZT0iZGVmYXVsdCIgdHNjaGVkPTEgCiAgICAKICAgICMgQWNjZXB0IGNsaWVudHMgLS0gdmVyeSBpbXBvcnRhbnQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1uYXRpdmUtcHJvdG9jb2wtdW5peAoKLm5vZmFpbAouaWZleGlzdHMgbW9kdWxlLXgxMS1wdWJsaXNoLnNvCiAgICAjIFB1Ymxpc2ggdG8gWDExIHNvIHRoZSBjbGllbnRzIGtub3cgaG93IHRvIGNvbm5lY3QgdG8gUHVsc2UuIFdpbGwKICAgICMgY2xlYXIgaXRzZWxmIG9uIHVubG9hZC4KICAgIGxvYWQtbW9kdWxlIG1vZHVsZS14MTEtcHVibGlzaAouZW5kaWYK | base64 -d > /etc/pulse/default.pa

# Change to your UID/GID
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user

# Get TBB
ADD tor-browser-bundle.pub /home/user/tor-browser-bundle.pub
ADD https://www.torproject.org/dist/torbrowser/7.0.11/tor-browser-linux64-7.0.11_en-US.tar.xz /home/user/tor.tar.xz
ADD https://www.torproject.org/dist/torbrowser/7.0.11/tor-browser-linux64-7.0.11_en-US.tar.xz.asc /home/user/tor.tar.xz.asc
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
CMD pulseaudio & cd /home/user/tor-browser_en-US && exec ./start-tor-browser.desktop --verbose
