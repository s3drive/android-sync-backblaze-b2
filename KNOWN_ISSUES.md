## Sync for Backblaze B2 known issues

Below list aims to communicate known issues with the app or the ecosystem that we're dealing with and aim to address one way or another.
At this stage this document is pretty experimental and far from being exhaustive.

If you find any issue that concerns you, please reach us at: support[at]syncaware.org or report an issue: https://github.com/photosync/android-sync-backblaze-b2/issues

### Current:

#### QR code generator shall clean-up confidential data
Currently, once you paste the B2 app keys into QR: https://web.syncaware.com/qr/, it will generate a QR code which you can scan by the app. Ideally the client-side QR generator shall clean-data after e.g. 60s, to reduce risk of someone stealing data.  

### Resolved:

#### Media files are uploaded one by one (resolved as of June 2022)
Currently, when there is a queue of files to be uploaded it is executed one by one. This issue is visible during initial folder sync, especially if there are plenty smaller files.

**Resolution:** Distributed multiple file uploads to multiple Work processes. 

####  Some uploads are not captured (resolved as of April 2022)<br>
Currently app does try to upload pictures the very first moment they're created. There are many gaps in that approach. There may be network conditions or other circumstances which make upload impossible. Another edge case is when we actually lose the "new picture event", for instance you may reboot phone and instantly shoot pictures even before upload service starts.
<br> In these cases some of the photos may not make to cloud. We're working on more robust concept called: "Media scanner", before then, please periodically use: "Preview" and "Sync" features from Home Gallery to sync any missing files to cloud.

**Resolution:** Media scanner was implemented as a single source of truth. 

### Partially resolved:

#### Videos takes long time to load when clicked (resolved as of June 2022)
When clicking video in Cloud Gallery it usually takes long time to load, depending on the size of the video and your network connection speed. This is because video needs to downloaded first, then it's played on your phone. Application doesn't yet support streaming as Backblaze doesn't support streaming natively. It may be possible to implement it client side, however we don't have clear priority on this. Until then it maybe actually better to download video (by long tap) on your phone instead of relying on preview.

**Resolution:** Old Intent media player was replaced by ExoPlayer. Even though performance it's still far of from e.g. HLS stream, it handles loading and buffering much smoother.
