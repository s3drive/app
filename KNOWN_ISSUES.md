## Backblaze Sync

#### Videos takes long time to load when clicked
When clicking video in Cloud Gallery it usually takes long time to load, depending on the size of the video and your network connection speed. This is because video needs to downloaded first, then it's played on your phone. Application doesn't yet support streaming as Backblaze doesn't support streaming natively. It may be possible to implement it client side, however we don't have clear priority on this. Until then it maybe actually better to download video (by long tap) on your phone instead of relying on preview.

#### Media files are uploaded one by one
Currently, when there is a queue of files to be uploaded it is executed one by one. This issue is visible during initial folder sync, especially if there are plenty smaller files. 

### Resolved recently:

####  Some uploads are not captured (resolved as of 25th April 2022)<br>
Currently app does try to upload pictures the very first moment they're created. There are many gaps in that approach. There may be network conditions or other circumstances which make upload impossible. Another edge case is when we actually lose the "new picture event", for instance you may reboot phone and instantly shoot pictures even before upload service starts.
<br> In these cases some of the photos may not make to cloud. We're working on more robust concept called: "Media scanner", before then, please periodically use: "Preview" and "Sync" features from Home Gallery to sync any missing files to cloud.