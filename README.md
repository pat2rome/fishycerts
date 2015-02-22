There's something fishy going on.

Last week, the sneaky & dangerous practice of MitM'ing SSL sessions was uncovered at #superfish. Much analysis was done. But something caught our eye at cryptostorm, when we looked "upstream" to spyware dealer #komodia...

We kept seeing intermediate-layer CA credentials in trust chains (both for signing code, and for ssl/https certs) that looked ok on the surface... but ALL had only SHA1 signatures. All of them. Now SHA1 is out there, and it's not uncommon to see it pop up in trust chains... even at root in a few cases, still today. But all of them?

No way.

So one of us (pj) started spelunking in the trust chains of distributed binaries that have bundled into them #komodia malware. And sure enough: SHA1. Always. Down to root level. What does this mean?

Based on earlier research into "leak test" websites that was interrupted by the #superfish forensics, pj had already started forming susicions that fraudulent certs - intermediate or below - were being used to validate some leaktest websites. Not yet pinned down, but he was already jumpy about strange anomolies in cert credentials. Subtle typos in the names of companies. Start times that are 1:00:00 exactly. That sort of thing. If you've looked at alot of x.509 materials over the years, that stuff starts to pop out.

Nearly a week of heavy diffing has lead to many tantalizing hints, but we feel we're missing something (or somethings). There's a clever hack afoot in this, that's our hypothesis. Someone out there is able to generate end-level certs (and keys) that verify legit all the way down to root certs carried in web browsers today - and do so arbitrarily, or nearly so. How? How widespread are these instances? Is there .gov involvement in this, or is it all private mercenary work?

At cryptostorm, our interest in this is primarily practical: what can we do to protect our members from these compromised https sessions? We think we've got some leverage to do just that, and filter out these fraudulent attempts to initiate "secure" web sessions as they come across cstorm. That'll be groundbreaking work, and it'll require alot of thought and testing before we deploy... but that's where we see this going.

At this point, we've decided to start data-dumping what we've got into this repository. That'll be in the form of captured & unpacked certs (and sometimes keys), with annotation. Our suspicion is that someone will come along and say "oh, of course - what you're seeing is xxx" - but, so far, we can see the shadow but not the tree.
