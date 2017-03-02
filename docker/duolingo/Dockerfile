FROM debian
RUN sed -i 's/main$/main contrib non-free/' /etc/apt/sources.list && apt-get update && apt-get install -y iceweasel flashplugin-nonfree ca-certificates && (find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true)

# Setting up user environment
RUN find / -type f -perm /6000 -exec chmod -s {} + 2>/dev/null || true
RUN groupadd -g 1000 user && useradd -g user -G audio -m -u 1000 user
#RUN echo 'pref("", "");' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js
RUN \
    sed -i '/browser.rights.3.shown/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("browser.rights.3.shown", true);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/datareporting.healthreport.service.firstRun/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("datareporting.healthreport.service.firstRun", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/datareporting.healthreport.service.enabled/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("datareporting.healthreport.service.enabled", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/datareporting.healthreport.uploadEnabled/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("datareporting.healthreport.uploadEnabled", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/datareporting.policy.dataSubmissionEnabled/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("datareporting.policy.dataSubmissionEnabled", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/browser.reader.detectedFirstArticle/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("browser.reader.detectedFirstArticle", true);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/pdfjs.firstRun/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("pdfjs.firstRun", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/toolkit.telemetry.reportingpolicy.firstRun/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("toolkit.telemetry.reportingpolicy.firstRun", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/browser.newtabpage.enabled/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("browser.newtabpage.enabled", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/browser.newtabpage.enhanced/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("browser.newtabpage.enhanced", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/browser.newtabpage.introShown/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    echo 'pref("browser.newtabpage.introShown", false);' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox.js && \
    sed -i '/startup.homepage_welcome_url.additional/d' /usr/share/firefox-esr/browser/defaults/preferences/firefox-branding.js && \
    echo 'pref("startup.homepage_welcome_url.additional", "");' >> /usr/share/firefox-esr/browser/defaults/preferences/firefox-branding.js

USER user
ENV HOME /home/user

# It's go time
CMD /usr/bin/firefox -no-remote https://duolingo.com
