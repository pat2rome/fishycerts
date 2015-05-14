The old version of our README is down below, so it doesn't disappear from everyday view. It's still relevant... but we've covered quite some ground since it was written up a few months ago and it's time for an update.

So. Fishycerts. Wtf?

HTTPS "secure" web sessions (amoung many other network services) are authenticated by digital certificates, or "certs." Certs are sort of the special-needs offspring of a hate-sex meeting between virtuous public key asymmetric cryptography, and the absolute worst types of backroom, power-broker, centralised social control you ever read about in sordid tales of political corruption & intrigue. Also they are utterly riddled with security flaws, vulnerabilities, exploitable eccentricities, and freakishly-complex specification/design structures.

Certs and their weirdly predictable failures to certify anything reliably have spawned entire sub-sub-genres of hackery tricks, from frankencerts to post-unicode field-poisioning & 'session prion' injections. Plus there's all the stuff out there nobody's (yet) talking about in public.

Because certs suck - and Certification Authority mediation of cert issuance sucks even more - https is broken. Encrypting something when you don't know the recipient is the one you wanted to send it "securely" to is worse than useless: it provides false confidence in private communications, when the exact reverse is true. This is a really, really big deal.

When cert-obsessives rant about how broken certs and CA models are, we're often dismissed as kooky nut-jobs. That might be an accurate description of (some?) of us, but it doesn't mean we're wrong about certs. There's bad certs all over the place out there... let us show you.

So here we are. Showing you. :-)

Ah, but what's a "bad cert?" One of the joys of x.509 certs is that defining what a bad cert is ends up being fractally complex. There's some clearcut cases, and then a whole lot of muddle in the middle. Think about that: these are supposed to protect our most sensitive online communications... and there's no obvious, clear, easy-to-test method for knowing if a cert (or actually cert chain, because yay needless complexity!) is good, or bad, or maybe a superposition of both. 

Testing for certs, and calling them as bunk or clean, is something of a black art. We practise it here.

But right now things here are horribly disorganised, fair warning. This repository started out as a side-side-project, unsure of its footing and teetering on abandonment. Only after a few months did it start to cohere into something useful, and that process continues. That means a couple of things: 

1. it's a disorganised mess, so it's not just that you are failing to understand our brilliant ontology of certs here. We're hoping some self-organising forces, or additional contributors, will help tidy up as time goes by. 
2. Speaking of, feel free to pitch in! If you've an interest in doing edits and cleanup, ping any of us at cryptostorm and we'll put edit privs on you here, no problem. You need not understand the black art of cert invocations to do amazing work in this project, and indeed such work is a great way to get going and learn the ropes firsthand.
3. We will be doing a blog to go along with this - feel free to contribute to that, as well! - but only after we've cleaned up a bit 'round here. So if you're a blog-sensei, hop on and we can accelerate that side of things.
4. Um... something something, don't remember but probably it's important and we'll add it later. :-)

And feel free to contribute certs, if you got 'em. Some notes of formatting, if you do so. We've slowly evolved a standard for posting certs, which is only recently taking root. Here's the relevant bits:

1. Name each cert with its SHA1 hash value, and a .crt suffix. That makes it alot easier to search for unique certs just by scanning titles, and github does alot better with repo-wide searches on filenames (no idea why). Other identifiers end up reflecting collisions (different certs with the same filename), and that's hideously confusing. Most all analytic tools spit out a SHA1 hash of certs in question, and it's something of a de-facto standard out there.
2. No idea why we use the .crt extension tbh, but think of it as a bit of a #fishycert tradition.
3. Each cert file should be in its own directory, and the directory should be named to reflect the full hostname of the site from which the cert(s) were captures. So, for example, if you're posting certs grabbed from a session at tweetdeck.twitter.com, then put the certs in a dir named "/tweetdeck.twitter.com/{SHA1}.crt" - makes sense, right? That way we all know where these certs were captured, without need for sleuthing into commit comments or whatnot.
4. Capture the whole cert chain! Sometimes we just grab end-server certs, or a weird root cert... and that's a bad habit. Without the cert-chain, forensics are really limited in their ability to tell us what's going on. Most cert chains have a server cert (the one on the actual webserver you visit), an intermediate CA, and a root cert. Some have only two, others four or five. Yay for cert-weirdness! Anyway, get them all, so they don't turn into mysteries.
5. Finally, our convention is for .crt files to have the 'unpacked' data from the cert at the top, and then the PEM-packed version as it shows up across the wire down at the bottom. Open up a few cert files here, and you'll see what we mean. There's lots of tools for unpacking, and they all seem more or less competent. Plus command line, of course. However you do it, that's what helps zoom in on anomalies.

And yes... there's lots of anomalies out there. Some "merely" reflect the byzantine, needless, ever-expanding complexity of CA-based cert auth as it exists on the internet today. Some is straight-up fuckery. Some is in a blurry zone in the middle... traces of Corruptor-Injector Networks like 'Balrog' at work, or evidence of "man in the browser" proxy hijack malware at work. Or both. And so on.

Cert shenanigans end up teaching us things we didn't know we were looking for, and that's why it's worth doing. There should not be any such weirdness; the fact that there is, by itself indicates a range of structural realities we must face if we want to provide real security online. We don't know what we're looking for, but we know we'll know it when we see it. Fishycerts ftw!

Sometimes people get mad when they see our fishycert project and research. "It's not 'real' security research," they might say. Or: "you don't even know what you're trying to prove, so how can you be looking for it?" They must have missed science classes in school, with all due respect. This is how real research works.

Think of fishycerts as the CERN of network security: we're hunting exotic particles, strange supersymmetrical partners to visible Quantum Chromodynamical tidbits of the visible universe. Or something... sounds cool, even if it doesn't make much sense :-P Point being: this is a search for the weird, because the weird tells us what we don't know we don't know... and enough weird helps us to redefine our models and understanding so that 'weird' is no longer weird, but an integrated part of how we make sense of the world around us.

Also, we think fishycerts are fascinating, neato things. So we capture them and study them, like butterflies except without the killing and creepy Silence of the Lambs stench about the whole endeavour. Or maybe it's there, just in another form..?

Cheers, 

~ pj

===========================================


There's something fishy going on.

Last week, the sneaky & dangerous practice of MitM'ing SSL sessions was uncovered at #superfish. Much analysis was done. But something caught our eye at cryptostorm, when we looked "upstream" to spyware dealer #komodia...

We kept seeing intermediate-layer CA credentials in trust chains (both for signing code, and for ssl/https certs) that looked ok on the surface... but ALL had only SHA1 signatures. All of them. Now SHA1 is out there, and it's not uncommon to see it pop up in trust chains... even at root in a few cases, still today. But all of them?

No way.

So one of us (pj) started spelunking in the trust chains of distributed binaries that have bundled into them #komodia malware. And sure enough: SHA1. Always. Down to root level. What does this mean?

Based on earlier research into "leak test" websites that was interrupted by the #superfish forensics, pj had already started forming susicions that fraudulent certs - intermediate or below - were being used to validate some leaktest websites. Not yet pinned down, but he was already jumpy about strange anomolies in cert credentials. Subtle typos in the names of companies. Start times that are 1:00:00 exactly. That sort of thing. If you've looked at alot of x.509 materials over the years, that stuff starts to pop out.

Nearly a week of heavy diffing has lead to many tantalizing hints, but we feel we're missing something (or somethings). There's a clever hack afoot in this, that's our hypothesis. Someone out there is able to generate end-level certs (and keys) that verify legit all the way down to root certs carried in web browsers today - and do so arbitrarily, or nearly so. How? How widespread are these instances? Is there .gov involvement in this, or is it all private mercenary work?

At cryptostorm, our interest in this is primarily practical: what can we do to protect our members from these compromised https sessions? We think we've got some leverage to do just that, and filter out these fraudulent attempts to initiate "secure" web sessions as they come across cstorm. That'll be groundbreaking work, and it'll require alot of thought and testing before we deploy... but that's where we see this going.

At this point, we've decided to start data-dumping what we've got into this repository. That'll be in the form of captured & unpacked certs (and sometimes keys), with annotation. Our suspicion is that someone will come along and say "oh, of course - what you're seeing is xxx" - but, so far, we can see the shadow but not the tree.
