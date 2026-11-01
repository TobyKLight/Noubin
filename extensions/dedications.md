# DEDICATIONS EXTENSION

| | |
|---|---|
| Extension name | `dedications` |
| Extension version | 1.0.0 |
| Built from core standard | v0.5.0 |

This extension lets an artist make a unique version of a Noubin URL for a specific fan, carrying one or more extra tracks that exist only online: a personal audio message from the artist to that one fan.

Example: Sam buys a Noubin with a personal dedication from Jam Master Doris. It contains an extra track titled "Dedication to Sam" containing a voice note that says "this is Jam Master Doris, thanks for buying my album Sam. It means a lot to me that fans like you keep showing up to JMD shows". Sam can play the dedication file directly when he taps the noubin on his phone, or his Noubin player treats it as an extra track. 

## Properties added

### `isDedication` (boolean)
- Optional, if omitted assume `false`
- Child of a `mediaList` item
- `true` means this media item is a personal message from the artist to the holder of this specific Noubin URL, not part of the release's general track listing
- The flag is intended to be consumed by players so they can offer modes to the user that enable or disable playback of these. (Maybe the fan doesn't want to always hear the dedication when they normally play the album)
- For ordinary items writers SHOULD omit the property rather than write `false` 
- A release MAY contain more than one flagged item (e.g. a spoken dedication plus a dedicated extra song)

Where the property sits:

```
ROOT
|-- playableItemTitle (string)
|-- mediaList (array)
     |
     |-- item (object)                       <-- ordinary release track
     |    |-- uniteData (object)
     |    |    |-- expectedFilename (string)
     |    |    |-- expectedLength (string)
     |    |    |-- freeDownloadURL (string: URL)
     |    |-- localFilePaths (array)
     |    |-- mediaTitle (string)
     |
     |-- item (object)                       <-- dedication
          |-- isDedication (boolean: ADDED BY THIS EXTENSION)
          |-- uniteData (object)
          |    |-- freeDownloadURL (string: URL: REQUIRED by this extension)
          |-- localFilePaths (array)
          |-- mediaTitle (string: the personalised title, e.g. "Dedication to Sam")
```

This extension adds no property to `releaseData` and adds nothing outside `mediaList`. A dedication exists in the metadata only once its audio exists, as an ordinary `mediaList` item with the flag set. To tell a fan that a dedication is coming before that, use the core `pendingMedia` property — see below.

No other property is added, removed or redefined. Nothing about the release's own tracks changes.

## Conformance rules

### Artists 

An artist offering dedications: 
- Artists MUST only sign up fans for dedicated Noubins with their consent. (E.g. through purchasing a release that comes with this) 
- Artists SHOULD collect fan contact details (email) that the linkhost can use to reunite the fan with 'lost' audio dedications in future (e.g. if the Noubin breaks.). This can also be used to send the user the file directly if you no longer wish to host it. 

### Linkhosts

A linkhost offering dedications:

- SHOULD put the dedicated media item last in `mediaList`, so the underlying release's track numbering is unaffected.
- MUST set `uniteData.freeDownloadURL` on the media item, because the media exists only online and is not part of any purchase.
- MUST keep the media item free download link openly downloadable by Noubin players as per any other free download: no login, no captcha. The download link must work for simple devices that cannot pass a captcha (see `02 standard.md` section 2).
- SHOULD set `mediaTitle` of the item to a personalised title, for example "Dedication to Sam". Other metadata for the item should match the rest of the release, e.g. same artist, same release title. 
- MUST mark dedication HTML pages `noindex` (e.g. `X-Robots-Tag: noindex` and a `robots` meta tag) and keep them out of sitemaps so dedicated Noubin URLs are never part of search results.
- MUST NOT run per-dedication non-anonymised analytics without the end users consent. 
- SHOULD NOT put the fan's name or contact details anywhere that is visible if you don't hold the unique URL. 
  - This applies to any pending state that is visible before the audio is published, such as the message shown for a release awaiting its media. Because `pendingMedia` is a bare boolean it carries no title, so the recommended way to promise a dedication leaks nothing. 
- SHOULD tell the artist when creating a dedication that the link is personal to the holder, that it should not be posted publicly, and that if leaked or given to others they can hear the dedication audio. 
- MUST offer opt out and deletion should they receive communication from a fan that they want the dedication removed. 
- MUST offer to the fan that they can permanently download the dedication audio file for their own archive in case the linkhost stops hosting the file in future. 
- MAY store the fan contact details (email) if the artist collects it on the basis of providing the dedicated Noubin, and can use that to provide recovery of the dedicated noubin special audio file should the original noubin somehow be lost/damaged etc. 

### Players and Media library management software

A player that knows this extension:

- MAY offer settings to play or not play `isDedication` items as part of regular playback of the release. 
- MAY download a flagged item through `uniteData.freeDownloadURL`, with the same consent and safety rules as any other direct media download (see `02 standard.md` section 3.5 WEB FUNCTIONS).
- MUST NOT count a `isDedication` item as a missing file in the `unite` completeness check (see `02 standard.md` section 4.7). A release whose dedication has not been downloaded is still "complete" for unite purposes.
- MUST preserve the `isDedication` flag when it rewrites a `.noudata` file, and SHOULD preserve the relative position of the item in `mediaList`.
- MUST NOT treat the absence of local files for a flagged item as an error condition, and MUST NOT prompt the user repeatedly to fetch it.
- SHOULD make it clear in the UI that the item is a personal dedication rather than a release track.
- The `isDedication` has no meaning in a user playlist. If a user intentionally added the dedication audio track to a playlist then it should play. 


### Implementations that do not know this extension (Graceful degradation)

A dedication is an extra `mediaList` item with a `freeDownloadURL` and no corresponding purchased file. Unaware software treats it as an extra free track: during `unite` it offers to download it, lists it after the release's tracks, and plays it. That is acceptable degradation, and it is why the item carries a download URL and why linkhosts put it last. If the user declines the download, unaware software may show the item as missing media; that is a limitation of not implementing the extension, not a malformed file.

## Combining with pendingMedia

The motivating case is a Noubin bought before a show at the merch desk. The merch sales person is saying "This more expensive one comes with a dedication from the artist, I just need your name and email" then after the show the artist records the dedication afterwards and publishes it the next day. The dedication does not exist yet when the fan walks away with the Noubin, so this extension has nothing to flag. That gap is filled by the core `pendingMedia` property (`02 standard.md` section 2.3), which tells the Noubin Player to keep rescanning the linkhost for media that has been promised but not yet published. 

## Privacy

This extension is the intended exception to the unique-links rule in `02 standard.md` section 2.2. A dedication URL is necessarily unique, because the whole point is that one holder receives something no other holder receives.

The exception is acceptable only in this narrow form:

- Artists MUST only sign up fans for dedicated Noubins with their consent. (E.g. through purchasing a release that comes with this)  
- The link is not used to build a profile of the holder. No per-dedication analytics, no tracking pixels, no combining dedication page visits with purchase or account data.
- Basic access logs that any web server keeps are still allowed, as is anonymised statistics. Dedicated Noubin URLS MUST NOT be used to report tracking data based on the unique URL without that fans explicit consent to tracking (not just consent to receiving a dedication). 
