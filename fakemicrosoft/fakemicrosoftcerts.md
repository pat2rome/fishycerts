http://blog.didierstevens.com/2012/06/04/flame-before-and-after-kb2718704/


https://www.f-secure.com/documents/996508/1030745/w64_regin_stage_1.pdf


"The authors of this sample have also digitally signed the malware, with the intent to give it additional credibility and to
make it look as if it was a Microsoft file. Keeping in mind the what was said about sample’s exports in section 3.1, it is even
more likely that with the digital signature they were trying to camouflage the sample as a legitimate Microsoft Windows
system file.

The Authenticode signature is, in itself, valid, but the certificate properties (shown in Figure 2), highlight the inability of
the system to find and validate the issuer of this certificate. The certificate’s validity period is from 15/7/2011 to 14/10/2012;
the fact that the compilation timestamp from the file’s PE header is inside this range makes us believe that the binary was
indeed built on November 25th, 2011. Moreover, combining the alleged compilation time and the certificate validity range,
we can speculate that this binary was probably updated regularly.

The issuer of the certificate is an alleged Microsoft Root Authority. This name resembles a valid Microsoft issuer, but if we
focus on the KeyID we find that such entry does not match any of the known Microsoft Root Authority IDs. Details about
the supposed issuer of the certificate are presented below.

KeyID=41 68 26 6a 16 60 0f 36 41 19 af 06 f9 54 4d 06
Certificate Issuer:
CN=Microsoft Root Authority
OU=Microsoft Corporation
OU=Copyright (c) 1997 Microsoft Corp
Certificate SerialNumber=0c ea ea 19 bb bd 4f 86 4e b7 e9 47 97 cf 74 a8

It is also interesting to notice that, while the malware claims to be signed by Microsoft, the certification path does not
show the same structure of proper Microsoft-signed binaries; specifically, there is a lack of an intermediary before
Microsoft’s Root Certificate Authority (CA). The certification path for the malware versus the one from a valid Microsoftsigned
binary is shown in Figure 3. It is likely that the authors of this threat have used standard signing tools to create such
a certificate hierarchy; they then deployed the signed binary in combination with the root certificate."


There's a bunch of these out there - their serial numbers and dates vary, but all have identical OS data:


"OU=Copyright (c) 1997 Microsoft Corp., OU=Microsoft Corporation, CN=Microsoft Root Authority"
