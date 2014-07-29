*This is EME Opinion Draft proposed by Sergey Konstantinov*.
<a href="http://www.w3.org/TR/2014/WD-encrypted-media-20140218/">Draft under discussion</a>

**Technical Problems**

There are two major problems in current EME design.<br/>
1. Security and privacy issues.<br/>
2. Architectural issues.<br/>

In the current scheme, the CDM uses the browser (user agent) as a channel for transferring information to a third-party server and getting responses back. The scheme doesn't set any limit on what information could be sent and received; furthermore, the encryption algorithms used are intended to be kept secret.

This scheme exposes two kinds of potential risks.<br/>
1.1. Risk of spoiling private information via license requests. If the license request contains any kind of information unknown to user agent (including CDM versions and hardware support information, which could be used for fingerprinting), it could be extracted if there was either a vulnerability in the encryption algorithm or if the License Server master key was compromised.  Such problems occurred previously with browser extensions (for example, <a href="http://techdetails.agwego.com/2008/02/11/37/">user MAC address sniffing via Java applet</a>).<br/>
1.2. Risk of exploiting vulnerabilities in response parsing code. Since license response formats are to be kept in secret, CDMs will have proprietary response decrypting algorithms which could be compromised, thus creating a vector of attack via malformed license response. This kind of vulnerability (malformed proprietary data format) is a known method of attack against web users.<br/>

Coincidentally, the security problems are strictly related to architectural ones:<br/>
2.1. Using the CDM for user authentication and license checking is wrong from architectural point of view. Issuing license requests and checking responses is clearly not a responsibility of a <em>Content Decryption Module</em>. Obviously, the webapp must be capable of identifying the user and checking user permissions on its own behalf; why the <em>decryption</em> module needs to check the <em>license</em> is difficult to understand. Correct separation of concerns should limit CDM's responsibility to (a) establishing a secure channel to third-party server (CDN), (b) decrypt frames. So, in fact, there is no need in either license request or response, both could be easily replaced with handshaking.<br/>
2.2. The EME proposal specifies some objects and interfaces (notably MediaKeys) which are already defined in <a href="http://www.w3.org/TR/WebCryptoAPI/">WebCrypto API</a>, and exposes its own APIs instead of relying on underlying low-level system capabilities. Conversely, using standard system key storage and encryption algorithms could significantly decrease risks of exploitation of possible CDM vulnerabilities.<br/>

**Sandboxing**

In our opinion, using such a scheme without additional security measures is extremely risky. All EME implementations which use current scheme with proprietary license request and response formats `MUST` put CDM in sandbox, thus (a) restricting its access to any sensitive information, (b) limiting possibilities to exploit CDM vulnerabilities.

**Interoperability**

Apart from pure technical issues, TAG keeps in mind several questions which have no clear answers at this point:

1. I am an indie studio; what should I do in order to have my content protected by popular DRM systems?
2. I am an indie browser maker; what should I do in order to make my browser capable of using popular CDMs?
3. I am an indie DRM vendor; what should I do in order to make browsers support my CDM?

In our opinion, these questions need developing formal procedures since current EME scheme could eventually lead to segmentation and fragmentation of the Internet for pure marketing reasons.

**Alternative**

In our opinion, we should possibly present to market appropriate alternative to proprietary formats by creating an open and standardized license and encryped media data exchange formats. Such standard should:

1. Eliminate possible security and privacy issues by replacing license request and response with exchanging of standardized public keys and certificates.<br/>
2. Replace custom data structures (MediaKeys, KeySystem) with corresponding WebCrypto entities.<br/>
3. Rely on system key storage instead of using CDM itself.<br/>

A proper scheme of CDM, user agent, and third-party servers interaction should be as follows.<br/>

1. The CDM provides its cryptographic key material into system key storage upon installation.<br/>
2. When the user requests some protected content, the webapp (a) authenticates the user and checks permissions; (b) prepares session information (user id, content id, session id, etc); (c) encrypts or signs session information with the CDM keying material; (d) derives public session key from the CDM keying material; (e) sends encrypted/signed session data and session key to the CDN.<br/>
3. In response to such a request, the CDN generates a second public session key and (optionally) a public certificate and sends them back to the webapp; the webapp then attaches the key and certificate to the media element; the user agent passes the key and certificate to the CDM.<br/>
4. The user agent streams encrypted frames from CDN to CDM to be decrypted using passed session key and redirected to video output.<br/>

Any additional functionality, if needed, should be implemented as a part of corresponding low-level APIs (WebCrypto, Streams, etc.)
