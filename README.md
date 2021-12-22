# juntos-separados
Conversations media

# Important

These files are used directly from this repository in production messages sent to Nubank customers.
That is, the messages sent to customers contain URLs which directly reference these files here.

- Do not recorganize, move or rename files
- Updating a media file automatically changes it for all messages that reference the file.

Media files delivered via Twilio to WhatsApp users are cached by Twilio. If a user previously 
received a message referencing a media file you have updated, then if they open the media file again
from the WhatsApp message, they will probably get the previous version. Everybody else opening it from
any message that references the media file, regardless of partner or project or conversation or when 
that message was or will be sent, will get the updated version of that media file.

It is possible to get a permalink URL for a file. This is a version-specific link so is impervious 
to changes in the file here in the repository. However, that makes it difficult to "fix" things if
we discover a problem with an image. Because the URL contains version information, we would have to
edit every existing message to reference the new version of the file. Given the tradeoff, using
the URL that references the latest version of the file seems a better option.

## Getting a URL to use in a message

The URL to media files in this repo involve a redirect. The easiest way to create URLs is to start 
with the standard base URL and append the relative directory and filename. For example:

`https://raw.githubusercontent.com/nubank/juntos-separados/main/` + `images/Nubank-%C3%8Dcone-small.jpg`

begets:

https://raw.githubusercontent.com/nubank/juntos-separados/main/images/Nubank-%C3%8Dcone-small.jpg
