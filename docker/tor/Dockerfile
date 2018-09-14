# Base
FROM debian
RUN apt-get update && apt-get install -y iceweasel pulseaudio && \
    apt-get install -y xz-utils gnupg && \
    apt-get autoremove && apt-get clean

# Add user
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -m -u 1000 user

RUN echo 'flat-volumes = yes' > /etc/pulse/daemon.conf
RUN echo 'autospawn = yes' > /etc/pulse/client.conf
RUN echo IyBSZXBsYWNlIHRoZSAqZW50aXJlKiBjb250ZW50IG9mIHRoaXMgZmlsZSB3aXRoIHRoZXNlIGZldyBsaW5lcyBhbmQKIyByZWFkIHRoZSBjb21tZW50cwoKLmZhaWwKICAgICMgU2V0IHRzY2hlZD0wIGhlcmUgaWYgeW91IGV4cGVyaWVuY2UgZ2xpdGNoeSBwbGF5YmFjay4gVGhpcyB3aWxsCiAgICAjIHJldmVydCBiYWNrIHRvIGludGVycnVwdC1iYXNlZCBzY2hlZHVsaW5nIGFuZCBzaG91bGQgZml4IGl0LgogICAgIwogICAgIyBSZXBsYWNlIHRoZSBkZXZpY2U9IHBhcnQgaWYgeW91IHdhbnQgcHVsc2UgdG8gdXNlIGEgc3BlY2lmaWMgZGV2aWNlCiAgICAjIHN1Y2ggYXMgImRtaXgiIGFuZCAiZHNub29wIiBzbyBpdCBkb2Vzbid0IGxvY2sgYW4gaHc6IGRldmljZS4KICAgIAogICAgIyBJTlBVVC9SRUNPUkQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1hbHNhLXNvdXJjZSBkZXZpY2U9ImRlZmF1bHQiIHRzY2hlZD0xCiAgICAKICAgICMgT1VUUFVUL1BMQVlCQUNLCiAgICBsb2FkLW1vZHVsZSBtb2R1bGUtYWxzYS1zaW5rIGRldmljZT0iZGVmYXVsdCIgdHNjaGVkPTEgCiAgICAKICAgICMgQWNjZXB0IGNsaWVudHMgLS0gdmVyeSBpbXBvcnRhbnQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1uYXRpdmUtcHJvdG9jb2wtdW5peAoKLm5vZmFpbAouaWZleGlzdHMgbW9kdWxlLXgxMS1wdWJsaXNoLnNvCiAgICAjIFB1Ymxpc2ggdG8gWDExIHNvIHRoZSBjbGllbnRzIGtub3cgaG93IHRvIGNvbm5lY3QgdG8gUHVsc2UuIFdpbGwKICAgICMgY2xlYXIgaXRzZWxmIG9uIHVubG9hZC4KICAgIGxvYWQtbW9kdWxlIG1vZHVsZS14MTEtcHVibGlzaAouZW5kaWYK | base64 -d > /etc/pulse/default.pa

# Get TB files
COPY --chown=user:user tor-browser* /home/user/

# Change to user
USER user
ENV HOME /home/user

# Verify and install TBB
RUN gpg --import /home/user/tor-browser-bundle.pub
RUN gpg --verify /home/user/tor-browser.tar.xz.asc
RUN cd /home/user && tar xJf tor-browser.tar.xz
RUN cd /home/user && rm  tor-browser-bundle.pub tor-browser.tar.xz tor-browser.tar.xz.asc

# Disable startup prompt
RUN echo 'pref("extensions.torlauncher.prompt_at_startup", false);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
# Set high security as default
    echo 'pref("extensions.torbutton.security_slider", 2);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
    echo 'pref("extensions.torbutton.show_slider_notification", false);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
# XMonad tiling fix - potential fingerprinting vector
    echo 'pref("extensions.torbutton.resize_new_windows", 0);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
# Don't preload hovered links
    echo 'pref("network.http.speculative-parallel-limit", 0);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
# Change search engine to Startpage - DDG is pretty slow with no JS
    echo 'pref("browser.search.order.extra.1", "Startpage");' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
    echo 'pref("signon.rememberSignons", false);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
    echo 'pref("privacy.resistFingerprinting.autoDeclineNoUserInputCanvasPrompts", true);' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js && \
    echo 'pref("browser.search.defaultenginename", "Startpage");' >> /home/user/tor-browser_en-US/Browser/defaults/pref/channel-prefs.js

# It's go time
CMD pulseaudio & cd /home/user/tor-browser_en-US && exec ./start-tor-browser.desktop --verbose
