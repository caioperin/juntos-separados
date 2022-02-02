*** Draft in progress ***

# juntos-separados
Conversations media - WhatsApp-compatible media used in customer-facing messages from the Conversations squad.

# Important

These files are used directly from this repository in production messages sent to Nubank customers.

* All content here must be considered approved for public use with Nubank customers.
* Do not delete, move, rename or reorganize files as this will break references in existing production messages.
* Updating a media file, replacing a file with one having the same name, automatically changes it for all messages sent after that update.
* There is curerntly no way in Mercury to search for messages that reference these media files in a token. Always assume that any media file here is used in other messages; that deleting, moving or renaming will break those messages; and, that changing the file contents will change for all messages anywhere in our content that reference it.

## How it works
1. A media file is placed in the appropriate folder in this GitHub repository.
2. A content message includes an image token providing the url to the file here in this GitHub repository. *Detail: messages with audio content should not include any message text. See WhatsApp rendering below.*
3. The message is sent to a customer through the Mercury conversation platform using the Twilio messenger configured for WhatsApp.
4. Mercury removes the token from the mssage body and sends the message body plus the media details to Twilio.
5. Twilio retrieves the media file from this GitHub repository and stores it on a Twilio server with a different filename.
6. Twilio uses the WhatsApp Business API to send the message to the end user's WhatsApp application.

### Supported media types
For sending media we only support images and audio files and they must have a supported file extension
* Image: jpg, jpeg, png
* Audio: mp3, ogg, amr

The combined size of the message and the included media file must not exceed 16MB.

*Detail: An OGG file is a special media container format. OGG files may end up with different file extensions, including .opus used in our test file. Google "OGG" to learn more.

*Detail: AMR (Adaptive Multi-Rate encoding) is a compressed audio file format. It is reportedly much more efficient than an mp3 file so if the 16MB limit becomes a problem you might investigate the AMR format.

*Detail: It could be possible to support video files, i.e. mp4 files, but due to an incompatability between the way GitHub and Twilio work, the video files would need to be staged on a different server than GitHub. When Twilio makes an HTTP request to retrieve the media file, it expects the HTTP response header to give the content type matching the file extension, so *.mp4 should give content-type video/mp4; but, GitHub responds with content-type set to application/octet-stream. Twilio rejects this as a media type mismatch.*

### WhatsApp rendering

The way WhatsApp renders a message with media depends on the media type.
* image: WhatsApp renders the message with the image at the top and the message text below it.
* audio: WhatsApp renders an audio player widget with a play button. Any text in the message body is stripped and not displayed.

* Media files delivered via Twilio to WhatsApp users are cached by Twilio and then downloaded to the mobile device by WhatsApp. If a user previously received a message referencing a media file you have updated, they will probably continue to see the previous version in WhatsApp. It is not clear at this time how Twilio caching handles an updated file - they may see the change and update their cache or continue to use their cached version. So, unless somebody can document the behavior, it is probably best to assume we can't successfully change a media file once it is in use.

## Getting a URL to use in a message

The obvious ways of getting a URL to files here begets a URL using content type HTML and returning a redirect. This can't work. The easiest way to create URLs is to start with the standard base raw-content URL and append the relative directory and filename. For example:

base url: `https://raw.githubusercontent.com/nubank/juntos-separados/main/`
your file: `images/an_image.jpg`
full URL: #{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/images/an_image.jpg}

*Detail: It is possible to get a permalink URL for a file. This is a version-specific link so is impervious to changes in the file. This guarantees that even if somebody updates the file in the repository, existing messages will continue to get the original version. It seems a better plan to use the generic link so that, in the case there is a bad problem with a media file we need to fix, we can at least change the media file in the repo and that new media will be used in all content messages that reference it thereafter.

## Media tokens in messages

The basic syntax is #{0|image|url}. This same token works regardless of the media type. For example, these were used when testing:

#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/images/Nubank-%C3%8Dcone-small.jpg}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/images/Nubank-%C3%8Dcone-small.png}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/audio/file_example_MP3_700KB.mp3}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/audio/gs-16b-1c-44100hz.opus}
#{0|image|https://raw.githubusercontent.com/nubank/juntos-separados/main/test_files/docs/SamplePDF.pdf}


# Media types and user experience

We have successfully tested JPEG, PNG, MP3, OGG (audio) and PDF files. In the case of images, the original text of the message is displayed below the image in WhatsApp. For audio and pdf media, the original message text is lost. It is sent to Twilio but WhatsApp appears to simply discard the message text and just render the media.

![Sreenshot 1](https://github.com/nubank/juntos-separados/blob/main/test_files/screenshots/Screenshot_1.jpeg)
![Sreenshot 2](XXXXX_URL_XXXXX)
![Sreenshot 3](XXXXX_URL_XXXXX)

There are a number of other file formats that are documented as being supported. We tested with an MP4 file, but it was never delivered to the customer. The Twilio dashboard indicates they received the message and it was pending delivery, but it never got beyond that step.

## Media restrictions & limitations

- Meday cannot be included in template files; Whats app simply does not support this.
- Image files cannot have a transparent layer or the image may not render properly on the users device
- ...whatsapp/twilio pages on restrictions

## Special notes

In addition to embedding media in a message, it also works to simply include a URL in the text of a message. In this case we do not use token replacement, but just include the URL in the text of the message.  As seen here, WhatsApp detects the URL and handles such messages in a special way, including sort of a preview of what the URL references.

![Message including a link to the Nubank public blog](XXXXX_URL_XXXXX)

## References
