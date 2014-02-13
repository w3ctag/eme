# Proprietary Extensions to the Web

**This is an unreviewed draft by Jeni Tennison, with contributions from Sergey Konstantinov and Peter Linss, and does not represent official TAG or W3C opinion.**

The goal of the web is to enable people to freely and easily share information with each other. This is the source of its beneficial impact on society. It is important that the architecture of the web and the technologies that we use on the web do not undermine the social goal of the web.

This goal of supporting free and easy sharing of information does not imply that all information on the web must be available to all people all the time. The web can also be used to share information with restricted groups of people. For example, to protect our privacy, it should be possible to share information that no one can see aside from the intended recipients. And because of this need for privacy, the web needs to also be secure and trustworthy.

The value of the web arises from the power of the network of links between information that the web holds. The web is much less valuable for us all if it is partitioned, and much more valuable the more links there are between content. Restricting access to content devalues links to that content, because not everyone can follow the links to see the content.

Over the web's 25 years there have been several technologies and architectures which have had the effect of restricting access for some people to portions of the web. This document explores how these work and the effect they had on the web, with the ultimate goal of aiming to inform the debate about the inclusion of Encrypted Media Extensions (EME) in HTML.

## Examples of Restricting Access to Web Content

In this section we look at existing technologies that "break the web" by restricting the content that can be accessed by different users.

### Authentication

Some content on the web is protected through authentication mechanisms such as HTTP authentication, OpenID or OAuth, which mean that only logged in, authenticated users can access some web content.

In some cases the web content protected through these mechanisms is accessible to those who pay for access (ie pay walls). In other cases, users may be granted access for other reasons (eg because they work for a particular company or within a particular consortium).

For users, one important characteristic of authentication mechanisms on the web is that there is not usually any relationship between the platform that is used to access the content (browser, operating system or hardware) and the ability to access the content. As long as the user has paid to access the content, they can do so whatever the device that they have.

### The `<object>` tag

The `<object>` tag is used in HTML to indicate an area of the page whose content is determined by an external plugin. Popular plugin technologies are Flash and Silverlight. For a while, Java applets were also popular.

Viewing content that is supplied through a plugin requires the user to install the plugin technology on their device. This can restrict access to the content if:

  * there is no implementation of the plugin technology on the platform the user is using (eg there is no implementation of Silverlight in Chrome on MacOS)
  * the platform the user is using does not permit the installation of additional software, for example locked-down, managed business laptops
  * the user does not want to install a plugin, for example because of security or ethical concerns

Content that is accessed through a plugin is also rarely accessible to other technologies in use on the web. It cannot be linked to from other pages (reducing the number of links that could be made, and thus reducing the value of the web). It can be hard to index by search engines.

The eventual impact of this on the web and on users of the web can be illustrated by looking at the history of Flash. Flash is a plugin for providing graphically impressive content within web pages. For a while, there was a "virtuous" cycle that supported the use of Flash:

  * most platforms supported Flash and Flash provided a better user experience than plain HTML
  * so developers could build sites using Flash without excluding many people
  * so there were lots of sites that used Flash
  * so new platforms had to support Flash

This cycle gradually shifted as the functionality of HTML caught up with the functionality of Flash, making it feasible for developers to build engaging websites that didn't use Flash. However, the killer blow was when a single extremely popular platform, namely iOS, didn't support Flash. This shifted the "virtuous" cycle to:

  * major platforms don't support Flash
  * so developers can't build sites using Flash without excluding many people
  * so sites use technologies other than Flash
  * so new platforms don't have to support Flash

Flash is still deployed sufficiently widely that is the basis for a few "hacks" to provide non-essential functionality that is missing from HTML, for example manipulating the content of the clipboard. There are also sites that have not been updated to use technologies other than Flash and therefore cannot be accessed by users whose platforms do not support it.

## Principles for Web Technologies

This section explores the principles that should apply to new web technologies to ensure that they satisfy the social goals of the web.

### Platform Independence

One basic principle for web technologies that enables the web to grow and be valuable and prevents it from fragmenting is **platform independence**:

> If a user is authorised to access a piece of content, they must be able to do so on a platform [browser, operating system, hardware] of their choice.

When this principle is broken, the web is easily partitioned based on market share, particularly when the same organisations control both the platform (browser, operating system and/or hardware) and popular content. We see closed feedback cycles of the type seen with Flash. Users who wish to access particular content must use particular devices, so more of those devices are bought, so more content is published targetted exclusively at that device.

It's important to recognise that the introduction of a technology that breaks this principle does not break the entire web, only partitions off the part of the web that uses the technology. However, if the content in that part of the web is highly desirable, that can have a global impact on the technologies that are used on the rest of the web, which can then influence the remaining content.

When we consider content for the web we need to take a long-term perspective. Particular platforms (browsers, operating systems and hardware) are short lived compared to the lifespan of content on the web. During the lifespan of the web, we have seen many browsers come and go, and completely new operating systems and devices become dominant. Creating content that can only be used on a particular platform limits the lifetime of that content.

Content with a limited lifetime is contrary to the best interests of consumers of that content. They may find that they have to keep old systems running so that they can continue to access the content. For example, there are some intranet systems that are 'Best run in Internet Explorer 6' which have prevented whole government departments from moving to new releases of operating systems or other browsers. Or users may find that they have to re-purchase content that they have already bought in order to view it on a new device (see for example the progression from VHS to DVD to Blueray).

The flourishing of the web &mdash; both in content and in the variety of platforms that can access it &mdash; is due to the fact that most content on the web is published in a platform-independent, standards-based way. Platform independence ensures variety, innovation and competition.

### Accessibility

*TODO: talk about web accessibility*

### Security

*TODO: talk about security*

### Layering

The Technical Architecture Group recommends an architecture for the web platform in which each layer is built on a layer of existing primitives.

*TODO: talk more about layering*

## Applying the Principles to EME

The Encrypted Media Extensions defined in [the Encrypted Media Extensions Working Draft](http://www.w3.org/TR/encrypted-media/) aim to provide a common, platform independent interface to enable sharing of encrypted content on the web. The common interface encapsulates a "black box" of encryption technologies and very little has been specified about the way in which these technologies operate.

### Platform Independence

We have seen, in the use of the `<object>` tag, that standard interfaces over platform-dependent technologies do not turn the platform-dependent technologies into platform-independent ones. So when considering encrypted media on the web, we have to ensure that the "black box" of encryption technologies is platform independent, as well as the interface that surrounds it.

Platform-dependent encryption technologies use private/secret/proprietary algorithms to encrypt and decrypt content. Because these technologies are private/secret/proprietary, a user's ability to use them will depend on their use of a particular platform. A content publisher may come to a deal with the owners of a particular platform to provide that content exclusively through that platform. They would then use the proprietary algorithm to encrypt the content, which could then only be decrypted on the proprietary platform.

Platform-independent encryption technologies use a combination of public, standard algorithms to perform the de/encryption of content, and private/secret keys that are only known by those who are authorised to access the content.

A mechanism for ensuring that encrypted media could be used on the web without undermining the principle of platform independence would be to define a standard set of public, standard, encryption algorithms that can be used, coupled with a standard mechanism for accessing a key to decrypt encrypted media.

**The EME specification must ensure platform independence, for example by specifying the use of open standard encryption algorithms.** 

### Accessibility

DRM systems usually prohibit any manipulations with content including displaying third-party subtitles, or (in case of e-books) reading a book using system voice-over engine, thus making a content less accessible by disabled people. This practice is incompatible with an effort to make Web more accessible. 

**The EME specification must address accessibility by specifying how content made available through EME can be manipulated.**

### Security

Copyright law (notably, [DMCA](http://www.law.cornell.edu/uscode/text/17/1201)) prohibits any attempts of searching for vulnerabilities in DRM systems. There were [notable](http://en.wikipedia.org/wiki/Sklyarov) [cases](http://en.wikipedia.org/wiki/Edward_felten#The_SDMI_challenge ) when IT security specialists and academic researchers were arrested or threatened with legal actions under DMCA.

This practice is incompatible with ensuring a secure web for all, particularly if the code to support DRM is embedded within browser code, rendering that also un-inspectable.

**The EME specification must address security issues arising from the inability to inspect code.**

### Layering

In the case of EME, according to the layering principle, it should be possible to specify a set of primitive APIs that would enable a DRM system to be built in Javascript. Many of the pieces that would be needed for this are already in progress, such as:

  * Streams API
  * Crypto API (with access to hardware crypto and tokens)
  * WebAudio API
  * Canvas (with tainting) for video output
  
There may be other APIs that would be necessary, such as a secure JS worker that would only have access to a narrowly limited set of APIs, and would run in a special context that could not be inspected by the user, to prevent the exposure of crypto keys. 

This approach has the following advantages: 

1. The DRM execution is under the control of the browser to prevent privacy and other security violations. 
2. The DRM code is delivered to the browser as Javascript, and is therefore platform independent. 
3. The DRM code would live outside the browser code base, which means that browsers continue to be inspectable. 
4. The exposed primitives and features are general purpose and have other uses beyond DRM (eg encrypting WebRTC streams) and make the entire platform richer. 
5. By using only the existing platform mechanisms for audio and video output the normal accessibility features could still work (e.g. filtering the video to increase contrast). 
6. DRM can be deployed client-side by anyone by including a small JS library, rather than building a binary plugin.

**EME be built as a layered system that works in combination with other platform APIs.**



