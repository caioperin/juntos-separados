# juntos-separados
Conversations media - WhatsApp-compatible media used in customer-facing messages from the Conversations squad.

# Important

These files are used directly from this repository in production messages sent to Nubank customers.

* All content here must be considered approved for public use with Nubank customers.
* Do not delete, move, rename or reorganize files as this will break references in existing production messages.
* Updating a media file, replacing a file with one having the same name, automatically changes it for all messages sent after that update.
* There is curerntly no way in Mercury to search for messages that reference these media files in a token. Always assume that any media file here is used in other messages; that deleting, moving or renaming will break those messages; and, that changing the file contents will change for all messages anywhere in our content that reference it.

## Supported media
Media cannot be included in WhatsApp **template messages**. A message with media cannot start a session (the 24-hour follow-on block where messages don't have to be approved templates. Messages with media sent where an approved template is required will be rejected by WhatsApp the same as any non-approved template message in the same scenario.

For sending media we only support images and audio files and they must have a supported file extension
* Image: jpg, jpeg, png
* Audio: mp3, ogg, amr
* ~~Video: mp4~~
* Document: pdf

The combined size of the message and the included media file **must not exceed 16MB**.

*Detail: AMR (Adaptive Multi-Rate encoding) is a compressed audio file format. It is reportedly much more efficient than an mp3 file so if the 16MB limit becomes a problem you might investigate the AMR format.*

*Detail: OGG is only supported when using the opus codec, in which case the file will have an opus extension.

*Detail: It should be possible to send mp4 video files but this does not work due to an incompatibility between GitHub and Twilio. To send mp4 files, the video files would need to be staged on a different server than GitHub. (tldr; When Twilio makes an HTTP request to retrieve the media file, it expects the HTTP response header to give the content type matching the file extension, so *.mp4 should give content-type video/mp4; but, GitHub responds with content-type set to application/octet-stream. Twilio rejects this as a media type mismatch.)*

## How it works
1. A media file is placed in the appropriate folder in this GitHub repository.
2. A content message includes an image token providing the url to the file here in this GitHub repository. *Detail: messages with audio content should not include any message text. See WhatsApp rendering below.*
3. The message is sent to a customer through the Mercury conversation platform using the Twilio messenger configured for WhatsApp.
4. Mercury removes the token from the mssage body and sends the message body plus the media details to Twilio.
5. Twilio retrieves the media file from this GitHub repository and stores it on a Twilio server with a different filename.
6. Twilio uses the WhatsApp Business API to send the message to the end user's WhatsApp application. This message includes the URL to the media on the Twilio server.
7. WhatsApp receives the message then retrieves the associated media from the Twilio server renders the message.

### WhatsApp rendering

The way WhatsApp renders a message with media depends on the media type. 
* image: WhatsApp renders the message with the image at the top and the message text below it.
* audio: WhatsApp renders an audio player widget with a play button. **Any text in the message body is stripped and not displayed**.
* video: *unknown, probably similar to rendering audio*
* pdf: WhatsApp renders a snapshot preview of the PDF file and a link to the file. User can open the full PDF by tapping. As with audio content, any message text will be discarded by WhatsApp.

Some sample screenshots from testing are at the bottom of this readme.

## Adding a token to a content message

The basic syntax is #{0|image|url}. This same token works regardless of the media type.

For example, these tokens for various media types were successfully used in testing:
```
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/images/Nubank-%C3%8Dcone-small.jpg}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/images/Nubank-%C3%8Dcone-small.png}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/audio/file_example_MP3_700KB.mp3}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/audio/gs-16b-1c-44100hz.opus}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/docs/SamplePDF.pdf}
```
As pointed out in the section on rendering, any text in the body of a message that sends an audio file will be discarded by WhatsApp and the end user will never see it.

## Getting a URL to use in a message

The obvious ways of getting a URL to files on GitHub produce a URL that will not work. The easiest way to create usable URLs is to start with the standard base raw-content URL and append the relative directory and filename. For example:

base url: `https://raw.githubusercontent.com/nubank/juntos-separados/main/`
your file: `images/an_image.jpg`
full URL: `#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/images/an_image.jpg}`

*Detail: It is possible to get a permalink URL for a file. This is a version-specific link so is impervious to changes in the file. This guarantees that even if somebody updates the file in the repository, existing messages will continue to get the original version. It seems a better plan to use the generic link so that, in the case there is a bad problem with a media file we need to fix, we can at least change the media file in the repo and that new media will be used in all content messages that reference it thereafter.

## Special considerations

* Given the multiple places a message with media can fail, or render in WhatsApp in an undesireable way, it is strongly recommened that all media content be tested end-to-end, from Mercury through Twilio and WhatsApp, to a WhatsApp application.
* Regardless of where you put the token in the message, it will be removed and *attached* to the message. If you are embedding the token in a message with text, format the text as if the token isn't there.
* Messages that send audio content will not show the message text in WhatsApp. The customer will only get the audio player.
* Media cannot be included in WhatsApp template messages.
* Image files should not have a transparent layer or the image may not render properly on the users device.

## Special note on embedding simple URLs in the body of a message

In addition to embedding media in a message, it also works to simply include a URL in the text of a message. In this case we do not use token replacement, but just include the URL in the text of the message.  As seen in the sample below, WhatsApp detects the URL and handles such messages in a special way, including sort of a preview of what the URL references. The user can tap the URL to open the web site.

# References
* [Guidance on WhatsApp Media Messages | Twilio](https://www.twilio.com/docs/whatsapp/guidance-whatsapp-media-messages)
* [Sending and Receiving Media with WhatsApp Messaging on Twilio – Twilio Support](https://support.twilio.com/hc/en-us/articles/360017961894-Sending-and-Receiving-Media-with-WhatsApp-Messaging-on-Twilio)

# Sample screen shots

## Message with image
![Sreenshot 2](/test_files/screenshots/Screenshot_2.jpg)


## Message with audio
![Sreenshot 3](/test_files/screenshots/Screenshot_3.jpg)


## Message with PDF
![Sreenshot 4](/test_files/screenshots/Screenshot_4.jpg)


## Message with simple URL (no Mercury token)
![Sreenshot 1](/test_files/screenshots/Screenshot_1.jpeg)

