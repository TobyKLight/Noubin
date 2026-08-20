
### Standard Design Decisions
#### What's the origin of the name?

Noubin is a portmanteau of 
- Noumenon, which is a philosophical concept that roughly means "thing-in-itself"
- Nubbin, in the sense of a kind of nonsense word for a generic object like macguffin, whatchamacallit, thingamajig.

Considering physical media for music until now: 
There was the record, the cassette, the CD and then what's next...? 

We propose the thing itself. The noubin.
Put your music on anything. 

See also [ding an sich](https://en.wikipedia.org/wiki/Thing-in-itself).

---
#### Why do Noubins contain just URLs and not actual media files?

Due to current NFC technology. 

NFC tags don't require batteries or power because the scanning device supplies a very tiny amount of power to them wirelessly. This allows NFC tags to be portable and battery free, but means they have a very slow data transfer rate. 

In turn this means they aren't practically designed to contain much more data than a URL. 

You could theoretically design a Noubin that has a USB stick in it or find another way to give a user files directly. If there's interest we could look into standardising this as well. To get started for now though Noubin works with existing audio media distribution systems. 

---
#### Does a Noubin bought new from an artist come with the music files? Or come with a 'download code' to get the files from a storefront? 

For paid content: Unless the artist/retailer explicitly says otherwise (e.g. "Noubin + Download Code") selling a Noubin is just selling a Noubin.

For freely downloadable content: yes if the Noubin URL is set up properly with the links to the content. 

#### I bought a Noubin that was meant to come with a download code but I didn't get it / it didn't work 

You have to contact the point of sale from where you bought it. 

---
#### What happens if I sell a Noubin or buy one second hand? Does it come with the music files?

So probably no, unless the listing explicitly says it comes with the files (which it may do for freely downloadable content). Unfortunately the reality today is most music storefronts don't allow transfer of purchases to another account. 

Unless explicitly made clear otherwise, selling a Noubin is just selling the Noubin, not the media files. 

#### Why should Noubins be linked to releases (albums, singles etc) and not just a general link to an artists website/social media etc? 

This is an intentional design decision because we want Noubins to always be something that the user can 'play', analogous to a record. 

It also makes the Noubin stay conceptually close to the music and the artists creative agency rather than just being a toy related to the artist. 

What's the difference? So typically when you see an album cover it gives the impression "these are visuals curated by the artist to go with their music". But if you see a toy of the artist for sale it can give more of an impression like a toy company just bought a licence to make action figures. The artist may have given approval but it's a step further away from the artists core creative output. 

We want Noubins to feel like the former, objects the artist picked to be a curated part of their media release. 

---
#### Can Noubins be linked to playlists? 

For end-users YES. Users can link a Noubin to any playable item in their library, including tracks, albums and playlists. End-users can also reassign any Noubin they own to these playable items. 

For official noubins from Artists: NO, they shouldn't sell Noubins linked to a playlist. Artists should always sell Noubins linked to a release. This is because it's a bad experience for an end user if an artist releases a Noubin linked to a playlist and that playlist includes tracks from releases the end user doesn't have. 

Note however a release be a greatest hits album ,it can be a mixtape, it can be a compilation. So there are ways for Artists to release a Noubin that plays a chosen collection of songs as long as the digital release itself contains those songs.  

---
#### Do Noubins have to be linked to new releases? 

No, an artist can sell an official Noubin for digital media that was released earlier. E.g. releasing the official Noubin now for a 2014 album without re-releasing the album. 

The artist can also re-release the album to go with the Noubin. As long as the Noubin is available separately it doesn't disadvantage the user, the user who has the old digital release can buy just the Noubin and link it to the old files.  

---
#### Why local first? What about streaming? What about DRM? 

The standard requires that (for paid content) at least one link at a Noubin URL is to a distribution platform where the media can be 
- Bought to be owned 
- Free of DRM 

The standard nudges users towards digital media ownership and local playback because
- a) It's more private for users, with a better experience and playback quality that doesn't rely on an internet connection
- b) Buying music is usually more economically favourable for artists compared to streaming platforms 
- c) It's the only practical way to ensure interoperability, that a Noubin links to content a Noubin Player can play. 
	- The reality today is many links to media on closed platforms can only be played by their own app or partner ecosystem hardware. e.g. audiobooks bought on XYZ play only on XYZ app or official XYZ partner hardware. 
	- Most such platforms don't have an open program that allows any manufacturer or developer to play their content. 
	- If we allowed Noubins that link only to closed platforms it would be a nightmare of "Oh sorry that only plays if you have a ABC brand Noubin player that has the XYZ platform  integration"

As long as there is at least one link at the Noubin URL where the user can buy and download the primary audio media in a supported DRM-free format it's following the standard. 

Note it's "at least one", so 
- links to streaming services, radio, media rental or other distribution platforms can also be provided at a Noubin URL, this is entirely up to the artist and which platforms they want to work with. 
- Alternate forms of the digital media, e.g. a video clip on a video streaming platform, can also be listed at the Noubin URL.

Fortunately in the music industry DRM is uncommon on storefronts at the moment. Unfortunately this is not always the case in other sectors e.g. audiobooks.

OK so this means for audiobook authors with major publisher contracts they are simply not going to be able to make Noubins if their publisher refuses to release non-DRM audiobooks? Doesn't this hurt adoption and hurt artists who are trying to make a living from their work? 

So for this reason we actually aren't morally against DRM. DRM only Noubins would potentially be tolerable as a necessary evil if it weren't for the closed ecosystem factors:
- DRM is spruiked as a tool to prevent piracy (which it doesn't, it only takes one hacker to upload one trivially cracked copy to a torrent site to enable piracy). The real bulwark against privacy is low friction purchasing and playback.  
- What DRM actually does is enforces ecosystem control. XYZ audiobooks only play on XYZ players and those of XYZs high profile commercial partners. 
- Which means general Noubin Player manufacturers have no practical avenue to play XYZ media.
- So the users choice of what store to use, what price to pay is no longer a matter of an open and fair marketplace but dictated by what closed ecosystem equipment they already own.
- Why bother making an open standard to enable monopoly behaviour? These monopolies also harm artists as platforms increase their bargaining power over a closed market.
- Let such platforms make their own XYZ NFC tags in that case. 
And so they can. Major closed platforms already have functions to publish playback URLs and could easily put them on NFC tags and distribute them for exclusive playback on their services. 

If such NFC objects don't contain a link to the artists choice of distribution channels, with at least one link where the media can be bought and owned it's not a true Noubin in our eyes (so please don't call it one and abuse the standard). 

However also note links are intercepted by Noubin players to check for local files before they query the internet. SO for an end user: if a cool NFC tag merch item is exclusively available only with a link to a major streaming platform, but the user does own the digital media files in their local library, they can DIY associate that NFC tag with their local media and create a true Noubin experience. 

Alternatively if the tag is rewritable than an end-user who taps the tag but only to always select the major streaming platform link (that then opens their streaming app), and who never will have a Noubin player, can just rewrite the streaming app link straight into the Noubin NFC tag so it's one action to open their streaming app instead of two. 

---
#### What about file format licencing issues? Why support proprietary audio formats?

Fortunately today in the digital audio media world we are blessed with many good quality open audio formats, and these are widely available at storefronts including: 
- WAV
- MP3
- FLAC

There are however some more closed popular formats that would require licensing for hardware Noubin Players
- AAC
- OPUS (*controversial)
(Disclaimer: this isn't legal advice, manufacturers please check yourself)*

BUT on the other hand potential issues with closed formats are practically mitigated because these formats can be transcoded by the user to open formats with free tools. 
- E.g. Apple Music sells AAC files but allows you to convert them to MP3 directly in iTunes. 

On the artist side distribution platforms occasionally have exclusives (usually temporarily) and we aren't against them incentivizing artists (e.g. paying them) to prioritise a platform by putting it at the top of a list. If an artist can get money and major platform support this way then we say take it. The storefronts already have to meet the other requirements (DRM free music to own), so we won't quibble about the file format IF users can convert it easily.

Given the state of things here is where we ended up:

Noubin Players must
- At least support all three of the major open audio file formats (WAV, MP3, FLAC)
- Optionally support closed audio file formats (AAC, OPUS)

A Noubin URL should
- Have at least one link to a storefront where a user can buy the media to own, DRM free 
- At that storefront the files are either 
	- a) in one of the three open formats or 
	- b) in a closed format that can be trivially converted to an open format with free tools. 

It's a compromise but we think practically it should produce relatively minor friction for users while still enabling artists maximum choice of stores to work with.  

If this compromise is not working and users regularly report bad experiences with unplayable content we are open to changing this approach in a future version of the standard.

---
#### What about videos/films?

The initial Noubin standard is developed with music and audio content in mind. Theoretically in future Noubins could be linked to any kind of digital media. 

Video and film content presents some problems though that the current Noubin standard doesn't yet try to solve: 

- Noubin is meant to be local-first, playing DRM-free files you own. 
	- Video storefronts today are generally streaming based and heavily DRM restricted.    
	- Even stores where you 'buy' a video you are actually just buying the right to stream it repeatedly 
	- Download buttons don't give you a file but instead just an offline playback function in the storefronts own locked ecosystem player app.
- In audio media the codec is strongly wedded to the file type. A .mp3 file is MP3 codec, an .aac file is AAC codec etc.
	- In video this is not the case, an .mp4 file can contain a whole range of codecs, some of which are protected by relatively thorny licences and patents that it will be tricky for open source players to support.  
	- It's complex even for people who make video to be sure they are using a truly open format. 
- There is also the problem of naming and the capabilities of hardware players themselves, naturally players without screens or video output can't play video.
	- They may not even have the processing power to decode video files to play only the audio part. 

What this adds up to at the moment is pain for end-users. We really want if an artist sells a Noubin that a user with a Noubin player definitely will have a way to play it. 

Given this complexity we we will leave these problems alone for now. Other home media player projects exist that are more video oriented.

If there is demand for video in Noubin we can look at bringing it to future versions of the standard (or through extensions) so that video is always a great experience for end users. 

To be clear 
- there is nothing stopping artists adding video links to Noubin URL pages (and also links to images, interactive experiences etc) but that should be in addition to audio media. 
- there is nothing stopping players from supporting video playback already but currently it's not standardised. 

There is one final workaound though if you really want to make a Noubin for non-audio media. 
So for now a Noubin should be distributed with an audio release.
But note a Noubin can still have further links to other media as long as the main links are to audio content.
So the Noubin of the Film can actually be the Noubin of the Soundtrack of the Film. Or the Noubin of the Art Exhibition can be the Noubin of the Interview with the Artist of the Art Exhibition, etc. 



---
#### I'm an artist concerned about censorship. How can I ensure the online experience of Noubins I make remains available?

If you are concerned about censorship than you can always self-host your Noubin URLs on your own website. 

So the truth is any platform you rely on, including a Dedicated Noubin Linkhost but also any regular web host, domain registrar etc is some kind of legal entity that can theoretically be pressured to remove content in various ways. They also may have their own rules and values about allowed content. 

The best protection against being deplatformed is to have your media available on a range of platforms. The Noubin link then becomes a source of truth that you curate and update to point to where your media is available. 

Hopefully soon there will be open source software available that lets you export the `.noudata` files and  matching HTML page for your releases, which will make it easier to self-host them. 

---
### Collaboration and process
#### When will the standard reach v1.0 stable?

So you may have noticed my note on the readme.md to consider the standard volatile and that there may be breaking changes until v1.0. 

The reason is that it's all well and good for me to say how I think a system should work, but until the software is built, the system is running and the users are having a great time I believe it's premature to lock the system design down. 

When those things occur and seems like the system will meet the design goals then it can be called v1.0 stable. 

If anyone would like to implement this standard already though I certainly don't want breaking changes to ruin your day, so lets use the github discussions feature to communicate. 

---
### About the authors and commercial 
#### Who is behind the standard? 

Hi I'm Toby K from Now We Make. Currently I'm the author and maintainer of the standard.  
My background is I'm an interactive artist from Australia, now living in Berlin, Germany. And my company Now We Make is an indie software/hardware house based in Berlin.

---
#### How do you make money? 

From the side of Now We Make, here's how we make money from this project: 
- For end users we want to make boutique Noubin players (called Plippas) with open source software 
- For artists we want to help them make creative Noubins and with hosting the link content (at Noubin.com)
- We aim to have businesses sponsor the standard and our projects related to it

This open standard means we can't build a monopoly on making players, making Noubins or hosting links. Others are welcome and encouraged to do these things and make money from doing them. 

---
#### Is this anything to do with NFTs, blockchain or crypto? 

No. The Noubin standard does not use blockchain or NFTs (non fungible tokens). It has nothing to do with crypto currency. 

Note the standard does not prevent individual artists linking to blockchain related things at their Noubin URLs if they wish to. 

---
#### Are there plans to close the standard or put more restrictive commercial terms in place? 

We plan to keep the standard open. That's our values but it's also the only sane commercial thing to do. If you look at the history of physical media formats even giant companies have a very hard time getting closed standards adopted. 

If it made sense in future we would consider fees for larger businesses in the ecosystem, but given it's an open standard built on other open technology that's only going to work if it's relatively modest fees that bring those companies a lot of value. E.g. Asking companies that want to make officially certified Noubin Players to join a standards association similar to bluetooth. 

---
#### Why should we trust you? 

Lots of good stuff has been created before than enshittified by companies as they got bigger. It's easy for us to say we don't plan to turn this into a closed standard but why should users and artists trust us? 

The answer to that the standard is designed so you don't have to trust us. We hope to earn your trust of course, but lets consider a worst case future where we sell out to the most evil corporate interests in the land and they do indeed create a new closed version of the standard: 


- Noubins set up properly for local playback of local media don't require or connect to the internet, meaning there is no mechanism for a future enshittifier to mess with them. 
- Any previous version of the standard would still be available under its original open licence. An enshittifier can only try and control the branding. 
- Our Noubin URL hosting (if you use it) could indeed be enshittified as it's an ongoing service, but with artists able to host their links elsewhere (including their own website) and distribute Noubins with rewriteable tags this would hopefully have a limited impact. 
