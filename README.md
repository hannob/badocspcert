# badocspcert
Check for certs affected by July 2020 OCSP intermediate incident

# background

A number of certificate authorities have issued intermediate certificates that can be
abused to sign OCSP responses. The technical details are explained here:

* https://www.mail-archive.com/dev-security-policy@lists.mozilla.org/msg13493.html

Additionally there was an incident with Digicert intermediates, which will also
cause revocations. While this was an unrelated incident, I added a check for these
hashes as well. For background see:

* https://knowledge.digicert.com/alerts/DigiCert-ICA-Replacement

This script checks if web hosts send an intermediate certificate that is among the
affected certificates. If your web page is affected this means you should ask your
CA to replace the certificate.

The script is self-contained and contains a list of hashes of the affected certificates.
(Using a dirty trick to make this simple, the hashes are added as a comment and the
script greps itself for them.)

# limitations

This script checks all certificates sent by an HTTPS host. It does not consider a few
edge cases. Web hosts can be configured to not send intermediates, in most cases this
will still work because browsers cache intermediates and some implement so-called
AIA fetching. Such hosts wouldn't be detected.

Also theoretically there could be complex certificate chains that might contain one of
the affected certificates, but that can still be validated through another certificate
path.

# who

This script was written by Hanno Böck, https://hboeck.de/
