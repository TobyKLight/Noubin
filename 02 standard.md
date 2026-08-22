
# NOUBIN TECHNICAL STANDARD

The key words MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, and OPTIONAL in this document are to be interpreted as described in RFC 2119.

## 1 DEFINITIONS

##### 1.1 End User
- Someone who picks up a Noubin and taps it on either a Noubin Player or a phone/computer. 

##### 1.2 Artist
- Anyone who makes a public release of digital media linked to a Noubin
- Can also be the artists legitimate representative, company they contract etc. 

When are End Users also Artists? 
- End Users who make Noubins for private use only linked to local content in their own library are not 'artists' for the purpose of this standard.
	- So if it's for private use do whatever you want with your Noubin and it's still a Noubin in our eyes. 
- End Users who publish their own content to give to others (e.g. mixtapes, a voice note) and link it to a Noubin are Artists for the purposes of this standard.
	- In this case they should make sure their Noubin behaves consistently with a proper Noubin URL and nice release data for anyone who handles it. 

##### 1.3 Release
- A release is digital media published by an artist that they wish to link with a Noubin
- The release can be freely downloadable or available for purchase
	- Plus additionally available on streaming platforms or other forms of release platform 
- This standard is developed with music in mind although other forms of audio content (e.g. audiobooks, mixtapes etc) can also be played by Noubin Players
	- In addition to audio content any other digital or web content can be linked to the release e.g. a music video, images, interactive experience, website etc.
- In the music sector context a release is typically an album, single, compilation etc
- Artists can bend what a 'release' is to be any form of digital media (e.g. films, video games, illustrations etc) as long as the following conditions are met:
	- There's some kind of meaningful audio content as part of the release that a Noubin Player could play, e.g. a soundtrack. 
	- The audio content is available for free or purchase (not just streaming on closed platforms that most players won't support)
	- You've set the buyers expectations correctly about what you are publishing and what the Noubin will link to. 

##### 1.4 Release platform
- A service where an end user can preview, stream or buy and download a release from an artist (And sometimes freely download)
- e.g. Bandcamp, Spotify, Apple Music, Audible
- Not a strict industry term but used in this standard to differentiate from distribution services (e.g Distrokid), as well as include non music based platforms (e.g. Audible for audiobooks)

##### 1.5 Distribution service 
- A service where an artist can upload their music to have it appear on release platforms
- e.g. Distrokid, TuneCore, CD Baby 

##### 1.6 Noubin
- A physical object 
- The object MUST contain an NFC forum type 2 or type 4 tag.
- NFC tags produced by artists MUST be encoded with a URL that follows section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS
	- The NFC tag MAY be encoded by the end user with plain text, this can only be used to link to media in their local library. 
- The objects physical design and materials MUST NOT prevent the NFC tag from functioning
	- e.g. NFC sticker tags typically do not function when stuck directly to metal 
- The NFC tag SHOULD be rewritable, this gives end-users maximum DIY flexibility. 
- See ADDENDUM2 for notes about use cases for non-rewritable tags and type 4 tags. 

##### 1.7 Official / Original / Genuine Noubin
- A Noubin sold by an artist can be called an "Official Noubin" of the artist.
	- Or also "Original Noubin", "Genuine Noubin"
- As in it's the Noubin the artist officially made to accompany their digital media release.
- These terms imply endorsement of the artist. 
- Differentiating it from a Noubin made by an end-user where they could link any NFC tag to their existing digital media collection. 
- Note the Noubin and URL still have to conform to the Noubin standard in the first place to use the word "Noubin"
- See `Use of the name and logo.md` for full details

##### 1.8 Certified / Authenticated / Verified Noubin
- Do not use "Certified", "Authenticated", "Verified" or any wording in connection with describing the Noubin that sounds like the maintainers of the Noubin standard have checked or endorsed the Noubin, Noubin URL or linked digital content. 
- To avoid confusion with "Authenticated" also don't use the word "Authentic"
- Reserved for future use in case there is interest in some kind of certification or authentication program, which we could then include via a technical mechanism in the standard.
- You can get other certifications for your media of course, just be clear in marketing materials that it's not the Noubin that's certified but the audio mastering etc. 
- See `Use of the name and logo.md` for full details

##### 1.9 Noubin Player
- A term encompassing both Hardware Noubin Players and Software Noubin Players
- All Noubin Players SHALL have at least the functionality laid out in section 3 NOUBIN PLAYER SPECIFICATIONS

##### 1.10 Hardware Noubin Player
- A standalone device that is not a general purpose computing device (e.g. a phone/laptop) 
- The device MUST have an NFC reader capable of reading NFC forum type 2 and type 4 tags
- The device MAY be capable of connecting to the internet (see section 1.12 Web Capable Noubin Player)

##### 1.11 Software Noubin Player 
- A software application ("app") running on a general purpose computing device (e.g. a phone/laptop) 
- The host device MUST have an NFC reader capable of reading NFC forum type 2 and 4 tags 

##### 1.12 Web Capable Noubin Player
- A noubin player (hardware or software) capable of connecting to the internet
- This is optional functionality
- When a user taps a Noubin on a web capable Noubin player and the local files are NOT in the users library, the Noubin Player fetches data from the Noubin URL about the linked digital media and where to download/purchase it.
- In the case that the files are freely available on the web the Noubin Player can download them directly. 
- When purchased files are added to the library the Noubin Player can fetch the "canon" metadata from the web and associate it with them. 
- In future a Noubin Player may be able to directly download files that the user owns from stores. This requires cooperation of stores to develop a suitable authentication method. 

##### 1.13 Noubin Media Library
- A folder containing folders, media files and metadata
- It MUST follow section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS
- Although note it's a pretty forgiving specification organisation wise, players will cope with chaotic libraries. 

##### 1.14 Noubin Media Library Management Software
- A dedicated app for managing Noubin media libraries and metadata
- It does not have to also be a Noubin Player (e.g. you double click a media file and it opens in external player app)
- It MUST follow section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS

##### 1.15 Noubin Key
- The string written into a Noubin NFC tag's first NDEF record that directs a Noubin Player to a Playable Item as defined by a `.noudata` file
	- In plain english this is the text you write into an NFC tag that is then matched to an album/song/playlist etc. 
- The raw noubin key read from the NFC tag is normalised according to the procedures in section 6.2 Noubin Key Normalisation, then compared to `noubinKeyNormalised` value of playable items in the media library. 
- The Noubin Key is ideally formatted as either a 
  - Local Noubin Key, as plain text (using NDEF Text record), which SHOULD follow the format laid out in section 1.16 Local Noubin Key.  
  - or a Noubin URL (using NDEF URI record), which SHOULD follow the format laid out in section 1.17 Noubin URL and section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS. 
- However if the Noubin Key is not formatted to either of those definitions, but after normalisation still matches a `noubinKeyNormalised` of a playable item in the media library it MUST trigger that item. 


##### 1.16 Local Noubin Key 
- A Noubin Key only for local playback SHOULD be: 
  - Encoded in an NFC tags as the first NDEF record
  - Use the NDEF text record type
  - a Text string in the format `<playableItemTitle>.noubin.<randomString>`
    - e.g. "Smooth Jazz.noubin.XYZ123"
- The player/media management software generating the string should set it in the NFC record and associated `.noudata` file, ensuring after normalisation both entries match the calculated `noubinKeyNormalised` value.  
- The purpose of the random string part is allowing Noubin Players to generate keys for items that have the same `playableItemTitle`, e.g. if the user accidentally creates two "Smooth Jazz" playlists which are in different parts of the media library.
- The player MAY shorten the string if it exceeds the length that can be stored in the tag, e.g. for 48 byte tags. 
	- In the case of these very small tags recommend using only a random string. 

##### 1.17 Noubin URL
- A full hyperlink accessible on the internet — a complete URL using `https`, 
- Eg `https://domain.com/noubin/artist/album` 
- Intended to be both stored in a Noubin NFC tag (as the Noubin Key when the tag uses a URI record) and hosted at a linkhost
- Noubin URLs SHOULD be well formatted: use `https`, the path section SHOULD end with a trailing slash pointing at the release folder, and follow section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS
- When accessed by a browser the Noubin URL returns a regular web page 
- When accessed by a Noubin Player it returns metadata readable by the player (*at a slightly different fetch URL — see section 6.3 Noubin URL Normalisation*)
- At the Noubin URL there MAY further be a link to directly download media files, useful for an artist to distribute e.g. mixtapes or music they wish to release for free. 
- At the Noubin URL there MAY be links to purchase/stream media
	- If the music is available for purchase one of the digital platforms linked MUST offer DRM free media for the user to purchase indefinitely in the Supported Media Formats (see sections 1.23 and 1.24)
- The Noubin URL MAY contain other links as the artist wishes e.g. to website, merch, social media etc.
- See section 6.3 Noubin URL Normalisation for how players normalize a Noubin URL when fetching `web.noudata` over the internet

##### 1.18 Noubin Linkhost
- A web server that hosts one or more NOUBIN URLs
- It MUST serve them as per section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS 
- This can be the artists own website, publishers website or any site
- To be clear any site serving Noubin URLS as per section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS is a linkhost. 

##### 1.19 Dedicated Noubin Linkhost
- A website whose main function is to host Noubin URLs
- Ideally with a user friendly interface for artists to manage their Noubin URLs. 

##### 1.20 Noudata Metadata file
- Each Noudata file contains metadata and represents a Playable Item (which can be a single track or multiple tracks), and optionally a key string to enable it to be linked to a Noubin
- Noudata metadata files MUST end with the extension `.noudata`
- Noudata files MUST be created, named and organised based on section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS and section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS.  
- Noudata file content MUST be encoded with JSON and follow the Noubin Metadata format, defined in `03 metadata-base.md` and its extensions.
- Noudata files MUST be UTF-8 encoded text (with or without a BOM; players MUST accept both)
- Multiline strings: Where `03 metadata-base.md` marks a string field as multiline (e.g. `description`, `legalNotice`, `additionalCreditsText`, `lyricsTranscript`, cue text), the following apply:
	- On disk the file MUST be valid JSON per RFC 8259. Unescaped control characters (including raw line breaks) MUST NOT appear inside JSON string values. Newlines MUST be written as the JSON escapes `\n` (line feed, U+000A) and, if present, `\r` (carriage return, U+000D).
	- After JSON parsing, the in-memory string MAY contain real line-break code points.
	- Writers SHOULD normalize line endings to LF only (`U+000A`) before writing. Prefer `\n` over `\r\n` or lone `\r` in the stored string value.
	- When reading or importing data that contains CRLF (`U+000D` + `U+000A`) or lone CR (`U+000D`), writers and media management software SHOULD normalize to LF before saving.
	- When displaying multiline text, players SHOULD treat LF, CRLF, and lone CR as line breaks. Players MUST NOT crash on any of these forms.
	- Trailing newlines at the end of a multiline field are OPTIONAL; players MAY trim these. 
	- Players SHOULD display multiline fields correctly, they MAY set a maximum line limit after which data is truncated if it's e.g. necessary for a cohesive layout. 
	- Any editor of Noudata Metadata files SHOULD offer a multiline input field 
- Note the SAME metadata format is both employed on the internet to provide information about media and where to purchase it AND then locally on players the same data is used and extended to provide location of actual downloaded media to play it. 
	- This addresses an issue with current music distribution reality that audio file formats have different embedded metadata standards, plus distribution services and release platforms manipulate and present metadata differently.
	- We propose that artists provide a Noubin URL with canonical information about their release including where to buy it, user buys and downloads it, then local media files are appended to the same metadata from that URL the artist originally generated.
- Note2 why not base this on the .nfo format established by the Kodi media player project?
	- One reason is that XML parsing is not so great for microcontroller based hardware players. 
	- Considering we need to extend the format quite a lot anyway prefer to start own format and avoid that technical debt.
	- We also want to provide more artist-centric metadata, what fields do artists want to see in a modern format to e.g. credit their collaborators properly? link to their soundclouds etc?
	- Building automatic .nfo converters into media library management software is not hard if there is demand from users who want to convert a Kodi library or vice versa. 
	- Note also the Noubin Media Library specifications is designed so that a library can be BOTH accessed by Kodi (and similar apps) and a Noubin player. Apart from generating `.noudata` files the Noubin Player should cope with the Kodi style organisation and file naming. 

##### 1.21 Playable Item
- A track, album, playlist etc that can be loaded for playback by tapping a Noubin on a Player
- Can be a single item (track) or collection of items (album, playlist etc)
- On the technical level every playable item is either
	- A) defined by a Noudata metadata file or
	- B) a single media file selected directly in the Noubin Player UI (when supported)

##### 1.22 Playlist
- A collection of media items that is not itself a release from an artist. 
- Items may still be associated with a release (so whilst an item is currently playing from playlist X it still shows relevant metadata from album Y that it originally came from)	

##### 1.23 Minimum Supported Media formats
- Currently in this version of the standard the minimum supported audio formats are
	- MP3 encoding contained in .mp3 files
	- WAV (PCM) encoding contained in .wav files
	- FLAC encoding contained in .flac files
- All players MUST support playing back media in these formats
- Artists SHOULD list stores that support these formats

##### 1.24 Optional Supported Media formats
- Optionally players may support
	- AAC encoding contained in .m4a, .m4b, .aac files
	- OPUS encoding contained in .opus files
- Players MAY support playing back these formats
- Artists MAY list a store that only supports these formats as the sole DRM-free store on a Noubin URL (see QandA question 'What about file format licencing issues?' for more info)

##### 1.25 Supported image formats 
- Artists MAY provide cover images with a release
- Players MAY support showing cover images
- Players that support cover images MUST support these formats
	- JPG encoding contained in .jpg files
	- WEBP encoding contained in .webp files
	- PNG encoding contained in .png files
		- Small detail here: PNG only strictly has to be supported if the player supports images above 1024x1024. Otherwise it can support 1024x1024 thumbnails which are limited to JPG and webP formats. (see thumbnail formats below)
- Artists MUST NOT link to images higher than this quality in `web.noudata`:
	- 3000x3000 resolution
	- 8MB file size
- Players MAY have smaller display capabilities and limit display support to only smaller thumbnail images
- The thumbnail tiers are 
	- 128x128 JPG 
	- 128x128 WEBP
	- 512x512 JPG
	- 512x512 WEBP
	- 1024x1024 JPG
	- 1024x1024 WEBP
		- Why two formats? WebP is smaller but not universally supported in microcontroller decoding yet, hence jpg still possible
	- The thumbnails must be stored in a `/thumbnails` subfolder and named `<imageName>-<resolution>.<extension>` 
	- e.g. for `cover.png` when all thumbnails are generated the following files should exist
		- `/thumbnails/cover-128.jpg`
		- `/thumbnails/cover-128.webp`
		- `/thumbnails/cover-512.jpg`
		- `/thumbnails/cover-512.webp`
		- `/thumbnails/cover-1024.jpg`
		- `/thumbnails/cover-1024.webp`
- Thumbnails only have to be generated once, stored with the release media files under `/thumbnails` folder
	- To generate thumbnails:
		- Artists MAY generate thumbnails themselves 
		- Linkhosts MAY automatically generate thumbnails from album art during upload 
		- Media management software MAY have a function to generate thumbnails 
		- Players MAY generate missing thumbnails during import or when the release is accessed for playback.
- The canonical thumbnails subfolder name is `/thumbnails` (lowercase `t`). Linkhosts, artists, and media library software creating new content SHOULD use this form.
- Players and media management software SHOULD accept `/Thumbnails` (uppercase `T`) as a fallback when resolving thumbnail paths, for compatibility with older libraries or case-insensitive storage where the folder may have been created differently.
- Artists SHOULD provide images that are square
	- If not than resolutions e.g. 3000x3000 refers to maximum X Y resolution. For a non square image it means the maximum resolution of a single side is 3000. 
- Players SHOULD support non-square images by fitting them into a square frame (not cut off)
- Artists MAY provide multiple alternate cover images with a release (e.g. equivalent to front and back image of a record sleeve)
- Players MAY support showing alternate cover images but they don't have to 
- Moving cover images (e.g. videos) are not yet part of the standard.

##### 1.26 Safe file and folder names
- For the purpose of safely naming files and folders so they can be read across different operating systems the following characters are NOT allowed in file names or paths:
	- `< > : " / \ | ? *`
    - ASCII Control Characters (0-31)
- Files and folders must not end with a space or a period ( . )
- Total length of a path including file and folder must not exceed 260 characters.
- UTF-8 encoding should always be used 
- Note this is only for file and folder naming validity. It doesn't set URL safe characters 
- see `03 metadata-base.md` entry for `playableItemSafeTitle` for more information

##### 1.27 Dynamic Playlist
- Players MAY support this function
- It means that a user can perform a search on a media library and essentially save the search string itself into a Noubin
- So when the user taps the Noubin again it does the search and plays the media
- Also respecting other playback settings like shuffle
- This allows the user to e.g. save a search for an artist to play all that artists songs, or save a search for a genre / category etc to play everything from that genre

## 2 NOUBIN URL AND LINKHOST SPECIFICATIONS 

- Noubin URLs MUST be `https` with domains setup properly with `https` certificates
- Noubin URLs MUST point to a folder representing a release and end with a `/` e.g. `https://domain.com/noubin/artist/release/ 
	- Regular browsers will resolve this to `https://domain.com/noubin/artist/release/index.html`
	- Noubin Players searching for metadata will resolve this to `https://domain.com/noubin/artist/release/web.noudata` 
- Noubin URLS SHOULD use `/noubin/` as the first folder to indicate all sub folder content is Noubin Player readable.
	- EXCEPTION short URLS , see below
	- EXCEPTION Urls that are domains dedicated to noubin linkhosting e.g. `noubin.com` 
- Noubin URLs MAY include the artist name in the domain OR a path element e.g `https://artist.com/noubin/release/` or `https://domain.com/noubin/artist/release/`
	- Note the actual list of artists who worked on a track can be defined in `web.noudata` in far greater detail than can be put in the URL. 
	-  EXCEPTION short URLS , see below
- Noubin URLs MAY include random strings in order to prevent automated crawling and/or hide exclusive content that's only available to Noubin owners. When these random strings are included they SHOULD be at the end of the url so that any url preview first shows human readable info like the domain, artist and release at the start. e.g. `https://artist.com/noubin/release/jfijo373/`
	- Note random string URL obfuscation should not be relied on as a security measure for protecting valuable content (like a download code) as anyone who handles the Noubin can access the link.
	
- Short Noubin URLS are handy in particularly for low capacity NFC chips In which case they can be typical short URL format  e.g. `https://domain.com/jfijo373/`

The Noubin URL contains the following data
- SHOULD contain canonical information about the release, including the title, release date, artist, credits etc.
	- For mixtapes, podcasts etc this can also include e.g. chapter information
- If it's a paid release MUST contain at least one purchase link to a store where a user can buy the music to own without DRM
	- If it's a paid release MAY contain links to other release platforms where the user can listen to the music
- If it's a free release MUST contain a link to where the user can download the media file in a supported format
- MAY contain other links as the artist wishes to provide e.g. to social media, videos etc.

At the Noubin URL folder there MUST be two items containing the data above
- The regular webpage version of the Noubin data with clickable links and buttons. This is what a browser fetches as `index.html` when the user navigates to the Noubin URL. 
- A `web.noudata` file which is a UTF-8 encoded text file containing a valid JSON object as per `03 metadata-base.md`. 
	- The `web.noudata` file should explicitly be in the release folder e.g. `https://domain.com/noubin/artist/release/web.noudata` or `https://artist.com/noubin/release/jfijo373/web.noudata`
	- When served over the web, the recommended MIME type is `application/vnd.noudata+json` or standard `application/json`.
	- The maximum size of a `web.noudata` metadata file MUST NOT exceed 1 megabyte (Ideally much less)
	- `web.noudata` files, album art and direct media download links MUST be accessible by simple devices that are not accessing through a full featured browser and cannot e.g. pass captcha challenges.

In the Noubin URL folder other data MAY be hosted including e.g. album art that is linked in the HTML/metadata files.

Linkhosts SHOULD host cover images and their thumbnail tiers alongside `web.noudata` on the static file server. Thumbnails MUST be in a `/thumbnails` subfolder next to each original image file, using the naming conventions in section 1.25 Supported image formats. Players fetching a release over the web derive thumbnail URLs from the image path referenced in `web.noudata` — there is no separate thumbnail URL in the metadata.

Example folder layout at `https://domain.com/noubin/artist/release/`: (For a static web server)

```
NOUBIN URL FOLDER
|
|-- index.html
|-- web.noudata
|-- cover.png
|
|-- /thumbnails
|   |
|   |-- cover-128.jpg
|   |-- cover-128.webp
|   |-- cover-512.jpg
|   |-- cover-512.webp
|   |-- cover-1024.jpg
|   |-- cover-1024.webp
```

What about the actual media files? 
- If they are for purchase then they would not be here user has to get them from an existing store
- If they are for free download they can be linked to anywhere on the web that supports free downloading of such media 
	- Optionally the linkhost could offer that service and indeed store the media files right here in this folder. 

If `web.noudata` references `cover.png` via `imageLocalFilename` or `imageWebURL`, a player importing the release SHOULD request thumbnails at e.g. `https://domain.com/noubin/artist/release/thumbnails/cover-512.webp` (same path as the image, with `/thumbnails/` inserted before the filename). Linkhosts MAY omit thumbnail tiers the artist has not generated; players use the largest available tier they support. If thumbnails are not found at `/thumbnails/`, players SHOULD retry `/Thumbnails/`.

It is hoped that the ecosystem will shortly produce simple open source software that allows artists to enter release data in a simple form that quickly produces both `web.noudata` and `index.html` files. 


#### 2.1 LINKHOSTS and 404

So linkhosts may get fetches from time to time for content they can't retrieve. They should return HTTP status code 404.
- Linkhosts SHOULD return a human readable error message explaining what happened e.g.
	- "Content never existed at this link"
	- "Content was removed by owner"
	- "Content was removed for reason XYZ"

- Noubin Linkhosts MAY show nice 404 pages to browsers when they fetch the HTML content at Noubin URLs with such messages
- For Noubin Players fetching `web.noudata` JSON files, linkhosts returning 404 SHOULD return a short JSON blob as well with the error message in this format 

```json
{  
"message": "Content was removed by owner"  
}
```

#### 2.2 PRIVACY

It's impossible to prevent that an Artist makes Noubins that have unique links in each and every Noubin. This could be used for interesting behaviour like giving end users totally unique personalised digital experiences, but it can also be abused to track users and know when they have accessed the unique Noubin URL. If this is then connected to purchase data it's possible to know quite a lot about an individual user. This is far more information than is needed by artists for typical marketing purposes to e.g. know in which country/city they have fans. 

- Artists MUST NOT make Noubin URLS that are unique per Noubin
	- Excepted only when the user is clearly informed and told the reason they get a benefit from a unique link when they purchase the Noubin, not hidden in fine print. 
- Web consent mechanisms for tracking and private data collection are already regulated in many jurisdictions. For the removal of doubt Noubin Linkhosts MUST NOT track users using unique links unless they have explicitly consented. 

Note there is no mechanism to offer users a consent banner for cookies/tracking when a Noubin Player accesses a `web.noudata` file. 
- Hence linkhosts MUST NOT collect any non-anonymised data from Noubin Players accessing `web.noudata` files

## 3 NOUBIN PLAYER SPECIFICATIONS

Unless stated explicitly all functionality in this section applies to both Hardware and Software Noubin Players.


#### 3.1 MINIMUM FUNCTIONS OVERVIEW

A Noubin Player MUST
- Read and write NDEF data NFC Forum type 2 and type 4 tags. > see section 3.2 NFC FUNCTIONS
- Play audio out via a speaker, line out, headphones, bluetooth etc
- Support the MINIMUM SUPPORTED MEDIA FORMATS as listed in section 1.23. > see section 3.3 PLAYBACK FUNCTIONS
- At least support decoding and playback of stereo audio files up to 48kHz sample rate and 16-bit depth. > see section 3.3 PLAYBACK FUNCTIONS
- Allow a user to select and play a playable item that is not yet associated with a Noubin
- Allow a user to select a playable item that is not yet associated with a Noubin and associate it with a Noubin NFC tag (Because the Noubin Player might be the only NFC reader the user has access to) > see sections 3.2 NFC FUNCTIONS and 3.4 MEDIA LIBRARY FUNCTIONS
- Have an interface where the user can import media to internal storage OR support inserting of removable media already formatted as a Noubin Media Library > see section 3.4 MEDIA LIBRARY FUNCTIONS
- Read and Write Noubin Media Library format > see section 3.4 MEDIA LIBRARY FUNCTIONS

A Noubin Player MUST NOT crash or block playback due to unrecognised or poorly formatted `.noudata` files. Particularly when encountering unknown property names or uknnown enum values. This is because the standard may be extended in future so players must fail gracefully if they encounter unsupported metadata. 

Generally a player should never block playing of audio as long as you can find valid local files. 
Which means a minimal `.noudata` file should be playable as long as it contains: 
- a `mediaList` array with at least one item 
- that `mediaList` item contains at least one valid `localFilePaths` entry 
- the `localFilePaths` entry points to a valid media file 
- note without a `normalisedNoubinKey` the file is not linked to a Noubin but may still be selectable for playback in the UI. 


Noubin Players MAY
- Support the OPTIONAL SUPPORTED MEDIA FORMATS as listed in section 1.24. > see section 3.3 PLAYBACK FUNCTIONS
- Support complete media library management > see section 3.4 MEDIA LIBRARY FUNCTIONS
- Have web capability > see section 3.5 WEB FUNCTIONS
- Support displaying cover images > see section 1.25 Supported image formats
	- Note players may support only a certain tier of thumbnails and if these aren't available in the media library display no image.
	- Although better UX is for the import process to generate the thumbnails if the player has the performance to do this. 
- Support searching for items (may be impossible on e.g. extremely minimal player with no UI)
- Support saving searches as 'dynamic playlists' so that a user can tap a Noubin to effectively search for all songs by artist, all songs with a genre etc. See section 3.7 DYNAMIC PLAYLISTS and the `searchString` property in `03 metadata-base.md`
- Support user categorisation and ratings of content and searching by these properties
- Support using cue list data to provide users with time annotated functions for e.g. navigating chapters in audiobooks, displaying lyrics / transcript etc. For accessibility this is highly recommended but won't make sense on a minimal player without a screen. 
	- There can be multiple cue lists (e.g. in different languages) so Players should provide a way to activate/deactivate these. 

Whilst the main functionality descriptions are in this document there is some spillover of function description in `03 metadata-base.md`. This is because to some extent functionality is also derived from how metadata should be interpreted. So both documents should be referred to during implementation. 





#### 3.2 NFC FUNCTIONS
- All players MUST give the user feedback that an NFC tag has been detected
	- It's up to the developer how to do this but vibration/sound/visual should be considered.
- Exception: The player SHOULD ignore tags if it's the same NFC tag that is currently loaded/playing (duplicate/repeated tap detection). 
- For Hardware Noubin Players only, they MUST complete the read of all tag data before giving the user feedback that the tag has been detected. 
	- A minor UI cue that a read has begun is acceptable, but shouldn't be confused by the user with the clear 'ding' that a read is complete.
	- This is to prevent the case that the user removes the NFC tag before the read is complete, leading to partial data and key string mismatches. 

- Then there are three steps, a player MUST always do these in this order (local first principle) 
- 1. The contents MUST initially be parsed to see if they match an entry in the local library for playback, see section 6.2 Noubin Key Normalisation. If this fails to locate playable media then go to step 2. 
	- If there are multiple matching entries (same `noubinKeyNormalised`) then the developer can decide what to do (e.g. play the first match, offer the user a choice etc)
	- Nice to have functionality is a warning to the user this is what has happened and even an offer to resolve it. 
- 2. Fallback direct internet access. If all of the following are true:
	- The device is web capable
	- The device currently has an internet connection 
	- The device has an appropriate user interface for communicating info from Noubin URLs to the user
	- The NDEF record is a valid URL 
- Then proceed to the behaviour in section 3.5 WEB FUNCTIONS
- 3. Fallback indirect internet access. If the following is true
	- The NDEF record is a valid URL 
	- The player is a hardware player
- Then the player SHOULD proceed to section 3.6 PASSING A URL TO A MOBILE DEVICE
- Otherwise if no fallbacks are possible the player SHOULD show an error with a user-readable message

- Players MUST support writing NFC tags
	- Even a bare-bones player must support this, why? 
	- Because players MUST support associating a Noubin NFC tag with media. See section 3.4 MEDIA LIBRARY FUNCTIONS (of players)


##### 3.2.1 Additional Performance Requirements for Hardware Noubin Players only: 
- In normal operation the player SHALL check for NFC tags at least 5 times a second
- In normal operations the player SHALL load linked media ready for playback within 1000ms, ideally <300ms. 
- Low power or standby modes that have reduced functionality in the above two regards are permitted. 

#### 3.3 PLAYBACK FUNCTIONS

- It is up to the developer whether tapping a valid Noubin results in immediate playback, loading the file and waiting for the play button, adding the media to the playing next list or some other behaviour.
- It is up to the developer whether currently playing files stop, transition or even continue to play when new media is prepared for playback. 

	- When multiple playback modes are supported preferably let the user choose default behaviour in settings
	- There is the possibility of using custom metadata extensions so the user can set unique preferences for individual Noubins. see `03 metadata-base.md` 


- Players MUST support the MINIMUM SUPPORTED MEDIA FORMATS as listed in section 1.23.
- Players MAY support the OPTIONAL SUPPORTED MEDIA FORMATS as listed in section 1.24. (See QandA.md for background as to why)

- Players MUST at least support decoding and playback of stereo audio files up to 48kHz sample rate and 16-bit depth.
	- Note that mono players are allowed (e.g. a hardware player with one speaker), they should still support stereo files and do a mixdown to mono. 
- Players MAY support higher audio quality
- If a player supports a higher quality media file than it's actual DAC (Digital Analog Converter) supports it MUST downsample and not for example attempt to pipe 24bit data directly into a 16bit DAC and produce unwanted noise art. 
- Players MAY reject playback of files for being too high quality. E.g. because they don't have the bandwidth to stream high quality FLAC off their storage. 
- If a user attempts to play an unsupported quality file the player MUST NOT fail by stuttering or poor playback quality and instead MUST give a clear error before playback explaining why the file can't play and the limits e.g. "The file is 192khz/24 bit FLAC. This player only supports maximum 48Khz/16bit FLAC"
	- A better UX when possible is that the user finds out quality is not supported when they bring files onto the device and not when they try to play the file. 
	- Then the system can potentially offer conversion to the user or point them to free conversion utilities. 
	- Note the user and artist have a lot of agency in the quality department. Artists can release multiple formats on some stores or choose stores that exclusively support extremely high quality formats. So if your player doesn't support a common quality level that artists are releasing on you should educate the user what quality they should buy as part of your onboarding.

- When there are multiple valid media files available for a single media item (i.e. multiple entries in the `localFilePaths` array) the player MUST play the highest quality file that it supports
	- the player MAY give the user feedback which file type it is playing and in what quality (if it has an appropriate UI) so users can have assurance of the quality that is being heard
	- the player MAY give the user the option to play a specific alternate file (if it has an appropriate UI) and to save that as the preferred one for future. 

#### 3.4 MEDIA LIBRARY FUNCTIONS (of players)

- If a Player has media library functions it MUST implement section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS for all touchpoints where the user will interact with media and metadata files. E.g. merging and exporting media from web interface, removable drive, backup file etc. 
	- This means media libraries are transferrable between Noubin Players of all kinds. Turn up at your friends house and share your music to their player. 
	- Note for internal only storage, players may use a different format if the developer wishes. But any export function must produce an export formatted according to section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS.
- Players MUST implement a function where a user can associate a Noubin with a playable item
	- This should create/set appropriate cross-compatible `.noudata` metadata in the media library, including `noubinKey` and `noubinKeyNormalised` (see sections 6.1–6.2 and section 4.7 UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY)
	- This means if a Noubin has an NFC tag with blank NDEF record the player must create one.
	- Or if the user provide an NFC tag that already has a first NDEF record it must be ovewrriten. 
	- See section 1.16 Local Noubin Key for recommended format
	- Avoid collisions with existing `noubinKeyNormalised` values in the library by appending a random string. 
	- Note the player will have to adapt to the available capacity of the NFC tag, some are as low as 48 bytes and may not fit a string with a long or complicated `playableItemTitle`. So the string should not be generated or set in the `.noudata` file until the moment the NFC tag capacity is confirmed. 
- Players MAY implement further media and tag management functions including
	- Add media to the library
	- Put media into playlists
	- Edit metadata
	- Identify orphan media without metadata files and generate metadata files
	- Detect metadata files linked to non-existent media and attempt to fix
	- Delete media from the library
	- See the total size of the library and remaining space on storage volumes
- Note Hardware Players do not necessarily have to provide these functions on their built-in UI. Other options could include
	- Web interface (over LAN)
	- App
	- Offline app - preparing the data on an external storage device then connecting it to the hardware player. 

- Note: Why make media management an optional function of players? Isn't a user stuck if they buy a player and can't manage the media?
	- No, because the media library format at its simplest is just putting files in a folder. This can be done on any computer and then the library transferred by e.g. removable media. The only step that the Noubin Player MUST support is associating a Noubin with a playable item as desktop computers don't typically have NFC transceivers. 
	- This enables Hardware Noubin Players that are minimal, physical, local and non screen based.
	- Plus hopefully there will be an ecosystem of open source player software and media management tools for convenient and rich media library management. 

#### 3.5 WEB FUNCTIONS

Players MAY support web functions. 
If the player supports web functions then all the following "MUST" and "SHOULD" behaviours apply. 

**General web function and security considerations**
- The standard aims to protect user privacy. 
- The player MUST give the user a choice whether to access any URL or resource on the web along with an appropriate message like "local media not found, access Noubin URL on the internet?"
	- For privacy reasons it must be very clear to the user that internet access will occur if they proceed
	- If the player has a screen it MUST show the user as much of the URL as it can reasonably fit  
	- Bear in mind we can't prevent malicious linkhosts from tracking when links are accessed. We don't want to give them a view into every time the user plays the media. 
	- Once the user has the release locally then there MUST NOT be any automated checks from the player looking for e.g. updated cover art. If such a check is offered the end-user must consent to web access. 
	- Manually initiated checks by the user are OK as long as it's clear to the user this involves web access. 
- If the player can download the release it MUST use the section 4.7 UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY procedure. This attaches the canonical web metadata, media files and associated content (e.g. images) together in the library so no further web access is required for future taps.  
- All URLs MUST become `https` if it's `http`
- Note that it's possible for bad actors or malfunctioning web servers to put malicious payloads or 5GB files at the Noubin URL. 
- The player MUST implement appropriate security features for a web device including
	- Not accepting incoming connections
	- Timeouts
	- Appropriate maximum size limits for fetches. 
		- 1MB is expected to be more than enough for `noudata` metadata files. 
	- Dropping connections if incoming data stream does not match expected format (e.g. HTML provided when expecting JSON, non media file at direct download link)
	- Deleting failed downloads/fetches
	- Maximum redirect limits 
	- Properly sanitising and handling all input, including ALL metadata in `.noudata` files, to prevent SQL injection attacks or equivalent. 


**Accessing Noubin URLs**
- The Noubin URL by default points to a web page for opening in a normal browser (fallback if tapped on a phone without a player app)
- To fetch `web.noudata`, the player MUST first apply **Noubin URL Normalisation** to the Noubin Key from the tag (see section 6.3), then perform a HTTPS GET request on the resulting Canonical Metadata URL
- If the GET request returns 200 success the player MUST attempt to parse the payload as JSON following the schema in `03 metadata-base.md` 
- The player MUST first show the user the overview of what the expected media content is as listed in the metadata. E.g. name of media and track listing if available.
- The player MAY fetch cover art from the same linkhost without a further consent request. 
- If the metadata includes direct media download links (for free content) and the player is capable of direct downloading then the player MUST offer downloading this content as the first action to the user before release platforms. see Direct Media Downloading below in this section
- Then the user can use the UI to move to the release platforms list.
- The player MUST show platforms in the order that the Artist put them in the metadata file with the appropriate call to action text (see `03 metadata-base.md`)
- If the user selects a platform for purchasing/streaming etc that the player cannot deal with directly (which will be all of them until stores support authentication by players) then the link should be passed to the users device following section 3.6 PASSING A URL TO A MOBILE DEVICE 
- If a Noubin URL web request fails on a hardware player the player MUST display the error message with an option like "open on your device" to offer that the URL can be passed to a mobile device see section 3.6 PASSING A URL TO A MOBILE DEVICE
	- Note this won't always make sense to open on your own device. 404 errors are likely to get the same result on a mobile device but it's up to the user to decide. 
- If a 404 response includes a JSON formatted human readable error message (see section 2.1 LINKHOSTS and 404) than the player SHOULD display it. 

**Direct Media Downloading**
- If media files are available for direct download the player MUST prompt the user if they wish to download, and show the user the URL that will be accessed (or list of URLs, the user can consent at once to all)
- If the user agrees the media player MUST perform a HTTPS GET request to the appropriate URL and show download status in the interface
- If the download completes successfully the player MUST put the media in the users library following the organisation principles in section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS, including the section 4.7 procedure 
- The player MUST create a `.noudata` file named and organised as per section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS 
	- This file MUST be based on the `web.noudata` data
	- This file MUST now be named according to section 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS
	- The file MUST be appended with information about the actual downloaded local media files, i.e. set the `localFilePaths` property for each item. See section 4.7 UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY 

NOTE
- In a future version of this standard it's anticipated that Noubin Players will be able to extend web functionality to negotiate with stores directly, particularly for identifying media that the user already owns and downloading it to the local library. 
- This will involve an authorisation procedure that a storefront must also allow on their side.
- If the standard proves popular and we find a store that wants to participate, we will extend the standard with this functionality. 

#### 3.6 PASSING A URL TO A MOBILE DEVICE

Procedure for hardware players to transfer a URL to a device that has a browser.

- Players MUST show as much of the URL as possible and give the user a message along the lines of "tap your phone to access the link" 
	- Unlike other web operations a UI consent dialog for accessing an internet resource this way is not required. The user has the choice whether they proceed by choosing to tap their phone on the NFC transceiver or not. 
- The player MUST put the NFC transceiver into tag emulation mode and provision it with the Noubin URL as a NDEF URI record
- NFC based link sharing is the minimum requirement. Alternatively other methods for passing the URL to a device are permitted like showing a QR code, pushing the link to an app or showing it in a LAN web UI if the hardware device supports it.

#### 3.7 DYNAMIC PLAYLISTS

Players MAY support dynamic playlists (see section 1.27 Dynamic Playlist and `searchString` in `03 metadata-base.md`).

A dynamic playlist is a `.noudata` playable item where:
- `localPlaybackData.searchString` is set to a library search query
- `mediaList` is empty (or omitted)

When the user taps the associated Noubin, the player MUST run the search against the media library, use the results as the playlist for that playback session, and respect other `localPlaybackData` settings (e.g. `shuffle`).

- Players MUST NOT treat an empty `mediaList` as invalid when `searchString` is present and dynamic playlists are supported
- Players MUST NOT reject the `.noudata` file during library scan solely because it has no `mediaList` entries in this case
- Players that do NOT support dynamic playlists MUST show an appropriate error if user tries to play one e.g. "The item is a dynamic playlist. This player does not support dynamic playlists"



## 4 NOUBIN MEDIA LIBRARY SPECIFICATIONS 

#### 4.1 FILE SYSTEM
- Hardware players MAY have their own internal storage or rely on user provided removable storage e.g. usb stick
	- Manufacturers MUST ensure this is very clear in marketing materials so that users know if they also have to provide their own removable storage device. 

- Players MUST support exFAT and FAT32 as the file system formats for removable storage read and write. exFAT is preferred.
	- Tiny microcontroller based players who will struggle with indexing large media libraries (particularly on exFAT) are reccomended to make available media management software so the user can pre-index the media on their desktop computer. See section 4.6 PLAYER DATABASES AND METADATA FILES. 

- When Noubin player or media management software puts a users media library on a removable storage device it should check if the file system format is supported, and if not warn the user that it may not be readable by Players otherwise. 
	- To be clear a player does not have to provide drive formatting capability itself, it can direct users to common utilities for this.


#### 4.2 ROOT FOLDER
- A media library is a folder that contains media files and metadata readable by a Noubin Player
- It can be the root folder of a removable storage device OR a deeper folder in a file system e.g. `C:\myMedia\` 
- Software players MUST have a function to select that folder
- Hardware players SHOULD have a function to select that folder if they have an appropriate UI
	- However as a default and fallback for devices without screens, it's ok to assume the root folder of a removable storage device is a media library. 

#### 4.3 VERBAGE
- Players and media library management software MUST use consistent terminology as follows:
	- When a player `OPENS` a media library it's available to play or manage. Files are not moved. Libraries can be opened on removable drives. 
	- When a player `MERGES` a media library it imports selected files from a source library and copies them to a target library, helping the user resolve conflicts if needed.
	- When a player `EXPORTS` a media library they copy selected content to a target empty/new media library location as a new media library. The target can be a removable drive or a specific folder
		- If the target folder already contains a media library than the function should `MERGE` instead
	- When a player `BACKS UP` a media library the entire library to a target empty/new media library location, which can be a removable drive or a specific folder
		- If the target folder already contains a media library than the user should be warned and asked if they want to replace or `merge` 

#### 4.4 FOLDER ORGANISATION
- In media libraries the preferred folder naming and organisation is
	- for music `Music/<artist>/<release>/` 
		- e.g. `Music/Prince/Purple Rain/` 
		- Note `<artist>` is context dependant and can mean composer, performer etc. 
		- Does not have to be perfect here. The actual artist for display by the player is not determined from the folder name, but rather from metadata.
	- for podcasts `Podcasts/<podcast Title>/` 
		- e.g. `Podcasts/Freakonomics Radio/`
	- for audiobooks `Audiobooks/<author Lastname, <author Firstname>/<audiobook Title>/` 
		- e.g. `Audiobooks/Weir, Andy/Project Hail Mary/`
	- for mixtapes `Music/Mixes/<DJ Name>/`  
		- e.g. `Music/Mixes/Fatboy Slim/`
- However this organisation is entirely up to the users preference. They might add more categories e.g. /comedy or say "I only play music it's just gonna be music at the root"
	- Noubin Players MUST be able to deal with libraries arranged however the user wishes, including chaotic real life scenario of no organisation at all. 
		- Neither chaotic folder organisation or file naming should prevent Noubin Players from using a library. They only need to generate/read `.noudata` files somewhere in the library to associate Noubins and metadata with those files. 
		- This is because a Noubin Player might also access/use a media library that's actually been setup and is still in use by a different media player app e.g. Kodi. Apart from adding `.noudata` files Noubin Players should be able to cope with media library file and folder organisation as is and not require end-users to maintain multiple different media libraries with different folder organisation and file namings. 
		- Which I know introduces some complexity but at the end of the day the job of software is to make the users life easier. It's not the job of the user to make things easy for the software. 
- Players and media library management apps MUST scan all folders inside the library for media and metadata files, including subfolders up to at least 8 levels deep 
	- Except players and media library management apps MUST NOT scan folders whose names start with a period ( `.` ).  Folders such as `.<playerName>/` (player cache databases) are therefore excluded from regular library scans.
- A player MAY be able to `OPEN` multiple media libraries e.g. internal library plus a library on removable storage plus a library on the network 
- Playlists SHOULD be stored in a `/Playlists` folder off the media library root
- Thumbnails of images MUST be stored in a `/thumbnails` subfolder next to any original images (NOTE lowercase `t`, to match linkhost and URL conventions in section 2).
	- When importing cover art from a Noubin URL, players SHOULD save downloaded thumbnails into `/thumbnails` locally even if the linkhost served them from `/Thumbnails`.
	- Players SHOULD look for `/thumbnails` with a lowercase `t` first when loading images from a media library, and SHOULD fall back to `/Thumbnails` with a capital `T`.  

- When generated from `web.noudata`  all folder names should follow the artists capitalisation
	- e.g. if `playableItemTitle` (release title) is "Hello" then the `<release>` folder would be called "Hello"
	- if `playableItemTitle` (release title) is "HELLO" then the `<release>` folder would be called "HELLO"
- If ambiguous preferred styling is regular capitalisation e.g. "Hello"

#### 4.5 NOUDATA FILE ORGANISATION

- Noudata files MUST end with the extension `.noudata`
- Players MUST scan a media library for all `.noudata` files and ingest them (and not rely that they always follow the naming conventions below). 
- Players MUST preference the `.noudata` file as the first source of truth for all data, over and above factors like the names of files, names of folders, embedded metadata in audio files.  
	- How do Players know which `.noudata` file contains authoritative information for a given file? This should be built up during a scan of the media library, the player can check `.noudata` files, their `localFilePaths` entries and also check the actual media files. 
	- Then the Player knows if a certain `_<release>.noudata` file accounts for all the tracks in its folder as the authoritative source or if this is a folder with single files. 
- So from the perspective of the Player the `.noudata` file name, location in the media library, the names of the folder it's in etc is irrelevant. It's a valid Playable Item as long as the `.noudata` files contains working paths to local media files that can be played. See the metadata content documents for more information. 


- `.noudata` files SHOULD follow these naming and organisation conventions
	- For single media files Noudata files SHOULD have the name of the media file and be organised next to the media file.
		- e.g. `01 - Sweet Intro.mp3` has `01 - Sweet Intro.noudata` or `episode123 - guest.mp3` has `episode123 - guest.noudata` 
		- single track `.noudata` files are not needed when appropriate metadata is already available for this file in a `_<release>.noudata` file.
		- `.noudata` files for single items only need to be generated if 
			- A) the user wishes to link a single media item to a Noubin tag 
			- B) the user wishes to create rich metadata for orphan files that aren't listed in a `_<release>.noudata` file. 
		- So typically a folder for a well organised album only has one `_<albumName>.noudata` and not lots of individual `<trackName>.noudata` files
		- It will be common to have many single `.noudata` files: 	
			- In a chaotic folder that doesn't belong to a single release, to keep track of metadata for many individual media files OR
			- In folder like a folder of podcast episodes or audiobooks where it's the norm that each is a single playable item
	- `.noudata` files referring to a collection of media (e.g. an album or playlist) SHOULD have filenames starting with an underscore e.g. `_Purple Rain.noudata`
		- This will put them visually at the top of the folder for users in standard OS file viewers looking at files in alphabetical order. 
		- As opposed to being 'mixed in' visually with the media file, which is particularly confusing if the album shares a name with one of the tracks. 
	- For releases with multiple tracks the name of the `.noudata` file SHOULD be the release/album name  `_<release>.noudata` (generally from `playableItemTitle`) and placed in the release folder so `Music/<artist>/<release>/_<release>.noudata`  
	- All other `.noudata` files referring to multiple local files that are not releases (AKA playlists) SHOULD also be named according to their `playableItemTitle` the user wants them known as e.g. `_MorningJazz.noudata` 
		- Additionally playlists should be organised into a `Playlists/` folder off the root of the media library e.g. `Playlists/_MorningJazz.noudata`
		- Playlist files should not contain `releaseData`, `credits`, `releasePlatforms` etc 
		- This data should instead be read from the `_<release>.noudata` or single `.noudata` file associated with the specific media files.
		- Playlist `.noudata` files may contain `localCoverImage` (under `localPlaybackData`) to give a playlist a cover image
	- NOTE: what about `web.noudata` files? This naming is only used at Noubin URLS to contain data provided by an artist that isn't yet associated with local files.
		- So `web.noudata` files generally shouldn't appear in a media library
		- For more information see section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS and section 4.7 UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY 

- Edge case for the single `.noudata` files: what if a release has a single track, and the release and the track have different titles? Do you make a `_<release>.noudata` or a single track `<trackname>.noudata`?
	- In this case the single track rule applies: All `.noudata` files that only contain a single track take the same name as the media file filename. 
	- e.g. a release titled "Neon Drift EP" contains one track titled "Neon Drift (Radio Mix)" which is the media file `01 - Neon Drift (Radio Mix).mp3`
	  - the `.noudata` file would be called `01 - Neon Drift (Radio Mix).noudata`. 
	  - In the `.noudata` file set `playableItemTitle` to "Neon Drift EP" and the `mediaTitle` of the track to "Neon Drift (Radio Mix)" so the player still puts the correct information in UI display etc.
	- The reason for this is so that in a standard file browser the `.noudata` files for single tracks always appear next to the media file (when viewing in alphabetical order). Then if a user is moving files manually it encourages them to keep the files together. Particularly important because single tracks often end up in folders with many other single tracks. 
		- Note these single media file `.noudata` files will be common for many audiobooks and podcasts as well.  

- If an item is canonically renamed inside player or media management software than file names and organisation SHOULD follow e.g. if an album is renamed than it should be renamed in the noudata file metadata, name of the noudata file itself and name of the folder the media is in.
	- So `artist/album/_album.noudata` becomes `artist/album2/_album2.noudata`


With the file and folder rules above combined the structure of a typical release folder looks like 

Structure (nice example)
```
MEDIA LIBRARY ROOT
| 
|-- /Playlists
|   |
|   |-- _hardRock.noudata
|
|-- /Music/<artist>/<release>
	|
	|-- _<release>.noudata
	|-- <mediaFile1>.mp3
	|-- <mediaFile2>.mp3
	|-- <mediaFile2>.noudata 
	|-- <mediaFile3>.mp3
	|-- <coverImage>.png 
	|
	|-- /thumbnails
	         |
	         |--<coverImage>-128.jpg
	         |--<coverImage>-128.webp
	         |--<coverImage>-512.jpg
	         |--<coverImage>-512.webp
	         |--<coverImage>-1024.jpg
	         |--<coverImage>-1024.webp

```

Note in the example above the user has also linked `<mediafile2>` to a noubin so it has an invidual `.noudata` file.  
- Its `referToReleaseNoudata` property should point at `_<release>.noudata` so that the information about the release is not duplicated into that file but read from `_<release>.noudata`

When media libraries are more chaotic they may look like this 


Structure (Chaotic example)
```
MEDIA LIBRARY ROOT
|
|
|
| #Chaotic random files
|-- <mediaFile1>.mp3
|-- <mediaFile1>.noudata
|-- <mediaFile2>.mp3
|-- <mediaFile2>.noudata 
|-- <mediaFile3>.mp3
|-- <mediaFile3>.noudata
|-- <coverImage(1)>.png 
|-- <coverImage(2)>.png 
|
|-- /thumbnails
         |
         |--<coverImage(1)>-128.jpg
         |--<coverImage(1)>-128.webp
         |--<coverImage(1)>-512.jpg
         |--<coverImage(1)>-512.webp
         |--<coverImage(1)>-1024.jpg
         |--<coverImage(1)>-1024.webp
         |--<coverImage(2)>-128.jpg
         |--<coverImage(2)>-128.webp
         |--<coverImage(2)>-512.jpg
         |--<coverImage(2)>-512.webp
         |--<coverImage(2)>-1024.jpg
         |--<coverImage(2)>-1024.webp

```

In the chaotic example we also have multiple `.noudata` files associated with single media files. This time there is no nice `_<release>.noudata` file to associate with these. So in that case the single metadata files don't have any value in the `referToReleaseNoudata` property and instead contain their own `releaseData`, `credits` `coverImage` properties etc. Which is how then they can link to uniquely named image files in that same folder.  


#### 4.6 PLAYER DATABASES AND METADATA FILES
- The source of truth for a media library at rest is all the individual `.noudata` files, however accessing thousands of individual tiny files to e.g. browse playable items has poor performance in most cases. So the reccomended strategy is players build up a database/index of the media and its metadata.  
- When a player first `OPENS` a media library the player SHOULD build up an initial database by scanning every folder. 
	- The user should be given a nice UI explaining scanning is going on if it will have an averse impact on performance. 
	- For best performance generally and faster boot in future the player SHOULD store any database 
	- The database itself can be stored in the media library (see below)
	- After initial scan it's recommended to use lazy scanning methods to only look for changes
	- The player SHOULD index `noubinKeyNormalised` values from `.noudata` files for fast NFC tap lookup
- Developers of players can choose their own database format as best suits their player architecture and performance requirements. 
	- If saved in the media library the database MUST be saved to a folder whose name starts with a period, named after the player: `.<player>/` e.g. `.plippa/` (such folders are excluded from regular media scans by rules in section 4.4 FOLDER ORGANISATION)
- It's RECCOMENDED that tiny microcontroller based players which will struggle with indexing large media libraries make available media management software to users they can run on their desktop computer which generates the indexed database files in advance. 
- When media management functions are used that make changes to the database, e.g. editing a playlist, players and media management software MUST also propagate change to the individual `.noudata` metadata files.
- Players and library media management software MAY scan media files directly for embedded metadata, particularly for creating or augmenting `.noudata` metadata files
	- This is optional because embedded metadata formats are relatively inconsistent across audio file formats, asking modest hardware players in particular to have all the libraries to process this is burdensome. 
	- Note when relevant data exists in a proper `.noudata` file it MUST always take precedence over audio file embedded metadata 

#### 4.7 UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY 

Also known as a `unite` operation.

This is an important procedure because the reality today is that stores process and name media files differently and use different audio formats, and users may specifically choose a certain store for a certain release because they want those features. E.g. User may choose FLAC files from store A for some releases and MP3 files from store B for others. So the import process has to unite the artists canon metadata with the differing files the user may obtain from stores. 

Both media library software and players will be referred to as 'software' here. 

- The ideal outcome is to associate the media files with canon `noudata` metadata. 
- There are three main cases
	- A) Media files directly downloaded after the user tapped a Noubin
		- The files were either freely downloadable
		- Or in future available after authorisation with a store 
	- B) Media files imported by the user after being purchased and manually downloaded from a store 
	- C) Media files imported from another Noubin media library or (if the artist is so kind and the store allows) purchased and already including a `_<release>.noudata` file e.g. zipped with the media files. 

- If media files are detected that are not supported by the Noubin Player standard (e.g. because they are video files) then the user should be warned.
- Optionally if the software knows the limitations of the specific player (because e.g. it's the official media management software of the player) the user can already be warned if they are using unsupported optional formats or qualities. 
	- Ideally go further and offer to convert the media to supported formats (or point the user towards free tools for this).

- For case B: 
	- When the user wants to associate a Noubin with this media the software should prompt them if they have an official artist Noubin or Noubin URL they wish to use to get official release metadata from the internet. 
		- Ideally they have the Noubin and can just tap it
		- Fallback they can manually enter the URL which they could read by tapping the Noubin on their phone and copy / paste / share to a computer without a NFC reader.
	- The software then follows the procedure from section 3.5 WEB FUNCTIONS to access `web.noudata` and associated album art. 

- Continuing for Case A and B: 
	- The software now has `web.noudata` and a set of local media files that are meant to be linked together.
	- The first thing is the software should check the `playableItemTitle` property and media file names for illegal characters as per section 1.26 Safe file and folder names. 
		- Generate a `playableItemSafeTitle` if required
		- Rename media files if required
		- see `03 metadata-base.md` entry for `playableItemSafeTitle` for more information
	- The `web.noudata` will contain a section listing the files and their expected names and lengths
	- Depending on the store the files may be different formats, have small variations in length etc.
		- The files may also have relevant metadata embedded in the file
	- The software should first attempt to use the expected names and lengths to automatically match the files to the metadata.
		- Where multiple good candidates are found (e.g. because user bought both MP3 and FLAC files) multiple files can be added to the `localFilePaths` array
	- If matches are not found for track entries in `web.noudata` then prompt the user to allocate remaining media files manually and when relevant show them files mentioned in the release are missing/can't be found.
	- When all `mediaList` elements from `web.noudata` have at least one `localFilePaths` array entry AND all media files are accounted for the procedure is complete.
	- The now united metadata is saved as `_<release>.noudata` in the release folder (or `<trackName>.noudata` for a release with a single media item)
	- The player MUST set `noubinKey` to the literal Noubin Key extracted from the tapped tag's NDEF record (see section 6.1 Extracting the `noubinKey` from an NDEF record)
	- The player MUST set `noubinKeyNormalised` by applying Noubin Key Normalisation to that value (see section 6.2). This SHOULD also be indexed in the player's library database (e.g. a hash table) so future taps can be matched without re-normalising on every read
	- In case of errors the user should have an interface where they can reassign the linking of files to tracks
	- If album art is included both in purchased media files and also has been downloaded from the `web.noudata` metadata compare resolution of files and keep the higher resolution one as the primary album art entry.
		- Rename any non used art and keep it in the folder as e.g. `<albumArt>-alt.png`
		- Provide the user with an interface to manage this 

For Case C: 
- Because there is already a `_<release>.noudata` the only thing that needs to be done is associating it with a Noubin should the user wish (setting `noubinKey` and `noubinKeyNormalised` in `_<release>.noudata`)
- Though if the user is importing an existing Noubin media library (e.g. because they are getting a new Noubin Player) they may already have the Noubins associated. 

###### 4.7.1 Reference: heuristics for uniting `mediaTitle` and `uniteData` with actual media filenames

The most common patterns for file names of media downloaded from stores is 
- For music
	- `## - trackname.ext` where `##` is the track number. e.g. `01 - Sandstorm.mp3`
	- `artist - trackname.ext` e.g. `Darude - Sandstorm.mp3`
- Podcast titles often `episode 123 - topic.mp3` or `episode 123 - guest.mp3`
- Audio books can be
	- chapter name and number e.g. `Chapter 01.m4b` 
	- Older releases may be just one huge item
- Mixtapes will usually just be one huge file and names can vary a lot, but they are often freely downloadable with a direct link in `uniteData` so hopefully no `unite` operation required.

And the software should ideally know from `web.noudata` 
- `releaseData.releaseProfile` e.g. `music | podcast | audiobook | mix | other`
- `mediaTitle` for this item
	- But note may include characters that aren't possible in paths
- `expectedFilename` in `uniteData`
- `expectedLength` in `uniteData`
- Knows the track number based on the order of the element in the `mediaList` array

BUT it's always possible that this data is inaccurate. And storefronts mutate data before it makes it into files and filenames, e.g. removing illegal characters, slightly changing the length etc.

When we have a good implementation in the first software we will explain it here. 

#### 4.8 IMPORTING IMAGES AND  THUMBNAILS

- When an image is linked in a `.noudata` file (relative or URL) then the player / media management software should check for thumbnails in the `/thumbnails` folder following the naming conventions in section 1.25 Supported image formats (with `/Thumbnails` as a fallback if not found)
- When accessing a release from the web at a Noubin URL, or `merging` it into a library, players and media management software MUST check for and also download/copy thumbnail files relating to an image file, using the `/thumbnails` layout documented in section 2 NOUBIN URL AND LINKHOST SPECIFICATIONS. Save imported thumbnails under `/thumbnails` in the local library.
- When images are downloaded from the web they should be put in the release folder and the `_<release>.noudata` file should be updated with the now local filenames in each relevant `images[].imageLocalFilename` entry.
	- Note there are three places in the metadata format where images can potentially be downloaded and should be locally saved once this has occured
		- in `releaseData.coverImage.images` array 
		- in `localPlaybackData.localCoverImage.images` array
		- in `mediaList.item.itemCoverImage.images` array 

#### 4.9 WHEN IS A `.noudata` FILE A PLAYLIST?

- A `.noudata` file (playable item) is NOT a playlist IF
	- it contains a valid `releaseData` object
	- it contains `referToReleaseNoudata` that points to a valid `.noudata` file with a valid `releaseData` object
- Otherwise it's a playlist

Note: a per-track `.noudata` file used to link one song on an album to its own Noubin (with `referToReleaseNoudata` pointing at `_<release>.noudata` but no `releaseData` of its own) is **not** a playlist. Implementers MUST NOT treat every `.noudata` file without `releaseData` as a playlist — check `referToReleaseNoudata` first.

Equally playlists should not have `releaseData` object. If the user wants to set an author for the playlist use `createdByUser` property in the `userNotes` object. 

Note when the user sets cover images of their own (relevant mostly for playlists) that goes in `localPlaybackData.localCoverImage` not in `releaseData.coverImage`. (As creating the `releaseData` object would then turn this playlist into a release)

#### 4.10 REFERENCE COMBINING TITLE INFORMATION

- All up to show a user 'what's playing right now' that can mean displaying `playableItemTitle`, `mediaTitle`,  PLUS some info about the primary artist PLUS any relevant timestamped data like chapter title 
	- e.g. worst case someone made a playlist of DJ mixes that are annotated with names of the songs that are playing as timestamps.
		- `playableItemTitle` = "Skaturday Tunes"
		- `mediaTitle` of this track = "JAPANESE ROCKSTEADY SET VOL. 3"
		- `primaryArtists` (only entry) of this track = "Japanese Rude Sounds"
		- and the currently playing song in the mix itself according to the timestamp chapter name is "Wicked Babylon - Clippers"
	- So the full display of what's playing might be 
```
Skaturday Tunes: 
JAPANESE ROCKSTEADY SET VOL. 3 
by Japanese Rude Sounds
6:33 Wicked Babylon - Clippers
```

It's up to players how much detail they can show here depending on their UI.  


## 5 NOUBIN LAZY TIMESTAMP FORMAT

### 5.1 Background 
We need a timestamp format for adding time based annotations to media, e.g. lyrics, chapters etc.

Design goals:
- Human readable
- Human writeable, it shouldn't break with typical lazy entry patterns.  
- Machine readable
- Machine writeable 
- Not locked to "frames" or any concept that requires defining a time base like e.g. 60FPS. 
- Suitable for content many hours long (podcast) or just a few minutes long (song)

The WebVTT subtitle timestamp format is great, meeting all the requirements above `HH:MM:SS.mmm` where
- HH = hours
- MM = minutes
- SS = seconds
- mmm = milliseconds 

EXCEPT Only problem is in WebVTT files it needs to be strictly the entire timestamp to be valid, including 3 digits on the milliseconds. For human hand entry including the hour digits every time and strictly 3 digit milliseconds is a bit tedious. 

SO we will take the WebVTT format and add some "lazy parsing" rules. 

### 5.2 Spec

A NOUBIN Lazy Timestamp represents a position on a media timeline measured from the start of playback (`00:00:00.000`).

A timestamp MAY be written using between 1 and 4 components separated by `:` and optionally ending with a fractional seconds component introduced by `.`.

Valid forms are:

```text
SS
SS.mmm
MM:SS
HH:MM:SS
HH:MM:SS.mmm
MM:SS.mmm

```

Examples:

```text
45
45.25
12:55
1:02:03
1:02:03.125
12:55.5
```

Automated systems writing timestamps MUST use the full form `HH:MM:SS.mmm`
- They SHOULD pad leading zeros on the `HH:MM:SS`
- They SHOULD NOT pad trailing zeros on the `mmm`

#### 5.2.1 Parsing Rules

Components are interpreted from right to left.

| Number of `:` separated components | Interpretation |
|-----------------------------------|----------------|
| 1 | SS |
| 2 | MM:SS |
| 3 | HH:MM:SS |

Examples:

```text
45        = 45 seconds
12:55     = 12 minutes 55 seconds
1:02:03   = 1 hour 2 minutes 3 seconds
```

Each individual component should be interpreted as an integer regardless of the number of digits, padding of zeroes etc. More digits than appears in the spec is allowed (see validation below)

#### 5.2.2 Fractional Seconds

A decimal point MAY be appended to the final seconds component.

Examples:

```text
12.5        = 12.5 seconds
1:23.75     = 1 minute 23.75 seconds
2:01:03.12  = 2 hours 1 minute 3.12 seconds
```

The fractional component represents decimal fractions of a second and is not limited to exactly three digits.

The following values are equivalent:

```text
1:23.1
1:23.10
1:23.100
```

#### 5.2.3 Other validation rules

- All values MAY exceed regular clock maximum values. E.g. minutes and seconds values MAY be higher than 59 and MAY be more than 2 digits.  
	- This comes from a simple integer interpretation of each component.
	- This makes it permissible to write `120` as a timestamp which would be interpreted as 120 seconds, e.g. 2 minutes. 
	- The reason is to enable simpler hand copying from systems that only use seconds for timestamps. 
	- Hours theoretically MAY be over `99` as well (for a really epic audiobook)
- Negative values are not permitted.
- Whitespace before and after a timestamp SHOULD be ignored.







## 6 NOUBIN NDEF DATA TRANSFORMS

This section defines how players interpret data read from Noubin NFC tags.

To recap: 
- A **Noubin URL** is the full release hyperlink on the internet — a complete URL with `https://`, query parameters, path components, and so on. Noubin URLs SHOULD be well formatted (see section 2), but players apply **Noubin URL Normalisation** (section 6.3) when fetching `web.noudata` to cope with minor malformations.
- A **Noubin Key** is the literal string stored in the tag's NDEF record: either plain text or a Noubin URL This is what the artist (or user) actually wrote to the chip.
	- Note when it's an NDEF URI the Key is the NDEF URI with the URI prefix byte expanded to a full `http://` or `https://` string, potentially including `www.` also).


So then when a Noubin is tapped there are two different procedures that can happen, both start with the same first step
- `noubinKey` extraction (section 6.1)
And then 
- Noubin Key Normalisation (section 6.2): applied when matching a tap against the local library. Produces the `noubinKeyNormalised` value stored in `.noudata` files during the `unite` operation or when associating a tag manually.
If that fails (no local match) AND the `noubinKey` is a URL AND the Noubin player is web capable: it can offer the user to fetch data from the web about this Noubin. 
IF the user agrees then the extracted `noubinKey` from the first step is normalised in a different procedure
- Noubin URL Normalisation (section 6.3): applied when players access the internet to fetch `web.noudata`. Turns the Noubin Key into a reliable fetch URL.

Both procedures share the same first step (section 6.1 Extracting the Noubin Key from an NDEF record) to get a `noubinKey` 

#### 6.1 Extracting the `noubinKey` from an NDEF record

**Phase 1: NDEF Record check**
- The player MUST check if an NDEF record is available
- Only URI (TNF = 1, Type = "U") or Text (TNF = 1, Type = "T") records are acceptable. 
- Failing here SHOULD result in an error message to the user that the tag contains no data or non supported NDEF type and how to fix

**Phase 2: Payload Extraction**
- The player MUST process the first valid NDEF record. 
* **For NDEF URI:** 
	1. The player MUST fully expand the URI payload by resolving the URI Identifier Code byte into its full string prefix (e.g., `0x04` becomes `https://`).
    2. The player MUST decode the fully expanded payload into a UTF-8 string. 
	3. SPECIAL CASE **`noubin.com` redirect unwrapping (URL Noubin Keys only):** If the Noubin Key is a URL whose host is `noubin.com` or a subdomain of `noubin.com`, AND it contains a `?redirect` query parameter, the player MUST replace the Noubin Key with the URL-decoded value of that parameter (including any query parameters on the decoded URL) before continuing. See ADDENDUM1.
		- NOTE `noubin.com` urls that don't contain a `?redirect` query parameter should be left as is and in that case `noubin.com` is treated as a regular linkhost. 
	- NOTE At this point if there are query parameters or any other URL components beyond host and path (e.g. port numbers, `#` fragments, `;` matrix parameters, userinfo before `@`) this data remains and is included in the `noubinKey`
	- NOTE Percent-encoded sequences (e.g. `%20`, `%2F`, `%3A`) are also left as-is in the `noubinKey`, whether they appear in the path, query, fragment, or elsewhere. Do not URL-decode the path at this stage. The only decoding in Phase 2 is the special `?redirect=` unwrap in step 3 above, which URL-decodes that parameter's value when replacing the Noubin Key.
* **For NDEF TEXT:**
    1. The player MUST read the initial Status Byte to determine the encoding and the length of the Language Code.
    2. The player MUST discard the Status Byte and the Language Code bytes.
    3. The player MUST decode the remaining bytes into a UTF-8 string (converting from UTF-16 to UTF-8 if indicated by the Status Byte to prevent endianness issues).

The resulting string is the **Noubin Key**. When storing in a `.noudata` file as `noubinKey`, players MUST save this literal extracted value without further modification.
At this point the `noubinKey` could be written back into a Noubin and the Noubin would function the same as it did originally (including query parameters for example).

**Extracting the `noubinKey` Examples:**

Each example shows what is encoded on the tag → the resulting **Noubin Key** (stored as `noubinKey`). Only Phase 1–2 applies here; no case folding, trimming, or protocol stripping yet.

*NDEF URI records (prefix byte expanded, then redirect unwrap if applicable):*
- Prefix `0x04` (`https://`) + payload `artist.com/noubin/purple-rain/` → `https://artist.com/noubin/purple-rain/`
- Prefix `0x02` (`https://www.`) + payload `artist.com/noubin/album/` → `https://www.artist.com/noubin/album/`
- Prefix `0x03` (`http://`) + payload `artist.com/noubin/album/` → `http://artist.com/noubin/album/`
- Prefix `0x00` (no prefix) + payload `https://artist.com/noubin/album/` → `https://artist.com/noubin/album/`
- Tag encodes `https://artist.com/noubin/album?ref=nfc` → `https://artist.com/noubin/album?ref=nfc` (query parameters kept)
- Tag encodes `https://artist.com/noubin/album#track-3` → `https://artist.com/noubin/album#track-3` (fragment kept)
- Tag encodes `https://artist.com:8443/noubin/album/` → `https://artist.com:8443/noubin/album/` (port kept)
- Tag encodes `https://user:pass@artist.com/noubin/album/` → `https://user:pass@artist.com/noubin/album/` (userinfo kept)
- Tag encodes `https://artist.com/noubin/album;v=1/` → `https://artist.com/noubin/album;v=1/` (matrix parameter kept)
- Tag encodes `https://artist.com/noubin/album?ref=nfc#track-3` → `https://artist.com/noubin/album?ref=nfc#track-3` (query and fragment both kept)
- Tag encodes `https://artist.com/noubin/purple%20rain/` → `https://artist.com/noubin/purple%20rain/` (percent-encoding in the **path** kept — not decoded to a space)
- Tag encodes `https://artist.com/noubin/album?q=hello%20world` → `https://artist.com/noubin/album?q=hello%20world` (percent-encoding in the **query** kept)
- Tag encodes `https://domain.com/noubin/artist/album/index.html` → `https://domain.com/noubin/artist/album/index.html` (path not canonicalised)
- `noubin.com` as regular linkhost (no `?redirect=`): tag encodes `https://noubin.com/noubin/artist/album/fnj82974` → `https://noubin.com/noubin/artist/album/fnj82974`
- `noubin.com` redirect wrapper: tag encodes `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum%2F` → `https://artist.com/noubin/album/` (unwrap in step 3 — here the `%3A%2F%2F` in the query **is** URL-decoded because it is the redirect target)
- `noubin.com` redirect with query on target: tag encodes `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum%3Fversion%3D2` → `https://artist.com/noubin/album?version=2`

*NDEF Text records (language code bytes discarded, text payload kept as-is):*
- Text payload `MyPlaylist01` → `MyPlaylist01`
- Text payload `  summer mix  ` → `  summer mix  ` (whitespace not trimmed at this stage)

#### 6.2 Noubin Key Normalisation

Applied to a Noubin Key string to produce a **Normalised Noubin Key** for local library matching. Used when a tag is tapped (before lookup) and during the `unite` operation (saved as `noubinKeyNormalised` in `.noudata`).

Once extracted, the player MUST apply the following transformations in exact order:

1. **Case Normalization (ASCII only):** The player MUST substitute all uppercase English alphabet characters (`A-Z`) with their lowercase equivalents (`a-z`). Non-ASCII characters (e.g. `Ö`, `É`, `ß`) MUST NOT be case-folded — leave them exactly as they are. Other characters MUST remain unchanged. (Note this rule also makes A-Z hex letters in percent-encoding lower case, so `%2F` becomes `%2f`. still means the same thing)
2. **Whitespace Trimming:** The player MUST remove any leading or trailing whitespace characters from the string.
3. **Protocol Stripping (URL Noubin Keys only):** If the Noubin Key is a URL, the player MUST remove the exact string `http://` or `https://` from the beginning of the string, if present. (Note this is not the same as ignoring the NDEF URI identifier code byte, which would also sometimes prepend `www.` — that is handled in step 5.)
4. **Authority and parameter stripping (URL Noubin Keys only):** If the Noubin Key is a URL, the player MUST reduce it to host + path, applying the following in order:
	1. Remove the fragment: delete `#` and everything after it
	2. Remove the query: delete `?` and everything after it
	3. Remove userinfo: if the authority contains `@`, delete everything from the start of the authority through the `@` (inclusive), leaving the host (and optional port) that followed it
	4. Remove a port number: if the authority has a colon-port after the host (e.g. `artist.com:8443/...`), delete that `:port`. Do NOT remove colons that appear later in the path — path segments MAY contain `:`
	5. Leave matrix parameters (`;…`) in the path unchanged at this step
5. **`www.` stripping (URL Noubin Keys only):** If the host begins with the label `www.`, remove that label and its following dot (e.g. `www.artist.com/…` → `artist.com/…`). Do not remove other subdomains (e.g. leave `shop.artist.com` as-is).
6. **Path percent-decoding (URL Noubin Keys only):** In the path only, decode each `%XX` percent-sequence (two hex digits) to the corresponding byte, **except** do not decode `%2f` (encoded `/`) — leave `%2f` as the three characters `%2f` so path segment boundaries stay stable. Apply decoding left to right. This means e.g. `%20` becomes a space, `%61` becomes `a`, and UTF-8 sequences such as `%c3%a4` become `ä`.
7. **Trailing slash (URL Noubin Keys only):** If the path is empty or does not end with `/`, append `/`. (e.g. `artist.com/noubin/album` → `artist.com/noubin/album/`; `artist.com` → `artist.com/`)

The resulting string is the **Normalised Noubin Key**. Players MUST perform a byte-for-byte equality comparison of this value against `noubinKeyNormalised` in the local media library to match to a playable item (see `03 metadata-base.md`).

- If a Normalised Noubin Key is found and linked to valid playable item see section 3.3 PLAYBACK FUNCTIONS
- If not found then return to section 3.2 NFC FUNCTIONS
- If found but playback cannot start due to e.g. invalid local media file the player MUST give an appropriate error message e.g. ".noudata file found but media not found"

**Notes on Noubin Key Normalisation:**
- Should Noubin Key normalisation remove `www.`? Yes — see step 5. Other subdomains stay.
- Should Noubin Key normalisation add a trailing slash? Yes — see step 7. Do **not** strip `index.html` or other filenames here; that is only for HTTP fetch in section 6.3. A tag ending in `…/album/index.html` therefore normalises to a different key than `…/album/`.
- Should non-ASCII letters be lowercased? No — only ASCII `A-Z` → `a-z` (step 1).
- Should `%2f` in the path be decoded to `/`? No — leave it encoded so it does not invent new path segments (step 6).
- Should Noubin Key normalisation of `noubin.com` URLS do the special unwrapping procedure if they contain a ?redirect query paramater? YES this MUST happen before Noubin Key Normalisation, otherwise after the query parameters are dumped  `noubin.com` wrapper URLs would all collapse to the same key (e.g. `noubin.com/r/`).
- Section 6.3 Noubin URL Normalisation uses additional path rules for HTTP fetch (including stripping `index.html`). Do not apply those fetch-only rules here.

**Noubin Key Normalisation Examples:**

Each example shows the **Noubin Key** (after Phase 1–2 extraction, stored as `noubinKey`) → **Normalised Noubin Key** (stored as `noubinKeyNormalised`).

- Plain text tag: `MyPlaylist01` > `myplaylist01`
- Plain text with surrounding whitespace: `  summer mix  ` > `summer mix`
- Typical artist URL: `https://artist.com/noubin/purple-rain/` > `artist.com/noubin/purple-rain/`
- `www.` stripped: `https://www.artist.com/noubin/album/` > `artist.com/noubin/album/`
- Mixed case ASCII folded, non-ASCII left as-is: `https://Artist.COM/noubin/ÖAlbum/` > `artist.com/noubin/Öalbum/`
- Query stripped + trailing slash: `https://artist.com/noubin/album?ref=nfc` > `artist.com/noubin/album/`
- Fragment stripped + trailing slash: `https://artist.com/noubin/album#track-3` > `artist.com/noubin/album/`
- Query and fragment stripped + trailing slash: `https://artist.com/noubin/album?ref=nfc#track-3` > `artist.com/noubin/album/`
- Port stripped: `https://artist.com:8443/noubin/album/` > `artist.com/noubin/album/`
- Userinfo stripped: `https://user:pass@artist.com/noubin/album/` > `artist.com/noubin/album/`
- Matrix parameter kept in path: `https://artist.com/noubin/album;v=1/` > `artist.com/noubin/album;v=1/`
- Colon in a path segment kept (not a port): `https://artist.com/noubin/album:deluxe/` > `artist.com/noubin/album:deluxe/`
- Percent-encoding decoded in path (`%20` → space): `https://artist.com/noubin/purple%20rain/` > `artist.com/noubin/purple rain/`
- Unnecessary encoding of unreserved char decoded (`%61` → `a`): `https://artist.com/noubin/%61lbum/` > `artist.com/noubin/album/`
- Encoded slash NOT decoded (segment boundary preserved): `https://artist.com/noubin/a%2Fb/` > `artist.com/noubin/a%2fb/`
- Percent-encoding only in query — query stripped: `https://artist.com/noubin/album?q=hello%20world` > `artist.com/noubin/album/`
- Trailing slash added: `https://domain.com/noubin/artist/album` > `domain.com/noubin/artist/album/`
- Empty path becomes `/`: `https://artist.com` > `artist.com/`
- Filename on path left as-is (not stripped here): `https://domain.com/noubin/artist/album/index.html` > `domain.com/noubin/artist/album/index.html/`
- `noubin.com` as regular linkhost (no `?redirect=`): `https://noubin.com/noubin/artist/album/fnj82974` > `noubin.com/noubin/artist/album/fnj82974/`
- `noubin.com` redirect wrapper — tag encodes `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum%2F` but after Phase 2 unwrap the Noubin Key is `https://artist.com/noubin/album/` > `artist.com/noubin/album/`
- `noubin.com` redirect with query on target — tag encodes `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum%3Fversion%3D2`, after unwrap Noubin Key is `https://artist.com/noubin/album?version=2` > `artist.com/noubin/album/`

#### 6.3 Noubin URL Normalisation

Applied when a web-capable player needs to fetch `web.noudata` from the internet. Input is the Noubin Key extracted from the tag (section 6.1 Phase 1–2).

The player MUST apply the following in exact order:

1. Upgrade to HTTPS: If the URL begins with `http://`, the player MUST substitute `https://`.
2. If this is a `noubin.com` redirect wrapper URL, apply the same redirect unwrapping as in 6.1 Phase 2 step 3 before continuing (so the fetch targets the artist's URL, not `noubin.com/r/`).
3. Metadata URL resolution:
	1. Remove and discard the fragment: delete `#` and everything after it (fragments are not sent in HTTP requests).
	2. Remove and store query parameters: If the URL contains query parameters (e.g. `?ref=nfc`), remove and store them.
	3. Remove userinfo and port from the authority if present (same rules as 6.2 step 4.3–4.4), leaving `https://` + host + path. Path matrix parameters (`;`) remain.
	4. Strip a leading `www.` host label if present (same rule as 6.2 step 5).
	5. Percent-decode the path as in 6.2 step 6 (decode `%XX` except leave `%2f` / `%2F` encoded). Case-fold ASCII in the URL first if not already done, or treat `%2F` and `%2f` the same when deciding not to decode.
	6. **Path Normalization:** Treat the URL as a directory path. If it does not end in a trailing slash (`/`), append one. If the final path segment contains a period (`.`) followed by an extension (e.g. `index.html`, `page.php`), strip that entire filename before appending the trailing slash.
	7. **Append `web.noudata`:** Append `web.noudata` to the path.
	8. **Re-append query parameters:** If query parameters were stored in step 3.2, append them again.

The resulting URL is used for the HTTPS GET request.

**Noubin URL Normalisation Examples:**
- `https://domain.com/noubin/artist/album` > `https://domain.com/noubin/artist/album/web.noudata`
- `https://domain.com/noubin/artist/album/` > `https://domain.com/noubin/artist/album/web.noudata`
- `https://domain.com/noubin/artist/album/fnj82974` > `https://domain.com/noubin/artist/album/fnj82974/web.noudata`
- `https://domain.com/noubin/artist/album/index.html` > `https://domain.com/noubin/artist/album/web.noudata`
- `https://www.artist.com/noubin/album/` > `https://artist.com/noubin/album/web.noudata` (`www.` stripped)
- `https://artist.com/noubin/purple%20rain/` > `https://artist.com/noubin/purple rain/web.noudata` (path percent-decoded)
- `https://domain.com/noubin/artist/album?ref=nfc` > `https://domain.com/noubin/artist/album/web.noudata?ref=nfc`
- `https://domain.com/noubin/artist/album#track-3` > `https://domain.com/noubin/artist/album/web.noudata` (fragment discarded, not re-appended)
- `https://domain.com/noubin/artist/album?ref=nfc#track-3` > `https://domain.com/noubin/artist/album/web.noudata?ref=nfc`
- `https://artist.com:8443/noubin/album/` > `https://artist.com/noubin/album/web.noudata` (port stripped for fetch)
- `https://user:pass@artist.com/noubin/album/` > `https://artist.com/noubin/album/web.noudata` (userinfo stripped for fetch)
- `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum%3Fversion%3D2` → `https://artist.com/noubin/album/web.noudata?version=2`
- `https://noubin.com/noubin/artist/album/fnj82974` > `https://noubin.com/noubin/artist/album/fnj82974/web.noudata`
- `https://noub.in/fnj82974/` > `https://noub.in/fnj82974/web.noudata`

# ADDENDUM1 DEEP LINKING AND SPECIAL REDIRECT CASE 

Why do players need special functionality to handle `noubin.com` links with `?redirect=` query parameters? 
This is a long one


- At the time of writing there is a potential issue with Software Noubin Player deep links. 
- Apps must register with operating systems the domains for which URLS should not open in a general browser but instead be redirected to the app. 
	- This is known generally as Deep Linking although each OS has it's own specific name and implementation. 
- Note this redirection behaviour is relevant only when the app does not have focus. 
	- When an app has focus than typically it is able to intercept all NFC payloads directly. (Which is how software players can play non-URL payloads linked to local media) 
- The main problem here is that apps can only deep link to domains they can prove they are associated with. 
	- Via a metadata file at the domain that authorises the app
	- Which is nominally a good security feature. For example it prevents someone making a malicious app that takes over links to a banking app
- Because the Noubin standard allows for anyone to host a Noubin URL, including artists to host at their own domain, there is no single or finite list of domains that Software Noubin Players can register deep links for. 
	- And on the other side it's unworkable that all of these domains could be updated with lists of all the valid Software Noubin Player apps
- SO for the specific function that the OS redirects noubin links to Software Noubin Players when they don't have focus here is our proposed solution:
- We solve this by having a central domain e.g. `noubin.com`
  offering a redirect service goes to the original Noubin URL domains 
	- e.g. `https://noubin.com/r/?redirect=https%3A%2F%2Fartist.com%2Fnoubin%2Falbum1%3Fversion%3D2` which will redirect to `https://artist.com/noubin/album1?version=2`
	- At `noubin.com` we maintain a single list of the Software Noubin Player app IDs 
	- Software Noubin Players can then successfully register to intercept `noubin.com` links with their OS  
- This introduces two problems: 
- A) infrastructure maintenance, including maintaining central list of approved Software Noubin Players 
	- If the standard is successful than this may still be the best path forward
	- But it involves some work, what does an 'approved player' look like, how much should we check etc? 
	- Hence why today nothing like this is yet implemented in the standard; if there is demand for this it will involve an extension to the standard and the setup of the appropriate infrastructure at `noubin.com`.
- B) The bigger problem, that we have to think about now, is that centralisation is also a potential issue with playback. If Noubins are encoded with `noubin.com` redirect links but rely on `noubin.com` to really do the redirect, then if `noubin.com` goes down all online noubin activity is halted for all Noubins, even those using linkhosts elsewhere. 
	- This is also undesirable centralisation and allows the future operators of `noubin.com` to potentially censor and remove links, deactivating online functions of Noubins people already made and bought.
	- Hence why the special requirement for noubin web activity exist: players MUST unwrap `noubin.com` redirect URLs locally as defined in sections 6.1, 6.2 and 6.3, rather than relying on `noubin.com` to perform the redirect at fetch time.
	- Then it doesn't matter if `noubin.com` is up or has removed the redirect, regular players at least will still find the artists intended content. (although it may still be broken for browsers)

# ADDENDUM2 LOCKED TAGS AND TYPE 4 TAG USE CASES

Generally a Noubin should contain a type 2 tag left rewritable (not locked). This is cheap, simple and gives maximum DIY flexibility to the end user.

However in future there is potential for artists to make limited edition Noubins that are intended to hold value as collectibles. The Noubin standard supports this on the basis that it increases the economic opportunities for artists and helps them fight clones.

This is why the standard allows use of type 4 tags. These can connect to a security chip and potentially implement an 'authentication' procedure so end users can check that a Noubin is not fake. 

This is also why the standard permits factory locking of NFC tag content as this may increase the perceived value of a Noubin as always holding the "original link".  

Tag locking may also benefit Artists selling Noubins on a larger scale (e.g. through retail channels) who are concerned about mishandling before sale. Locking them reduces the perceived risk that they were tampered with. 

On the other hand locking prevents users doing things like overwriting the Noubin URL with direct streaming platform link if they do indeed always just want the streaming platform to come up.

So when locking is used the Noubin MUST be marketed as 'locked' or 'non-rewritable' so that end users know the limitations. See `Use of the name and logo.md`
