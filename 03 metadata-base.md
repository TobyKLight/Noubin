
# NOUBIN METADATA BASE FORMAT

## DESIGN CONSIDERATIONS

A single metadata file (`.noudata`) has two main roles depending on whether it's local or online
- Local: Core function is it's a list of local media files linked to a Noubin NFC tag 
	- That list can represent an album, playlist or a list generated on the fly like all tracks belonging to an artist
	- There can be a single file in the list which is likely to be the case for audiobooks, podcasts and mixtapes
- Online: It should provide rich metadata about the release that Players can view and use to unite actual media files with once the user has purchased the release.
	- Some of this metadata is "global" as in it comes from the artist/distributor and would be the same in any media library
	- Some of this metadata is "local" as in the user sets it and represents self categorisation and preferences for only that user

Plus additional design goals:
- Indie artists can generate metadata through a web form that is not overwhelming
- Professional artists can have this data generated from industry standard formats including DDEX ERN
- Users can generate this data from imported media libraries with various embedded audio file metadata or kodi .nfo files 
	- For the latter case a two way conversion should be possible for users who wish to move their media to other player types outside the Noubin ecosystem in future. 
- The data has to be complete enough to enable creators of various kinds to express necessary information in the credits. 
- For longer form media this may involve information annotated to timestamps.

Finally there are software considerations where structuring data in a given way is going to make it simpler for players (including microcontrollers) to parse and search the data. 
- All fields should have unique names for simpler parsing logic on basic hardware (excepting fields repeated on multiple array elements). 
	- e.g. so we use `mediaTitle` instead of just `title`. Conceptually several things have titles (albums, tracks etc) so if we only reused `title` in several places it would have different meaning depending on where you are in the structure.

## SCHEMA

`.noudata` metadata files MUST be JSON format

For information about file naming and organisation see `02 standard.md` section NOUBIN MEDIA LIBRARY SPECIFICATIONS

Structure overview (selectively expanded)
```
ROOT
|-- _whatAmILookingAt (string: fixed data see below)
|-- noudataVersion (string)
|-- noubinKey (string)
|-- noubinKeyNormalised (string)
|-- playableItemTitle (string)
|-- playableItemSafeTitle (string)
|-- referToReleaseNoudata (string)
|-- releaseData (object)
|    |
|    |-- credits (object: defined below)
|    |-- coverImage (object: defined below)
|    |-- releasePlatforms (array)
|    |     |
|    |     |-- item (object)
|    |           |-- platformName (string)
|    |           |-- platformType (enum: store | DRMStore | streaming | other )
|    |           |-- platformURL (string: url)
|    |           |-- customVerb (string)
|    |-- furtherLinks (array)
|           |
|           |-- item (object)
|                |-- linkTitle (string)
|                |-- linkURL (string: url)  
|
|-- localPlaybackData (object: defined below)
|
|-- mediaList (array)
     |
     |-- item (object)
          |
          |-- uniteData (object)
          |    |-- expectedFilename (string: file name (without extension))
          |    |-- expectedLength (string: Noubin Lazy Timestamp)
          |    |-- freeDownloadURL (string: URL)
          |-- localFilePaths (array: string: relative path)
          |-- mediaTitle (string)
          |-- itemCredits (object: defined below)
          |-- itemUserNotes (object: defined below)
          |-- itemCoverImage (object: defined below)
          |-- itemLocalPlaybackData (object: defined below)
	      |-- lyricsTranscript (string)
	      |-- musicalData (object)
          |-- cueLists (array)
```

### ROOT Objects

##### `_whatAmILookingAt` (string)

- Optional
- Always a string fixed with the value "This is a noudata file from the Noubin standard. See `<Noubin standard URL>`"
- This exists because JSON can't have comments. A hint for any non-developer who opens a `noudata` file.
- Should always be the first object in the file

##### `noudataVersion` (string)
- Required
	- If omitted up to the Player if it continues or not. 
	- (If the standard turns out to be highly volatile it may be impractical to continue without this)
- a string with the version of Noubin standard this `.noudata` file was made to conform to e.g. "0.1.0"
- uses semver standard
- do not prefix with a letter, so it's "0.1.0" not "v0.1.0"

##### `noubinKey` (string)
- Optional in local `.noudata` files; MUST NOT appear in `web.noudata` files
	- if not present a Noubin can not yet be tapped to start playback for this media, it can only be played through player UI. 
- The Noubin Key extracted from the NFC tag's NDEF record (see Extracting the Noubin Key from an NDEF record in section 6 of `02 standard.md`)
	- For URI records: the full expanded URL string (e.g. `https://artist.com/noubin/album/`), 
		- not the single-byte NDEF URI prefix form
	- For Text records: the decoded text payload
- Set by the player during the `unite` operation or when the user associates a tag with a playable item — not by linkhosts in `web.noudata`
- Stored without normalisation; use `noubinKeyNormalised` for tap matching
- see `02 standard.md` section 6 NOUBIN NDEF DATA TRANSFORMS for more information

##### `noubinKeyNormalised` (string)
- Optional in local `.noudata` files; MUST NOT appear in `web.noudata` files
	- if not present, the player SHOULD derive it from `noubinKey` using Noubin Key Normalisation when indexing the library, and then save it. 
- The Normalised Noubin Key produced by applying Noubin Key Normalisation to `noubinKey` (see section 6 of `02 standard.md`)
- Used for byte-for-byte equality comparison when a tag is tapped
- Set by the player during the `unite` operation or when the user associates a tag with a playable item
- Players SHOULD index this value in the library database (e.g. a hash table) so taps can be matched without re-normalising on every read
- There can be multiple items in the library with the same `noubinKeyNormalised`
	- Ideally not but if it does occur Players should be able to cope. Can happen e,g, by accident if a user merges two libraries. 
	- Players can play the first one or offer the user to pick between the options or other behaviour as the developer wishes.

##### `playableItemTitle` (string)
- Required
- a string that is the display name of this playable item
- For a release it MUST be the name of the release (e.g. name of the album)
- For a playlist it will be the name of the playlist
- If omitted Player or media library should use release data or media file names to generate a title and set this property

##### `playableItemSafeTitle` (string)
- Optional
- If the `playableItemTitle` contains characters that are not permitted under the Safe file and folder names from `02 standard.md` then the player/media management library should do a conversion to safe characters. 
- This field should contain the result of that conversion AND this is the string used to name the metadata file in the filesystem e.g. `_<release>.noudata`
- E.g. the album is called "HE??O" 
	- A player/media software accesses `web.noudata` from the Noubin URL 
	- The `playableItemTitle` is "HE??O" which is fine for display but the Player/Media Library software detects contains illegal characters for a file path
	- Upon import to the library the Player/Media software decides how to convert "HE??O" to safe characters as permitted in the Safe file and folder names rules in `02 standard.md` 
		- Note we don't define a deterministic way to do this as the best possible way is stylistic, not deterministic. E.g.  "HE??O" should become "HELLO" but "GOODBYE?" Should become "GOODBYE". 
	- Say the result is "HEO". Not ideal but acceptable as user doesn't interact directly with the metadata file much.
	- Then `playableItemSafeTitle` is set to "HEO" and the local release `.noudata` file is named `_HEO.noudata` 
##### `referToReleaseNoudata` (string)
- Optional
- A string that contains a relative file path
- Refers to a metadata file for a release 
- Used when a user associates a Noubin with a specific file in a release. Then it needs its own `noudata` file with unique `noubinKey`, `noubinKeyNormalised`, and optionally `localPlaybackData`
- Instead of duplicating all the other metadata like `credits` `coverImage` `releaseData` etc this tells the player to look for an authoritative `_<release>.noudata` file at the location. 
- Then if the user ends up triggering playback of this file through either its own Noubin or via playing the whole album the same metadata is shown. 
- E.g. if this is `mediafile2.noudata` belonging to the album named "HE!!O" , and whose `playableItemSafeTitle` is "HEO" 
	- Then `referToReleaseNoudata` would contain the value `_HEO.noudata`
- `referToReleaseNoudata` can contain a full relative path, not ideal but can be a situation in a chaotic library where a .noudata file refers to a release file elsewhere in the library. 

The other three root objects `releaseData`, `localPlaybackData`, and `mediaList` are defined below.

---
### `releaseData` (object)

- child of root
- Optional
	- If present that means this is a release (e.g. an album, single, compilation or some other release from an artist)
	- If omitted then this is a user created playable item like a playlist

Structure
```
releaseData (object)
|
|-- credits (object: defined below)
|-- coverImage (object: defined below)
|-- releasePlatforms (array)
|     |
|     |-- item (object)
|           |-- platformName (string)
|           |-- platformType (enum: store | DRMStore | streaming | other )
|           |-- platformURL (string: url)
|           |-- customVerb (string)
|-- furtherLinks (array)
       |
       |-- item (object)
            |-- linkTitle (string)
            |-- linkURL (string: url)  

```


`credits` object defined in their own sections below
`coverImage` object defined in its own section below

##### `releasePlatforms` (array)
- optional 
- if omitted/ empty this may be a free release (free download links should be provided in the media list)
- at least one array item should exist with the `platformType` value of `store` or the release does not conform to Noubin standard 
	- although this is not an error state for players
	- Rather if this is not true linkhosts should produce a warning for artists in the user interface where they are creating Noubin URLs 
- Each element contains
	- `platformName` (string)
		 - Required 
			 - If omitted not a valid `releasePlatforms` array element and should be ignored
		 - When Noubin URLs are being created linkhosts ideally discourage typos by letting the artist paste the link for the release on the platform, and the name of the platform is determined automatically from the domain. Provide optional functionality to set this manually. 
	-  `platformType` (enum)
		- optional 
			- if omitted assume `other`
			- Expect extensions may be implemented here in future so if it's an unrecognised enum value treat as `other`
		- enum with a value of either "store" | "DRMStore" | "streaming" | "other"  
			- `store` means a store where the end user can buy the digital media to own DRM free
			- `DRMStore` means a store where the end user can buy the digital media to own only with DRM
			- `streaming` meaning a platform where the user can stream the audio files but not download or own them 
			- `other` meaning anything else
		- The player should use this setting to determine which verb to use when describing the action the user will do if they visit the link e.g. `buy <album> on <platform>` or `stream <album> on <platform>`
	- `platformURL` (string: url)
		 - Required 
			 - If omitted not a valid `releasePlatforms` array element and the entire element should be ignored
		 - The link to the release on the specific release platform
	- `customVerb` (string)
		 - Optional 
		 - if present use it to describe what should happen at the release platform link, e.g. instead of `buy <album> on <platform>` the user can `<customVerb> <album> on <platform>`
		 - Intended for use with the `other` platformType
		 - IF platformType is `other` but no customVerb is supplied use "listen to" as the verb 

##### `furtherLinks` (array)
- optional
- Each element contains 
	- `linkTitle` (string)
		- Optional
			- If omitted just show the `linkURL` as the title
		- text to be displayed for the link
	- `linkURL` (string: url)
		 - Required 
			 - If omitted not a valid `furtherLinks` array element and the entire element should be ignored
		 - The link URL provided by the artist


---
#### `credits` (object) and `itemCredits` (object)
- Optional
- `credits` is a child of `releaseData`
- AND also `itemCredits`  is a child of element of `mediaList` array 
	- When `itemCredits` exists for a specific media track, any specified properties replace the `releaseData` level `credits` property for this media file only
		- Which is to say each property inside  `itemCredits` completely replace the release level `credits` property if it exists.
		- So if included the `primaryArtists` array of the track completely replaces the `primaryArtists` array of the release. (Not just appending to it)
		- This means you can have "various artists" as the primary artist of a compilation and each individual artist team listed fully on each track. 

Structure
```
credits
|
|-- releaseProfile (enum: music | podcast | audiobook | mix | other)
|-- primaryArtists (array)
|	|-- item (object)
|		|-- primaryArtistRole (string)
|       |-- primaryArtistName (string)
|-- primaryArtistOverrideName (string)
|-- primaryArtistSafeName (string)
|-- alternateSearchKeywords (array: string)
|-- artistLocalID (string)
|-- secondaryArtists (array)
|	|-- item (object)
|		|-- secondaryArtistRole (string)
|       |-- secondaryArtistName (string)
|-- contributorsAndCrew (array)
|	|-- item (object)
|		|-- contributorRole (string)
|       |-- contributorName (string)
|-- orgs (array)
|	|-- item (object)
|		|-- orgRole (string)
|       |-- orgName (string)
|-- releaseEdition (string)
|-- seriesInfo (object)
|	|
|	|-- seriesName (string)
|	|-- seriesPartLabel (string)
|	|-- seriesPart (integer)
|	|-- seriesTotal (integer)
|-- language (string: BCP 47 format)
|-- releaseDate (string: date format ISO-8601)
|-- originalReleaseDate (string: date format ISO-8601)
|-- description (string)
|-- categories (array)
|	|-- item (object)
|		|-- categoryType (string)
|       |-- categoryName (string)
|-- legalNotice (string)
|-- identifiers (array)
|	|-- item (object)
|		|-- identifierType (string)
|       |-- identifierValue (string)
|-- additionalCreditsText (string)
```

###### Note on credits 

Why is there the role differentiation based on `releaseProfile`?

Consider the case of music vs audiobooks. Music can have a songwriter and a performer, audiobooks often have an author and a narrator. 

Semantically in terms of roles songwriter is closer to author and performer is closer to narrator. 
But when the media is playing what should go in the 'artist' display area on a player? 
In the case of music the primary artist is usually the performer, in the case of audiobooks the primary artist is usually the author. 
Finally in music there is a convention of listing collaborations with multiple artists as a single entry "Artist A feat. Artist B and C"

So the structure here is a bit unusual to both 
- facilitate lightweight parsing by potentially basic microcontroller players 
- minimise errors at the data entry stage with explicit roles. 
A media management system can be strict to ensure data is entered correctly while the microcontroller based player just always takes the string and prints it. 

All types have an open `additionalCreditsText` field where the creator can put any further information that does not fit in structured data fields. E.g. special thanks. 

Finally note that specific media items (tracks) can have more specific credits that override the general release credits. 
E.g. An album by Beyoncé has the primary artist of the album set as Beyoncé. 
But an individual track that's a collaboration might specify a higher priority `primaryArtists` list of Beyoncé and Kendrick Lamar.

Back to the schema: 

##### `releaseProfile` (enum: `music | podcast | audiobook | mix | other`)
- Optional, 
	- If omitted assume `other`
	- Expect extensions may be implemented here in future, so if it's an unrecognised enum value treat as `other`
- This is guidance towards how the credits should be filled in
	- Used to suggest roles that should be used when generating `web.noudata` metadata specific for each industry sector. 
	- Used to inform `unite` function that matches `web.noudata` to actual media files purchased from a store, as there are different industry conventions around filenames for .e.g music vs audiobooks. See `02 standard.md` section `Reference: heuristics for uniting mediaTitle and uniteData with actual media filenames`
- This is not meant to be a perfect or exhaustive list. Could theoretically include e.g. comedy, radio play, speech etc but those types of productions are not linked to their own distribution channel conventions at the moment. For example comedy recordings are often distributed through the existing music, podcast or video channels and will use file namings from those worlds.
	- Since free text entry is always possible the creator can bend existing profiles or use the `other` profile to achieve what they want. 
	- You can still make "comedy" searchable by including that as a relevant category in the `categories` section (as a tag, genre etc)
- We of course extend this in future if there is demand.

##### `primaryArtists` (array)
- optional
- each element consists of 
	- `primaryArtistRole` e.g. songwriter, performer, author
	- `primaryArtistName`
- Ordered in desired display order
- `primaryArtistRole` has specific recommended values depending on `releaseProfile`. Media management software and Noubin URL creation tools should provide strong UX guardrails so that these are used (e.g with combobox selection before free text entry is allowed) so that the right artist ends up in the right field. Free text is allowed but not recommended.
	- for `releaseProfile` = `music` the primary artist roles are `performer | composer | songwriter | producer`  
	- for `releaseProfile` = `podcast` the primary artist role is `host`
	- for `releaseProfile` = `audiobook` the primary artist role is `author`
	- for `releaseProfile` = `mix` the primary artist roles are `performer | composer | songwriter | producer | DJ`
- `primaryArtistName` should be the name the artist wants displayed on the player for this release

##### `primaryArtistOverrideName` (string)
- Optional
- If present then should replace any listing of the primary artists for display by a player. 
- E.g. instead of listing an array of primary artists with full names comma separated "Artist A, Artist B" the override string might be "Artist A feat. Artist B"

##### `primaryArtistSafeName` (string)
- Optional
- If the first `primaryArtistName` or `primaryArtistOverrideName` contains characters that are not permitted in filenames under the Safe file and folder names from `02 standard.md` then the player/media management library should do a conversion to safe characters.
- Or if the artist prefers they can set their safe name here that will be used for e.g. file paths
- e.g. if the artists name is `MYSTERY *`
- They can say their `primaryArtistSafeName` should be "MYSTERY STAR"
- If present then used as the artist name in media library paths e.g `/Music/<artist>/<release>/

##### `alternateSearchKeywords` (array: string)
- Optional
- an array of strings representing alternative names relevant for the release or artist intended to help users with searches. Particularly where the primary artist is commonly known by an acronym, their name uses special characters or this release is aimed at users using a different alphabet. 
	- E.g. `PrimaryArtistName` could be "방탄소년단" then `alternateSearchKeywords` could be "BTS", "Bulletproof Boy Scouts" etc

##### `secondaryArtists` (array)
- optional
- each element consists of 
	- `secondaryArtistRole` e.g. songwriter, performer, author
	- `secondaryArtistName`
- Ordered in desired display order
- `secondaryArtistRole` has specific recommended values depending on `releaseProfile`. Media management software and Noubin URL creation tools should provide strong UX guardrails so that these are used (e.g with combobox selection before free text entry is allowed) so that the right artist ends up in the right field. Free text is allowed but not recommended.
	- for `releaseProfile` = `music` the secondary artist roles are `performer | composer | songwriter | producer`  
	- for `releaseProfile` = `podcast` the secondary artist role is `guest`
	- for `releaseProfile` = `audiobook` the secondary artist role is `narrator`
	- for `releaseProfile` = `mix` it's recommended not to offer secondaryArtists in the UI (instead use `cueLists` to offer credits of the original songs at the time they play)

##### `contributorsAndCrew` (array)
- optional
- each element consists of 
	- `contributorRole` e.g. 
	- `contributorName`
- Ordered in desired display order
- `contributorRoles` are free text, but can be used to add any other structured data about contributor or crew roles that doesn't fit in primary or secondary artists. 

##### `orgs` (array)
- optional
- each element consists of 
	- `orgRole` e.g. publisher, imprint
	- `orgName`
- Ordered in desired display order
- ``orgRole`` has specific recommended values depending on `releaseProfile`. Media management software and Noubin URL creation tools should provide strong UX guardrails so that these are used (e.g with combobox selection before free text entry is allowed) so that the right artist ends up in the right field. Free text is allowed but not recommended.
	- for `releaseProfile` = `music` the org roles are `label | publisher | recordingStudio | masteringStudio `  
	- for `releaseProfile` = `podcast` org roles are free text
	- for `releaseProfile` = `audiobook` the org roles are `publisher | imprint | recordingStudio | masteringStudio`
	- for `releaseProfile` = `mix` it's recommended not to offer org entry by default

##### `releaseEdition` (string)
- optional
- Across music and audiobooks is the concept of editions sometimes, e.g. 'deluxe edition' '2006 reissue' etc

##### `seriesInfo` (object)
- optional
- Used when a release forms part of a larger sequence, collection, anthology, multi-disc set, book series, podcast season, or similar grouping.
- `seriesInfo` object consists of these fields
	- `seriesName` (string)
		- optional
		- Name of the series or collection.
		- E.g. "The Expanse" "Greatest Hits Box Set"
	- `seriesPartLabel` (string)
		- optional
			- if omitted should just be replaced with "part"
		- Human-readable label for the type of item within the series.
		- E.g. "Book" "Disc" "Episode" "Volume"
	- `seriesPart` (integer)
		- required
			- if omitted then this is not a valid `seriesInfo` object and the whole object should be ignored
		- if series is part X of Y < this is the X
	- `seriesTotal` (integer)
		- optional
		- if series is part X of Y < this is the Y 
		- But optional as perhaps the series is ongoing and the total isn't known
- A player should parse these as 
	- `<seriesName> <seriesPartLabel> <seriesPart> of <seriesTotal>` 
	- e.g. If all known "The Expanse Book 3 of 5"
	- if it's totally minimal just `seriesPart` then "Part 3"


##### `language` (string: BCP 47 format)
- optional
- primary language of the release
- e.g. "en" or "en-GB"

##### `releaseDate` (string: date format ISO-8601)
- optional
- date the release was released
- ISO8601 supports time as well but just put the date YYYY-MM-DD
- e.g. "2026-06-17"

##### `originalReleaseDate` (string: date format ISO-8601)
- optional
- if the release is some kind of reissue the date the original was released
- ISO8601 supports time as well but just put the date YYYY-MM-DD
- e.g. "2025-06-17"

##### `description` (string)
- optional
- a short text description to accompany the release/track
- Can always be included contextually dependent on the `releaseProfile` for how relevant this is
- expect multiline
- E.g. users browsing podcast episodes probably do value reading a short description of the episode, this is less common for music

##### `categories` (array)
- optional
- each element consists of 
	- `categoryType` e.g. genre, tag, topic or literally "category"
	- `categoryName` e.g. blues, pop, news, fantasy, comedy
- For any kind of structured categorisation relevant to this kind of release (e.g. music and audiobooks have "genres", podcasts have "topics" etc)
- Players should generally ensure this data is searchable for finding releases. 
- This array is only for structured categorisation added by the creator. User added tags are stored in `localPlaybackData` object

##### `legalNotice` (string)
- optional 
- legal text 
- expect multiline

##### `identifiers` (array)
- optional
- each element consists of 
	- `identifierType` e.g. ISBN, ISRC, ASIN, UPC
	- `identifierValue`
- Here you can put the various numbers that are meant to be attached to various kinds of media. 


##### `additionalCreditsText` (string)
- optional
- free text for the creator to put additional credits information in unstructured format
- expect multiline
- e.g. "Special thanks to Dave for letting us use his snowmobile to get to the synthesiser repair shop"


---

#### `coverImage` (object), `localCoverImage` (object) and  `itemCoverImage` (object)
- `coverImage` is child of `releaseData` and represents images set by the artist to go with the release
- `localCoverImage` is a child of `localPlaybackData` and represents images set by the user for this release or playlist. If set it overrides the `coverImage`.
- `itemCoverImage` is a child of the `mediaList` array and represents per track/item images to be shown only when that item is playing. If set it overrides both `coverImage` and `localCoverImage`. 
- All of these are optional
- See `02 standard.md` section `Supported image formats` for more information about acceptable image formats and the role of thumbnails
- Note regarding thumbnails: only the original image NOT thumbnails is linked in the metadata file.
	- The presence of thumbnails or not is defined by the actual thumbnail image file presence in the `/thumbnails` subfolder using naming conventions as defined in `02 standard.md`. Players SHOULD also check `/Thumbnails` if thumbnails are not found.
- Small design note why isn't the `coverImage` object here just the array itself? To support future expansion of this object if we add moving cover images or some other more complex definition of multiple image slideshow behaviour etc. 

Structure
```
coverImage (object)
|
|-- images (array)
    | 
    |--item (object)
        |-- imageLocalFilename (string)
        |-- imageWebURL (string: url)
		|-- isImageOfNoubin (boolean)

```


##### `images` (array)
- required
	- if omitted the `coverImage` object is not valid and should be ignored
- Ordered so that the first element is the primary cover image. All others are alternates
- Players that support displaying images are only guaranteed to display the first image. 
	- Players MAY display subsequent images e.g. in a slideshow. Or let the user manually swipe through them etc. 
- contains:
	- `imageLocalFilename` 
		- Required
			- If the player is web capable then EITHER this or `imageWebURL` is required, otherwise this element of the `images` array is not valid and should be ignored
			- If the player is NOT web capable then only this field is relevant and if omitted this element of the `images` array is not valid and should be ignored
		- The filename of the primary cover image for searching in the current release folder
		- This should be checked first and if not present web capable players and media management should then query the `imageWebURL` to find the image (after checking with the user that online access is ok).  
			- If an image is downloaded it should be placed in the library folder and the `imageLocalFilename` should be written for future. 
		- If thumbnails are available they should be in `/thumbnails` relative path off this image as per `02 standard.md`
	- `imageWebURL`
		- Required
			- either this or `imageLocalFilename` is required, otherwise this element of the `images` array is not valid and should be ignored
		- The web URL of the primaryCoverImage where it should be available for direct download
		- If thumbnails are available they should be in `/thumbnails` relative path off this URL as per `02 standard.md`
		- This property generally should not be set in the `localCoverImage` object as in principle the user would drag in images from their local file system they would never be web sources, so `imageLocalFilename` would always be set. 

##### `isImageOfNoubin` (boolean)
- optional, if omitted assume FALSE
- if present indicates this is an image of the Noubin itself
- useful for displaying to the user in a media library view of associated Noubins, the user may think of their music as attached to object X rather than the typical album cover.
- Can theoretically be multiple images of the Noubin from e.g. different angles, with/without packaging etc.   

---
### `localPlaybackData` (object)

- child of root
- Optional
	- if omitted then default local settings shall apply, defined by the player



Structure
```
`localPlaybackData` (object)
|
|-- shuffle (boolean)
|-- repeatList (boolean)
|-- stopAfterEach (boolean)
|-- resumeAtItem (integer: min 1)
|-- searchString (string)
|-- userNotes (object)
|-- localCoverImage (object)
|-- artistLocalID (string)
```


##### `shuffle` (boolean)
- optional
- if true the player should shuffle the mediaList / playlist into random order
- Generally this means that no track should be heard twice before the playlists reaches the end at least once
- It's up to the player if the playlist shuffles again when the end of the playlist is reached 
- For very long playlists on devices with limited performance it's permitted to do a true random function instead

##### `repeatList` (boolean)
- Optional
- If true at the end of the mediaList / playlist the player should load the same mediaList again

##### `stopAfterEach` (boolean)
- Optional
- If true the player should stop after each item in the mediaList / playlist and wait for the user to press play
	- Use case is structured playback situations e.g. classrooms , exercise, sound effects where the user wants to play something and then speak/act after the media has ended before they manually trigger the next item. 
- If false the player should automatically play the next item in the mediaList / playlist

##### `resumeAtItem` (integer: min 1)
- Optional
- If present playback should start at this item when this release is played
- Use case is long items that are split into multiple tracks and the user wants to resume playback rather than start from beginning (e.g. some audiobooks)
- Intended to be set at the same time as the `resumeFromTime` property in `itemLocalPlaybackData` - see that part of the schema for more information. 
- Essentially if 
	- User is currently listening to a media item 
	- Playback stops (e.g. because user pressed stop or loaded another track)
	- The Player determines that the user will want to resume playback in future for this item because e.g. it's an `audiobook` (see the `resumeFromTime` schema)
	- The player should set both the `resumeFromTime` property in `itemLocalPlaybackData` AND this `resumeAtItem` property 
	- SO that if the user plays this release again it resumes from the correct item and correct time in that item 
	- Equally ensure this value is cleared if the Player determines playback should not resume (e.g. because the user finished the book)
- 1-indexed. Meaning a value of 1 refers to the first item in the mediaList. A value of 0 is invalid. 

##### `searchString` (string)
- Optional
- If present AND the mediaList is empty AND the player supports dynamic playlists THEN
	- This .noudata file represents a dynamic playlist
	- Use this string to conduct a search of the media library and load the results as a playlist
	- Respect other `localPlaybackData` settings notably the `shuffle` property
- See Dynamic Playlists in definitions section of `02 standard.md`

`userNotes` is defined below

`localCoverImage` is defined above


##### `artistLocalID` (string)
- Optional
- String that the user can add to link an artists releases where for some reason the artists regular primary name does not match across releases. E.g. the user may wish "Prince" and "The Artist Formerly Known As Prince" to appear together under "Prince". 
- Any player SHOULD then use this internally  

---
### `userNotes` (object) and `itemUserNotes` (object)
- Optional
- `userNotes` is a child of `localPlaybackData`
- AND `itemUserNotes`  is a child of element of `mediaList` array 
	- When `itemUserNotes` exists for a specific media track, any specified properties replace the `localPlaybackData` level `userNotes` property for this media file only
		- Which is to say each property inside  `itemUserNotes` completely replace the release level `userNotes` property if it exists.
		- So if included the `userTags` array of the track completely replaces the `userTags` array of the release. (Not just appending to it)
		- This means you can have "upbeat, happy" as the user tags of a release and "sad song, breakup" as the user tags of a specific file


```
userNotes
|-- userComment (string)
|-- userRating (integer: min 0 max 5)
|-- userTags (array: string)
|-- createdByUser (string)
```

##### `userComment` (string)
- Optional
- Where the user can write a comment about this release / item

##### `userRating` (integer: min 0 max 5)
- Optional
- Where the user can rate the release from between 0 and 5 points (e.g. stars, thumbs up etc)
- Note that this value being omitted and a rating of 0 mean different things
	- If it's omitted that means the release / item is not yet rated
	- If it's 0 it means the user really doesn't like this release / item 

##### `userTags` (array: string)
- Optional
- Where the user can add their own tags about this release / item

##### `createdByUser` (string)
- Optional
- To say which user created this `.noudata` file in the local library. Intended to be used to say who created a playlist. Could also be used for audio notes or some other user created content. 
	- This is different to an artist creating a release or a DJ creating a mixtape these both have `releaseData` with `primaryArtists` data
	- Why treat these differently? I don't mean to throw shade on the users curation here but it's a decision about the metadata management, because a user can create a playlist consisting of DJ mixes. SO when the player plays this playlist it should show the artist as each DJ who made the current mix. The author of the playlist itself is a separate property. 
	- The user didn't have anything to do with releasing the music publicly so therefore the  users name shouldn't be in the `primaryArtists` property
	- UNLESS the user does decide to release their playlist publicly as a mixtape (also typically mixing it together and making one file). Then the user becomes an artist in the context of the noubin standard and makes a public release with a `web.noudata` file which contains a `releaseData` object.

---
### `mediaList` (array)

- child of root
- required
	- must have at least one entry
	- if omitted or invalid then not a valid `.noudata` file; produce warnings that it will be ignored
	- EXCEPTION if the player supports dynamic playlists and the `searchString` property is present in `localPlaybackData` then this is a dynamic playlist; the player should perform a search to fill the `mediaList`


Structure overview (selectively expanded)
```
mediaList (array)
|
|-- item (object)
	|
	|-- uniteData (object)
	|    |-- expectedFilename (string: file name (without extension))
	|    |-- expectedLength (string: Noubin Timestamp HH:MM:SS.mmm)
	|    |-- freeDownloadURL (string: URL)
	|-- localFilePaths (array: string: relative path)
	|-- mediaTitle (string)
	|-- itemCredits (object: defined above)
	|-- itemUserNotes (object: defined above)
	|-- itemCoverImage (object: defined above)
	|-- itemLocalPlaybackData (object: defined below)
	|-- lyricsTranscript (string)
	|-- musicalData (object)
	|     |
	|     |-- BPM (decimal)
	|     |-- musicalKey (String)
	|-- cueLists (array)
```


##### `uniteData` (object)
- Is this required?   
	- Full story: The purpose of `uniteData` is to enable the `unite` operation, that is
		- A player/media management software reads a Noubin URL and obtains the `web.noudata` metadata file about a release
		- The user then chooses a `releasePlatform` link to purchase and download the actual media from their choice of store
		- then the player/media management software unites those media files with the original metadata to create a `_<release>.noudata`
		- so the role of `uniteData` is to contain the expected information about a media item needed for the `unite` operation (Plus also `mediaTitle` from the level above)
		- OR if it's a free release `uniteData` contains the URL where the media file can be directly downloaded
			- For more about this see `02 standard.md` section UNIFYING MEDIA FILES WITH WEB RELEASE DATA FOR INCLUSION IN A LOCAL MEDIA LIBRARY 
	- Answer:
		- `uniteData` is required in `web.noudata` or any `noudata` file that a player/media management app is going to try and `unite` 
		- `uniteData` is not required once a `.noudata` file has accurate `localFilePaths` values for the respective item 
- `uniteData` contains three properties, only one is required for this to be a valid `uniteData` 
	- `expectedFilename` (string: file name (without extension))
		- The filename  of the expected associated media file. 
		- Without extension
	- `expectedLength`  (string: Noubin Timestamp HH:MM:SS.mmm)
		- The expected length of the media file as a Noubin Timestamp
	- `freeDownloadURL` (string: URL)
		- A URL where the player / media management software can download the media file. 
		- It should be a suitable download link for hardware players to use which have no way to e.g. do captchas.  
- If it's a paid release then Noubin URL creators SHOULD set both `expectedFilename` and `expectedLength`. They MUST set at least one.
- If it's a free release then Noubin URL creators MUST set `freeDownloadURL`
- Note that `expectedLength` should not be used as the authoritative length value once `localFilePaths` entries have been set. From then on the actual track length should be determined from the real media file.

##### `localFilePaths` (array: string: relative path)
- optional 
	- But if not present this media item can't be played, a `unite` operation must be performed first.  (See `uniteData` above)
- an array of relative paths to the media files
	- Note all files are intended to be the SAME MEDIA, this is for e.g. when a download came with files in multiple qualities/file types (E.g. MP3 and FLAC)
	- Normally the Player selects the highest quality file it can play (see Playback section of `02 standard.md`)
	- Unless the user overrode that see `overridePlaySpecificAlternate` property of `itemLocalPlaybackData` 
- entries should include extension
- e.g. "01 - Sweet Intro.mp3"
- For a playlist this can be relative path out of the folder e.g. "../Music/Artist/Album/01 - Sweet Intro.mp3"




##### `mediaTitle` (string)
- Is it required? 
	- Optional for `.noudata` files with a single item in the `mediaList`, then if omitted players should display the `playableItemTitle` (e.g. the release title.) as the title. 
	- Optional if this is a playlist. On a playlist the `localFilePaths` ideally refers to a media file that already has an authoritative `.noudata` file associated with it elsewhere in the library (that has been loaded into the players database). The player should take the `mediaTitle` from the authoritative data first. Fallback is to take it from the playlist `mediaTitle`  
	- `mediaTitle` is required for all other `.noudata` files with multiple media items 
- It's the title of the song/track/chapter etc that should display in the player
	- Note the above rules mean that for audiobooks and podcasts which have a single file they probably don't have a `mediaTitle` and the player falls back to showing the `playableItemTitle` (e.g name of the book or podcast episode) 
- Note also some files may have both a `mediaTitle` AND timestamped chapter names etc which Players should also display. See `02 standard.md` section `REFERENCE COMBINING TITLE INFORMATION`

The objects `itemCredits`,  `itemUserNotes` and `itemCoverImage` are defined above. 


##### `itemLocalPlaybackData` (object)
- optional
- contains user preferences for playback of this specific media item
- contains the following properties
	- `repeatItem` (boolean)
		- Optional, if omitted assume false
		- If true then when this item finishes load it for playback again 
	- `startFrom` (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
		- Optional 
		- If included this is the time that the media item should start playback from when 'starting from the start' (not resuming)
		- Usecase is user wishes to trim silence or introduction off the start of a track
		- Invalid if timestamp is greater than the media item length
	- `resumeFromTime` (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
		- Optional
		- If present it means the media item should resume playback from this time when next played back
		- Intended usecase is resuming audiobooks, podcasts, mixes and other long media
		- Recommended UX is only set this on items when `releaseProfile` = `podcast`, `audiobook` or `mix` AND longer than X minutes. Let the user change these preferences.
		- Ensure also that Players clear it when user reaches end of an item. 
		- Invalid if timestamp is greater than the media item length
	- `overrideResumeBehaviour` (enum: default | neverResume | alwaysResume) 
		- Optional, if omitted treat as `default`
		- A setting from the user saying this specific item should either never have resume behaviour or always have resume behaviour
		- Usecase is items that weren't categorised properly and not captured appropriately in the resume function defaults.
			- e.g. a user has a comedy album which was released on a music storefront as an album with multiple tracks even though actual content is long standup comedy items. The user does actually want this release to resume playback when they return to it, even though they don't normally want music to resume playback. 
	- `useSpecificAlternate` (integer: min 1) 
		- Optional, if omitted Player chooses which media file to play (see Playback section of `02 standard.md`)
		- If present this is the index the user has chosen of the media file alternate that should always be played. 
		- This is 1-INDEXED so 1 is the first item in the array. 0 is invalid. 

##### `lyricsTranscript` (string)
- optional
- text of the lyrics (of music) or transcript (of spoken media)
- expect multiline
- If available, dynamic lyrics/transcript are preferred as part of a `cueLists` entry with `cueListType` = `text`. 

##### `musicalData` (object)
- Optional
- Can be used for search and to capture information by people who track this information for DJ or Karaoke purposes.  
- Should not be treated as authoritative, just an indicator.  These things can change in some songs. 
- Contains two properties
	- `BPM` a decimal number indicating the beats per minute
		- Optional
	- `musicalKey` a string.
		- Optional
		- When `musicalKey` is included then for typical western musical keys SHOULD be  formatted like so
			- "C" = C major
			- "Cm" = C minor
			- "F#" = F sharp major
			- "F#m" = F sharp minor
			- "Eb" = E flat major
			- "Ebm" = E flat minor
		- Note the use of hash # symbol and lowercase b from standard US keyboard character set, NOT fancy unicode U+266F or U+266D
- Note this is not enough information for looping or auto-mixing, doesn't indicate e.g. where the first bar starts or if BPM changes over the song, time signature etc. This should be covered by extensions to the standard if there is interest. 

---
##### `cueLists` (array)
- optional
- A list of lists
	- Each of those inner lists is a list of cues 
	- A cue is a block of time bounded by a `startTime` and `endTime`
- Why have multiple cueLists? Because there are multiple kinds of time based annotations you may want to sync to a media file E.g.
	- Lyrics / transcript in multiple languages
	- Chapter / navigation markers 
	- Guests for podcasts
	- Track listing of the tracks in a mix or live set. 
- The other reason to have multiple lists is to allow overlapping events to happen, whilst enforcing a nice invariant that any single list shouldn't have overlapping events.
	- Which then saves you always having to enter an `endTime`; when it's omitted a player MUST assume the next `startTime` is also the end of the previous block. 

Structure
```
cueLists (array)
   |
   |-- item (object)
		|-- cueListType  (Enum: nav | credits | text | other)
		|-- cueListTitle (string)
		|-- cueListLanguage (string: BCP 47 format)
		|-- cueListNavigable (boolean)
		|-- cues (array)
			| 
			|-- startTime (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
			|-- endTime (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
			|-- cueTextSpans (array)
			|    |
			|    |-- item (object)
			|		|
			|		|- text (string) 
			|
			|-- cueURL (string)
```


##### `cueListType` (Enum: nav | credits | text | other)
- Optional
	- If omitted treat as type `other`
	- Expect extensions may be implemented here in future so if it's an unrecognised enum value treat as `other`
- Used to set how the cue data should be interpreted 
	- `nav` = navigation markers, e.g. chapter markers in an audio book.
	- `credits` = information about content inside the track e.g. guest in part of a podcast, track playing inside a mix. Also navigable by default. 
	- `text` = text of musical content (lyrics) and non musical content (script/transcript).
		- Players can use `releaseProfile` from the credits object to display the option as `show lyrics` (for music) or `show transcript` (for podcast, audiobook etc)
- Note how `nav` and `credits` are kind of similar, why not combine them? Because a track could have both or even multiple cue lists of these types. Like main nav markers AND separate overlapping credits. 

##### `cueListTitle` (string)
- Optional
- For letting the user differentiate meaningfully between various cue lists accompanying the media.
- Generally only include if 
	- you have multiple versions of a single `cueListType` that are NOT already differentiated by `cueListLanguage` 
	- or `cueListType` = "other"
##### `cueListLanguage` (string: BCP 47 format)
- Optional
- If there are multiple cue lists in different languages then this property should be used to differentiate them 
- Structured format allows the player to automatically select preferred default language

##### `cueListNavigable` (boolean)
- Optional
	- If omitted and `cueListType` is `nav` or `credits` then this defaults to TRUE
		- If `cueListType` is anything else this defaults to FALSE
- Sets if the cue list is navigable and prev/next nav buttons (probably prev/next track on most players) should jump the playhead time through this list
- Should be set appropriately when `cueListType` = "other"
- Note if navigation is enabled on multiple active cueLists and the user presses 'next nav' the players SHOULD jump the user to the next navigable `startTime` on any track.
- Can be used as an override to e.g. disable navigation on `credits` tracks if there is already a `nav` track and you don't think the user will want to also 
- Or can force enable it to navigate through a `transcript` for some esoteric usecase like quick scrubbing through spoken text in a court transcript or something. Or jumping back to the line you want to sing over and over again in karaoke. 

##### `cues` (array)
- Required with at least one valid element for this cuelist to be valid. 
- has these properties
	- `startTime` (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
		- Required for this to be a valid `cues` array element
		- The time counting from the start of the media file that this cue starts
		- See `02 Standard.md` for more info about the timestamp format
		- This `cues` element is invalid if `startTime` is 
		  - greater than the media item length 
		  - greater than the `endTime` for this `cues` element
		  - if `endTime` was specified for the previous element : then this `startTime` cannot be less then the previous `endTime` value
	- `endTime` (string: Noubin Lazy Timestamp HH:MM:SS.mmm)
		- Optional, if omitted assume the next `startTime` or the end of the track is the `endTime` for this cue
		- Time counted from the start of this media file until this cue end
		- See `02 Standard.md` for more info about the timestamp format
		- This `cues` element is invalid if `endTime` is greater than the media item length 
	- `cueTextSpans` (array)
		- Required for this to be a valid `cues` array element
			- Must have at least one element to be valid.
		- Contains an array of the text to be shown to the user during this cue
		- The base functionality is Players generate the text for display by concatenating each `text` element of the `cueTextSpans` array without spaces.
		- Why use an array here? 
			- To support extensions in future that add extra **per word** metadata, like karaoke highlighting or different colours for who is speaking etc.  
			- We are supporting it as an array in the base standard now so imports from common open subtitle and transcript formats can retain per work markup data for future use.
		- If you don't have per word data then simply set only one element in this array with the whole text string
		- Each element of the array contains a `text` (string)
			- a UTF-8 string
			- Depending on the capabilities of the screen players SHOULD implement appropriate fonts and rendering functions to get a good level of accessibility across multiple languages.
			- However we recognise asking microcontroller based players and other modest hardware to support all of unicode is just not realistic. 
			- Players MUST NOT crash or produce errors on unsupported characters. The plain square missing character glyph (□ aka tofu) is the preferred fallback. 
			- MAY contain white space
			- MAY contain line ending / new line characters. Common in subtitle formats e.g.
				- "- are you ready?
				  -I was born ready"
	- `cueURL` (string)
		- Optional
		- Web enabled players MAY support visiting this URL or giving it to the user via NFC to check on their own device. 
		- Usecases are e.g. linking back to the original track playing as part of a mix, or letting user visit a website that hosts are discussing on a podcast
		- Users should always consent and see the domain they are navigating to before a web page is accessed.
		- It is permissible on a hardware player to set the NFC transceiver into tag emulation mode and prepare it with the link and give the user the consent choice by prompting them to tap the phone now if they want to visit the link. 



