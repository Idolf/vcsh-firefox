FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel ca-certificates pulseaudio gnupg && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)
RUN apt-get install bzip2 curl -y
#RUN mkdir /opt/firefox && cd /opt/firefox && curl -L -v 'https://download.mozilla.org/?product=firefox-latest-ssl&os=linux64&lang=en-US' | tar jxf - --strip=1
RUN mkdir /opt/firefox && cd /opt/firefox && curl -L -v 'https://gitlab.com/Monsterovich/firefox-fuckpa/raw/master/62.0b4/firefox-62.0.en-US.linux-x86_64.tar.bz2?inline=false' | tar jxf - --strip=1
RUN apt-get purge iceweasel firefox-esr -y
ENV PATH "/opt/firefox:${PATH}"

# Setting up user environment
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user
#RUN echo 'pref("", "");' >> /opt/firefox/defaults/pref/firefox.js
RUN echo -n > /opt/firefox/defaults/pref/firefox.js && \    
    echo 'pref("browser.rights.3.shown", true);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("datareporting.healthreport.service.firstRun", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("datareporting.healthreport.service.enabled", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("datareporting.healthreport.uploadEnabled", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("datareporting.policy.dataSubmissionEnabled", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("browser.reader.detectedFirstArticle", true);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("pdfjs.firstRun", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("toolkit.telemetry.reportingpolicy.firstRun", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("browser.newtabpage.enabled", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("browser.newtabpage.enhanced", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("browser.newtabpage.introShown", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("signon.rememberSignons", false);' >> /opt/firefox/defaults/pref/firefox.js && \
    echo 'pref("startup.homepage_welcome_url.additional", "");' >> /opt/firefox/defaults/pref/firefox-branding.js

#RUN (echo 'exit-idle-time = 0'; echo 'flat-volumes = yes') > /etc/pulse/daemon.conf
#RUN echo 'autospawn = yes' > /etc/pulse/client.conf
#RUN echo IyBSZXBsYWNlIHRoZSAqZW50aXJlKiBjb250ZW50IG9mIHRoaXMgZmlsZSB3aXRoIHRoZXNlIGZldyBsaW5lcyBhbmQKIyByZWFkIHRoZSBjb21tZW50cwoKLmZhaWwKICAgICMgU2V0IHRzY2hlZD0wIGhlcmUgaWYgeW91IGV4cGVyaWVuY2UgZ2xpdGNoeSBwbGF5YmFjay4gVGhpcyB3aWxsCiAgICAjIHJldmVydCBiYWNrIHRvIGludGVycnVwdC1iYXNlZCBzY2hlZHVsaW5nIGFuZCBzaG91bGQgZml4IGl0LgogICAgIwogICAgIyBSZXBsYWNlIHRoZSBkZXZpY2U9IHBhcnQgaWYgeW91IHdhbnQgcHVsc2UgdG8gdXNlIGEgc3BlY2lmaWMgZGV2aWNlCiAgICAjIHN1Y2ggYXMgImRtaXgiIGFuZCAiZHNub29wIiBzbyBpdCBkb2Vzbid0IGxvY2sgYW4gaHc6IGRldmljZS4KICAgIAogICAgIyBJTlBVVC9SRUNPUkQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1hbHNhLXNvdXJjZSBkZXZpY2U9ImRlZmF1bHQiIHRzY2hlZD0xCiAgICAKICAgICMgT1VUUFVUL1BMQVlCQUNLCiAgICBsb2FkLW1vZHVsZSBtb2R1bGUtYWxzYS1zaW5rIGRldmljZT0iZGVmYXVsdCIgdHNjaGVkPTEgCiAgICAKICAgICMgQWNjZXB0IGNsaWVudHMgLS0gdmVyeSBpbXBvcnRhbnQKICAgIGxvYWQtbW9kdWxlIG1vZHVsZS1uYXRpdmUtcHJvdG9jb2wtdW5peAoKLm5vZmFpbAouaWZleGlzdHMgbW9kdWxlLXgxMS1wdWJsaXNoLnNvCiAgICAjIFB1Ymxpc2ggdG8gWDExIHNvIHRoZSBjbGllbnRzIGtub3cgaG93IHRvIGNvbm5lY3QgdG8gUHVsc2UuIFdpbGwKICAgICMgY2xlYXIgaXRzZWxmIG9uIHVubG9hZC4KICAgIGxvYWQtbW9kdWxlIG1vZHVsZS14MTEtcHVibGlzaAouZW5kaWYK | base64 -d > /etc/pulse/default.pa


USER user
ENV HOME /home/user
#CMD pulseaudio & exec firefox -no-remote about:blank
CMD exec firefox -no-remote about:blank
