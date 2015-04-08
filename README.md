#Mine Canonical Content Registry

## Summary

In computer science, data that has more than one possible representation can often be reduced to a single unique representation called its canonical form.

Digital media, like images, always exist in different representations on the internet. The same photograph can be represented by many JPGs and PNGs of various compression and resolution. All of these instances have distinct digital fingerprints (md5 or sha256), but all represent a canonical "idea".

 The Canonical Content Registry allows registering a unique global identifier representing a canonical "idea" to function as a hook for persistent metadata for instances of digital media.

### Examples:

Da Vinci's "Mona Lisa" is an original idea with different incarnations as a JPG on the New York Times' website and a PNG on the Louvre's website.

![Different instances of the Mona Lisa resolve to the same Canonical Content ID](http://i.imgur.com/SRrFYJU.jpg)

```javascript
{
  id: "de305d54-75b4-431b-adb2-eb6b9e546013",
  author: "Leonardo Da Vinci",
  name: "Mona Lisa",
  type: "image",
  instances: [{
    origin: "http://www.louvre.fr/mona-lisa",
    url: "http://cdn.louvre.fr/mona-lisa-600x400.png",
    sha256: "79aef731091472c4395b63b32b2c00c919b9d9538dc1c990381cc8c4609fe9f8"
  },{
    origin: "http://www.nytimes.com/2006/09/27/arts/design/27mona.html",
    url: "http://graphics8.nytimes.com/images/2006/09/27/arts/27mona_CA1.190.jpg",
    sha256: "fe604b037ec42e6b9273e141b941986bf13fe29fc9d8f8ab3c0ea8ad6989c303"
  }]
}
```

Michael Jackson's "Beat It" is an original idea with different incarnations as a YouTube video, Spotify link, or a torrent.

![Different instances of the Beat It resolve to the same Canonical Content ID](http://i.imgur.com/NTENEB8.jpg)

```javascript
{
  id: "a3cfcc58-19b2-4c0c-b474-6cc7046f4192",
  author: "Michael Jackson",
  name: "Beat It",
  type: "music",
  instances: [{
    url: "http://www.youtube.com/watch?v=Ym0hZG-zNOk"
  },{
    url: "https://open.spotify.com/track/3BovdzfaX4jb5KFQwoPfAw",
  }]
}
```

The Canonical Content Registry is media-type agnostic.

##Problem

The discrete nature of digital media allows content to be represented in infinite forms suitible for delivery to the end user. Media is constantly changing encoding format, compression, and resolution as it propogates various channels of the internet. Compression algorithms are primarily concerned with perceptual integrity of media relevant to human vision so preservation of embedded metadata such as Exif data is optional. *There is no reliable way to persist metadata for digital media as it is organically shared online.*

As a consequence, creators and consumers rely on centralized media platforms to provide explicit metadata. For example, a photograph on the front page of the New York Times website has an explicit text attribution and annotation below it and a post on Twitter has an explicit username next to it. Other explicit metadata can be context: an image published on a Tumblr blog exists within the context of other images on that blog.

As users of social platforms use native sharing mechanisms like drag and drop, copy and paste, and screenshot to share media, explicit metadata like attribution and context is detached from the media and is difficult or impossible to recover. Creators cannot derive value from the subsequent virality of their works because it isn't possible to discover their identity. Consumers are disenfranchised as well because they cannot have rich interactions with the media. Their actions of annotation or curation (pinning, collecting, commenting, etc) are only visible on centralized platforms and are not persistent or discoverable across platforms.

##Proposal

In order to make metadata for media persistent, it is necessary to have a single global identifier that represents the "idea" of an original work. Any instance of the idea (e.g. a 600x400 72dpi JPG or a 30 Megapixel RAW file) should resolve to the global identifier representing the "idea".

The human visual apparatus can easily recognize near duplicate images and intuitively and confidently infer that very similar incarnations (e.g. the same photograph on a billboard, in a magazine, and on a postcard) actually represent the same "idea". The visual apparatus abstracts the physical differences of each so that the three instances can be considered to be of one photograph ("idea").

![Human Vision](http://i.imgur.com/xKJmgTB.jpg)

To emulate the human visual apparatus, we can use feature detection techniques in computer vision to aid in resolving a global identifier from an instance.

![Computer Vision](http://i.imgur.com/vc0sd5Y.jpg)

By building a global content registry on top of the Bitcoin blockchain (for timestamped proof of data integrity) using a DHT (for metadata volume scalability), we can specify a protocol to globally register canonical representations of digital media. Using computer vision techniques, the system can resolve various instances of an image to its canonical representation. Just like reverse image search or Shazam, computer vision will enable real time queries for image registrations timestamped with the blockchain, returning a global identifier to which various metadata can be appended to.

Because the global identifier can be derived anywhere an image exists, metadata becomes globally retrievable, therefore persistent and open. Anyone can read and write metadata.

Application layers relevant to various metadata can be built on top of the content registry. Some obvious applications are attribution and the ability to retrieve digital media.

![Hypermedia Application Stack](http://i.imgur.com/cCFwxvK.jpg)

Diagram based on Joel Monegro's [Blockchain Application Stack](http://joel.mn/post/103546215249/the-blockchain-application-stack)

#### Existing global media identifiers
While examples of global media identifiers exist, they have been exclusively used in proprietary databases with no protocol specification for open consumption and collaboration. Or, the standards don't provide a way to link many instances of digital media to a single identifier (a limitation of Magnet links).

- [International Standard Recording Code](http://en.wikipedia.org/wiki/International_Standard_Recording_Code)
- [Magnet URI scheme](http://en.wikipedia.org/wiki/Magnet_URI_scheme)

##Registration

In order to register a canonical unit of content, one generates a unique identifier for the content and assigns a content instance to it:

```javascript
{
  id: "de305d54-75b4-431b-adb2-eb6b9e546013",
  type: "image",
  instances: [{
    url: "http://i.imgur.com/R6chj3Y.jpg",
    sha256: "79aef731091472c4395b63b32b2c00c919b9d9538dc1c990381cc8c4609fe9f8"
  }]
}
```

##Lookup

Like the [ONS-resolver](https://github.com/openname/resolver), a Canonical Content Resolver will aid in reverse image search for canonical records in the Canonical Content Registry. By using computer vision techniques in near-dupe detection, the canonical content registry can be queried with a near-duplicate image and provide highly confident results of content registered with the system. A user is encouraged to interact with the canonical identifier and append metadata to it, such as pointers to more instances, attribution, etc.

##Attribution

One important layer on top of the Canonical Content Registry is attribution. Lack of attribution for media, especially images, is an enormous problem for creators on current media platforms. Unattributed content accounts for a majority of the content shared on platforms such as Pinterest and Tumblr, and there is no way for consumers to discover the creator or origin of the image. By making the Canonical Content Identifier discoverable by virtue of seeing an image, attribution will be one type of metadata that is persistent wherever the image exists. Discovering the attribution can be initiated by an action from the user, and can also be integrated with major image-sharing platforms. This will be highly beneficial to creators because they will be able to drive the value of their content propogating the network back to an identity they own and control ([Openname](https://openname.org/)), not to the centralized platform.

###Nametags

Because it is impossible to tell the age of a digital file, a decentralized attribution registry faces the problem of accounting for a trove of legacy content. Any user can potentially claim ownership of any digital file. We propose specifying a *Canonical Creator Identifier*, called a nametag. A nametag has no owner or maintainer, it is simply a global identifier used to associate canonical content identifiers with the idea of a creator. For example, the `*pablopicasso` nametag can be used by the community to tag all Pablo Picasso paintings in existence. Similarly, photographs by Annie Leibovitz can be tagged `*annieleibovitz`.

Unlike Pablo Picasso, Annie Leibovitz is a living artist that could potentially join the decentralized content community by creating an Openname identity. It would be useful if a nametag could be decorated by an actual user profile that can be interacted with by the community.

####Decorating a nametag

A nametag can be decorated with an Openname profile with the aid of a simple reputation system. The community will sign statements saying the user possesing the `+annieleibovitz` Openname profile is the same creator represented by the `*annieleibovitz` nametag (statements are part of the Openname [specification](https://github.com/openname/specifications/blob/master/profiles/profiles-v03.md)). After sufficient reputation is present, the nametag can route to the profile on the application level. The consumer user experience will alias the nametag to the profile, making the experience feel like the discovery of an image leads to the discovery of the creator's user account, where they can interact directly.

###Decentralized attribution

The Canonical Content Identifier becomes a hook for attaching nametags, and nametags become a hook for attaching a decentralized identity. Reputation is a necessary element because there is no centralized entity to vouch for correctness of attribution, and it is up to the community to sign statements to "vote up" a nametag for an image, and an identity for a nametag (see [Question 1](#q1)).

##Blockstore

[Blockstore](https://github.com/openname/blockstore) is a key-value store on top of Bitcoin that uses a DHT (distributed hash table) for data storage while storing only hashes of the data in the blockchain. The ability to store unlimited amounts of data in a DHT makes it an appropriate decentralized datastore for metadata related to images. Hashing of data in the blockchain guarantees that the history of metadata has not been tampered with.

###Blockstore integration

Based on discussion with @shea256 and @muneeb-ali, we have outlined the following Blockstore integration requirements:

1. Store unique CCID (canonical content identifier) in Blockstore
2. Append file instance metadata to CCID (see example above)
3. Tag a CCID with a nametag
4. Sign a statement stating a nametag belongs to an Openname profile
5. Append arbitrary metadata to CCID (hashtags, related CCIDs, etc)
6. Ability to easily interpret the above by a new node in the DHT

####Questions

1. <a name="q1"></a>How to avoid sybil attacks on writing metadata to DHT, tagging with nametag, and signing statements without high transaction costs?

