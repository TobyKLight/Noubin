<img width="300" alt="Noubin Logo" src="https://github.com/user-attachments/assets/1da4e295-836d-4135-ba8d-d094ad3b3998" />

# NOUBIN 
Local-first physical media hyperlink standard, 
aimed at artists who make music and audio content.
Based on NFC technology. 

This repository contains the documents defining the Noubin open standard. 
The open source reference software implementation (Plippa) will hopefully be online soon. 

----
### Status
This is v0.3.0 of the standard. It should be considered highly volatile and there can potentially be significant breaking changes until v1.0 stable is reached after some real-world testing. 

**If you'd like to implement the standard or contribute**: That'd be great! When you're ready please start a discussions thread to introduce yourself and what you're doing. We can share results and ideas and I can give you a heads up to discuss potentially breaking changes.

---
## GOALS

**For end users:**
- Enable users to play digital media they own locally and privately.
	- Optionally without looking at a phone or computer screen via a hardware player
- Playback is triggered via physical media objects which can be creative artworks in their own right. 
- If users don't yet own the digital media they are given a choice to open a web link where they can purchase, stream or otherwise listen to it. 
	- The user can choose the platform that best suits them from the release platforms the artist has made the digital media available on
- Making Noubins is entirely DIY-able, end users can make their own Noubins and link them to existing digital media they already own. 

**For artists:**
- Enable new revenue stream for artists to make and sell a wide range of physical objects as Noubins
	- Low barrier to entry, suitable NFC tags can be purchased as stickers for less than 50c and written with a typical smartphone.
- Nudge end-users towards buying and owning digital media, which is generally more economically favourable to artists compared to streaming.
	- However note the standard doesn't block streaming or other forms of digital media sharing. Artists can list all platforms they choose to publish their digital media on, just one has to be a DRM-free store.
- Noubins have value even if the user doesn't have a Noubin playing app or hardware player. 
	- They can be tapped on regular smartphones where they open the Noubin URL in a browser. 
	- This also presents the user with the list of platforms where to buy / stream / listen to the music
	- So for a user that prefers to stream they tap the Noubin on their phone, tap their chosen platforms streaming link, that will typically open their streaming app and they hear the music.  
	- Plus Noubin URLS can contain links to any other content the artist wishes to place there, e.g. music video, website, socials, other exclusive content linked to this release.  
- It's envisioned that Noubins can be packaged with and without 'download codes' for the associated digital media
	- Handy for users that do or don't already own the digital media
- In future this could be streamlined so that the Noubin also contains the download code, for 'tap, redeem and play' functionality. If there is interest and support from storefronts we can make this part of the standard. 
  - We could also go in the other direction which is the possibility of Noubins that actually contain the media files. But this would be much more technically demanding than using 50c NFC tags. 


---
## CONTENTS OF THE STANDARD
you are currently reading `readme.md`

#### CORE STANDARD

`01 user experience.md` start here, the intended experience for end users of Noubins

`02 standard.md` the technical standard defining shared minimum behaviour of Noubins, Noubin URLs and Noubin Players

`03 metadata-base.md` the core metadata format 

`QandA.md` further information about why the standard is designed this way

#### LICENCES AND BRANDING

`LICENSE.md` The standard itself is licenced under the permissive open source Apache licence. 

`Use of the name and logo.md` The Noubin name and logo have more strict (but still fairly permissive) licences. Basically as long as you implement the standard properly you can use the name and logo for free. 



---
###  How do I read the standard? What's an .md file?

`.md` files are markdown files.  If you're not familiar with markdown it's a documentation format that produces human readable text files with some small annotations that make them display better in a markdown reader. 

To read the standard, assuming you're on the repo home page, simply click on the files in the top pane above this readme.

If you open these files and see characters like ### you are looking at the raw markdown file. (Hashes are the symbol indicating headings). Look for a 'preview' button to instead see properly displayed headings. 

---

### Inspiration and thanks

My main inspiration was [this project](https://fulghum.io/album-cards) by Jordan Fulgham which hit the exact painpoint I'm already having listening to music with my son. 

I basically saw what Jordan had done and thought two things
- Someone should make a player and a kit so regular people can listen to their music collections like this (this is the Plippa project, more info soon)
- Someone should make an open standard so artists can make their own official NFC physical media for their digital music AND so people making their own players can trade NFC media. 

And so here we are! Big thank you to Jordan who opened my eyes on this whole thing. 

There's been many similar projects over the years I want to acknowledge. Here's what I found while researching this and I'm sure there's more out there.  
- https://hackaday.com/2012/03/14/rfid-jukebox-for-the-kids/
- https://community.home-assistant.io/t/nfc-digital-vinyl-player/381809
- https://f-droid.org/de/packages/com.trapplab.nfc_radio/
- https://notes.iopush.net/blog/2020/12-nfcaudio-nfc-controlled-mp3-player/
- https://gist.github.com/wkjagt/814b3f62ea03c7b1a765
- https://github.com/zacharycohn/jukebox
- https://github.com/tonuino/TonUINO-TNG
- https://pypi.org/project/gukebox/
