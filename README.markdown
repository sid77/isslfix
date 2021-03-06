iOS < 4.3.5 fix for SSL vulnerability (CVE-2011-0228)
=====================================================

Deb packages
============

https://github.com/jan0/issfix/downloads

daa5c6efae5b36690153e715712e265e  isslfix_1.1.deb
1.1 package on cydia (same as fix1)

51560b2e1cc888f708c8c84c62be75a5  isslfix-fix1.deb
test package for repeated leaf false positives fix

eee21f50d677a1edd6b8700f045e60f7  isslfix_1.0.deb
actual 1.0 cydia package in cydia

df580a7179b24ca1dfdd637cbcdf8062  isslfix.deb
version 1.0 submitted to cydia

f22887c41bc9c663f6a181c2e8e9fd03  isslfix-test3.deb
test version, without the comodo blacklist

Testing
=======

**Warning : backup your device before installing in case something goes wrong …**

```
dpkg -i isslfix.deb
launchctl unload /System/Library/LaunchDaemons/com.apple.securityd.plist
launchctl load /System/Library/LaunchDaemons/com.apple.securityd.plist
```

Visit https://issl.recurity.com to check if this is working.

If you already visited this page without the fix applied, reload the page or clear Safari's cache.

You should see the "Cannot Verify Server Identity" popup, and this message in syslog :

```
<Warning>: iSSLFix: Certificate <1BDC0A9E-7FC6-4BA4-A9E5-41F206B82D81> in chain starting at <issl.recurity.com> has isCA=0 => possible MITM attempt, making validation fail
```

Because securityd is restarted, existing processes and daemons will lose their "connexion" to it and most calls to the Security framework (Keychain, cert validation, etc) will fail : iTunes wont be able to connect to the device, apps will be unable to access the keychain, etc. These issues should disappear after a device reboot.

**If securityd crashed (check /Library/Logs/CrashReporter/), remove the package (dpkg -r isslfix) before rebooting.**

Comodo stolen certificates blacklist
====================================

For devices with older firmwares, the blacklist added in iOS 4.3.2 is replicated (see blacklist.c).


References
==========

http://support.apple.com/kb/HT4824

http://blog.recurity-labs.com/archives/2011/07/26/cve-2011-0228_ios_certificate_chain_validation_issue_in_handling_of_x_509_certificates/

http://blog.spiderlabs.com/2011/07/twsl2011-007-ios-ssl-implementation-does-not-validate-certificate-chain.html

http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-0228

https://github.com/hubert3/iSniff

http://support.apple.com/kb/HT4606

http://www.comodo.com/Comodo-Fraud-Incident-2011-03-23.html

https://bugzilla.mozilla.org/show_bug.cgi?id=643056

http://hg.mozilla.org/mozilla-central/rev/f6215eef2276

