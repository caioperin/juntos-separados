*** Draft in progress ***

# juntos-separados
Conversations media - WhatsApp-compatible media used in customer-facing messages from the Conversations squad.

# Important

These files are used directly from this repository in production messages sent to Nubank customers. That is, the messages sent to customers contain URLs which directly reference these files here.

- All content here should be considered approved for public use with Nubank customers.
- Do not recorganize, move or rename files as this will break references in messages already sent to customers
- Updating a media file, replacing a file with one having the same name, automatically changes it for all messages sent after that update.

Media files delivered via Twilio to WhatsApp users are cached by Twilio and then downloaded to the mobile device by WhatsApp. If a user previously received a message referencing a media file you have updated, they will probably continue to see the previous version in WhatsApp. It is not clear at this time how Twilio caching handles an updated file - they may see the change and update their cache or continue to use their cached version. So, unless somebody can document the behavior, it is probably best to assume we can't successfully change a media file once it is in use.

## Getting a URL to use in a message

The URL to media files in this repo involve a redirect. The easiest way to create URLs is to start with the standard base URL and append the relative directory and filename. For example:

`https://raw.githubusercontent.com/nubank/juntos-separados/main/` + `images/Nubank-%C3%8Dcone-small.jpg`

begets:

https://raw.githubusercontent.com/nubank/juntos-separados/main/images/Nubank-%C3%8Dcone-small.jpg

It is possible to get a permalink URL for a file. This is a version-specific link so is impervious to changes in the file. This guarantees that even if somebody updates the file in the repository, existing messages will continue to get the original version. It seems a better plan to use the generic link so that, in the case there is a bad problem with a media file we need to fix, we can at least change the media file in the repo and that might change it for cases where the message hasn't been sent yet.

## Media tokens in messages

Basic syntax is #{0|media|url}. When we send the message, the token will be stripped from the message and the URL *attached* in the message header. The url must be a publicly accessible object.

# Media types and user experience

We have successfully tested JPEG, PNG, MP3, OGG (audio) and PDF files. In the case of images, the original text of the message is displayed below the image in WhatsApp. For audio and pdf media, the original message text is lost. It is sent to Twilio but WhatsApp appears to simply discard the message text and just render the media.

![Sreenshot 1](XXXXX_URL_XXXXX)
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
