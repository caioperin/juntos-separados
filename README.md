# juntos-separados

## Note: This is a public repository. All content, including this README, can be viewed by anybody.

juntos-separados hosts Conversations media - WhatsApp-compatible media used in customer-facing messages from the Conversations squad.

Additional documentation for this repository can be found in Confluence: [Sending Media in Content Messages](https://nubank.atlassian.net/wiki/spaces/NuPI/pages/262490194514/Sending+Media+in+Content+Messages)

## Important

These files are used directly from this repository in production messages sent to Nubank customers.

* All content here must be considered approved for public use with Nubank customers.
* Do not delete, move, rename or reorganize files as this will break references in existing production messages.
* Updating a media file, replacing a file with one having the same name, automatically changes it for all messages sent after that update.
* There is currently no way in Mercury to search for messages that reference these media files in a token. Always assume that any media file here is used in other messages; that deleting, moving or renaming will break those messages; and, that changing the file contents will change for all messages anywhere in our content that reference it.
