pypolicyd-whois
===

This is a small and simple policy server for postfix which checks the sending domain's authoritative nameserver against a local blacklist and furthermore tries to retrieve whois information for that sending domain.

If a whois lookup can be made for that domain, its organization and even the registrar can be blacklisted, if these are accessible through whois.  

A few notes:
===
* This is in no way compatible to any RFC!
* I simply got sick of spammers registering new domains every few days and blast my mailboxes with DKIM-verified, SPF-allowed high quality spam

How to implement:
===
Place policyd-whois to /usr/libexec/postfix/policyd-whois and `chmod +x` the file.
Create the directory python-policyd-whois, place the policyd-whois.json there.
Put this in your /etc/postfix/master.cf:  
~~~
policy-whois  unix  -       n       n       -       -       spawn
    user=nobody argv=/usr/libexec/postfix/policyd-whois
~~~

Add this to your `smtpd_recipient_restrictions` in /etc/postfix/main.cf:
~~~
check_policy_service unix:private/policy-whois
~~~
