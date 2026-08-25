# INTENDED EXPERIENCE FOR USERS


#### GOLDEN RULE

The golden rule is that a Noubin MUST ultimately link to content that any Noubin Player can play. 
For now that means audio content in the supported file formats that is DRM free (see the standard for more detail).

Even if there are steps in between (like buying/downloading media/uploading it to a player) a Noubin should always link to either a) directly downloadable files or b) at least one paid storefront with content playable on a Noubin Player. 
#### PLAYBACK EXPERIENCE

A) When a user taps a Noubin on a Noubin Player
- IF the user owns the release and has the files downloaded in their local library they should be loaded for playback. 
	- No web request should be made. The playback should be an entirely local operation.
- IF the user does not own the release and the Noubin Player is web capable then the player should
	- First check with the user that they are happy for the player to access the internet
	- If the user agrees open the Noubin URL and show the user info about the release and where to download/purchase it. 
	- If direct download is available download the media and add to the library 

B) When a user taps a Noubin on a general computing device (phone/computer) without a Software Noubin Player 
- The Noubin URL should be opened by the web browser which presents the release metadata and links as a normal web page 

*Note that Software Noubin Players may only function correctly when they have focus and can intercept NFC tap data. OS deep linking restrictions may prevent them intercepting Noubin URLs when they don't have focus, in which case the fallback behaviour B will occur. (See ADDENDUM1 of `02 standard.md`)*


#### LOADING MEDIA EXPERIENCE

There are four levels to the media loading experience that will become available as the Noubin ecosystem matures. 

**Level 1** 
Fully DIY, end users make their own Noubins from NFC tags.

- Artists sell their music through existing DRM free stores, e.g. Bandcamp, Amazon MP3 (or make files freely downloadable for free releases)
- Users purchase the music, download it and manually add the files to Noubin Players, e.g. via drag and drop into a web portal.
- Users make their own Noubins, e.g.  by pressing a link button and then tap an NFC tag to link the tag with this media. 

**Level 2** 
Artists start making and selling Noubins. Manual linking of files to Noubins still required.

- As before Artists sell their music through DRM free stores for purchase. 
- Artists sell official Noubins to accompany their releases. 
	- Artists have to be clear when they sell the Noubin if they are packaging it with a digital album purchase code OR if it's just the Noubin and the user should purchase the music separately
	- Some users will prefer to obtain music files separately because they prefer their own choice of store  (e.g. because they have all music on Apple) OR if they are a big fan and are buying the Noubin at the show they probably already own the digital album
- Users still have to download the files, add the files to the Noubin Player and associate the Noubin with the files.
	- There is now the opportunity for the artist to provide rich metadata, images, links to other materials at the Noubin URL encoded in the Noubin 

*EXCEPTION: Freely downloadable releases can already be automatically downloaded by players at level 2. So then all the user has to do is tap the Noubin and a web capable Noubin Player downloads the files and links them to the Noubin. (Tap and you hear it first time)*

**Level 3**
Stores support automatic downloading of files directly by web capable Noubin Players. Manual purchase still required.

- As before Artists sell their music through online stores for purchase and sell official Noubins to accompany the release.
- Users must purchase both the Noubin and the music files. 
- The user does a one-time authorisation on their player for stores that support automatic downloading
- The user taps the Noubin on a player.
- The Noubin Player automatically downloads the files and links them to the Noubin

**Level 4** 
Stores support download codes built into Noubin NFC tags. 

- Artists sell Noubins that contain the download code 
	- There are security features e.g. a scratch off pin so a user can be sure the download code hasn't already been redeemed. 
- The end user taps the Noubin on their web capable Noubin Player and completes the security challenge
- The Noubin Player automatically downloads the files and links them to the Noubin. 

Authors notes:
> It would be great to jump to the highest level because it's the lowest friction for the end-user, but it requires stores to come onboard with a common API. 
> 
> On the other hand it's a huge positive that everything is DIY-able. 
> Users don't have to wait for Artists to release Noubins, they can DIY make Noubins to use with digital media they already own. (level 1). 
> Equally Artists don't have to wait for major industry music storefronts to support the standard,  they can already sell Noubins that link to existing stores their music is already in (level 2). 
> 
> On a personal note even if the project never gets to level 3 and 4 I'd still be very happy with the outcome.

The standard as written at the moment only defines technical behaviour up to level 2. When a major storefront actually wants to come onboard then they would ideally collaborate with us in developing the technical standard for level 3 and 4.

#### LIBRARY PORTABILITY EXPERIENCE

A Noubin media library should be in a standard format that is portable between all Noubin Players (both hardware players and apps). So if they upgrade or change players in future their existing library and Noubins just work. 

It's anticipated that some Hardware Noubin Players will explore form factors and UI that are great for playing music, but don't lend well to setting up and managing a media library. e.g. without screens. A benefit of a standard library format users can easily do the media management on a desktop computer or another player with more suitable user interface. 

The media library system should extend familiar file system concepts where possible for at least basic functionality. E.g. if a user simply drops media files into the library using the file picker then players can manually select and play it. 

#### DIY EXPERIENCE

Both users and artists can
- buy cheap and common rewritable NFC tags as stickers 
- use those to make their own Noubins from suitable objects

Rewritable tags are encouraged. This gives the end user the most choice to define the behaviour of their Noubin. For example if they don't have a Noubin Player and really prefer using a major streaming platform they can replace the Noubin URL with the major streaming platform link. 

Note that even non-rewritable tags can be associated with any media in a library. So if a user bought Artist As Noubin they can associate it with Artist Bs music if they feel like it, or a playlist, compilation, podcast, mixtape etc. 

Users can become artists by publishing their own Noubins - simply put the audio file somewhere online where it's freely downloadable, publish a free Noubin URL and write it to an NFC tag. This can be used to e.g. give your friend a mixtape. In future web connected Noubin Players could let you do this entirely from the player. 
#### PRIVATE EXPERIENCE

Playing music locally should never involve internet access or any web server, for a totally private experience without tracking or ads. 

Any functionality that does access the internet should always ask the user first. 

The user does not have to put their Noubin Player on the internet at all. As long as they've got a computer to make the initial music purchases and downloads they can do everything else locally, e.g. through a web interface over LAN or by preparing their media library on removable storage and inserting it in the player. 



