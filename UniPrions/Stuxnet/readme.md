
This is a quick note to keep track of some work-in-process data relating to the forged 'Flame' certificates used as part of the Stuxnet attacks. I've bumped into Didier's analysis of those certs (http://blog.didierstevens.com/2012/06/06/flame-authenticode-dumps-kb2718704/) several times, and each time felt there was a piece we're all missing, right under our noses.

I think we've begun to understand what that piece is, now. As this is a partial post, not a full exposition, I'll go thin on the backstory. And jump right to the interesting bits.

Looking at the certs in question, there's obvious formatting issues in some of the fields - in terms of how they come out of a conventional de-pem'ing tool. That's always seemed particularly salient to me, but statistical work on those outputs got me nowhere. Now that I'm less ignorant of the underlying non-transitive parsing and encoding that can (and is) done in certs, I had a better idea where to look.

Based on other research cryptostorm has done for several months, I was curious to see what happens when we are careful to retain full data as we decode from the exact certs themselves. And indeed, we see that there's some surprises in the CRL fields, just for starters.


When we reverse this back this strings from hex/octet format:

3081A130819EA0819BA08198864A687474703A2F2F63726C2E6D6963726F736F66742E636F6D2F706B692F63726C2F70726F64756374732F4D6963456E664C696352656741757443415F323030392D31322D31302E63726C864A687474703A2F2F7777772E6D6963726F736F66742E636F6D2F706B692F63726C2F70726F64756374732F4D6963456E664C696352656741757443415F323030392D31322D31302E63726C


We get this:

0ÂÂ¡0ÂÂž ÂÂ› ÂÂ˜Â†Jhttp://crl.microsoft.com/pki/crl/products/MicEnfLicRegAutCA_2009-12-10.crlÂ†Jhttp://www.microsoft.com/pki/crl/products/MicEnfLicRegAutCA_2009-12-10.crl


Actually, that's not really what we get - much of the good stuff is (wisely) input-sanitized out by github before being uploaded (I suspect, anyhow). So, let's do a line-by-line analysis of the actual UTF-8 data coming out of that de-hexing. Thanks to Rishida's UniView (http://rishida.net/uniview/#title) an invaluable tool for such funky-spelunking, and leaving out standard characters (latin letters, numerical digits, punctuation), what we end up with is quite interesting, indeed:

  â€Ž0081  [control]
  â€Ž00A1  INVERTED EXCLAMATION MARK
  â€Ž0081  [control]
  â€Ž009E  [control]
  â€Ž0081  [control]
  â€Ž009B  [control]
  â€Ž0081  [control]
  â€Ž0098  [control]
  â€Ž0086  [control]
  â€Ž002E  FULL STOP
  â€Ž002E  FULL STOP
  â€Ž0086  [control]

That's five quite rare control characters hidden in that one URL-ish blob of hex... not counting the inverted exclamation point, not sure what to make of that tbh :-)

I'd also point out that CRLs like this are fed right into the browser's guts, and from there to NSS (need to confirm the exact object flow here, of course)... pulled silently, and parsed behind-the scenes per MIME parameters on the local machine. When Flame was afoot, even Chrome still used CRLs... nowadays they are something of an abandonware on global scale, but even so countless tens of millions of CRL lookups happen silently, behind the scenes, on a daily basis even now.

What happens when control characters like that hit browsers, and NSS? I don't know... but we've seen some hints in the data gathered thus far that it's not good, whatever it is. That we find these characters in this level of APT-class attack code is... telling, perhaps. Or maybe simply suggestive, for now.

Whatever the case, having found this trove of interesting data in the fake Flame certs, I'll be quite surprised if we don't find that this vector is neither new, nor rare, nor of minor destructive power.

More to come, as time allows.

Cheers, 

~ pj


{I've not echoed over the raw cert data; those keen to see the details are encouraged to avail themselves of Didier's copies, for now - we'll replicate to our fishycerts repository once all the write-ups are in process, formally}
