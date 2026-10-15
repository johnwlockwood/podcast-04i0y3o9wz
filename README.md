Output to Outcome Book Club: static podcast feed. Only the feed lives here (feed.xml, episodes.json, cover.jpg);
episode audio is served from the same Cloudflare R2 bucket as John's Deep Dives (`podcast-ca307yc2hh`),
configured in `podcast.json` under `storage`. Never commit audio.

Normal flow: upload once via the main feed (`publish_episode.py add` in podcast-ca307yc2hh), then list the same
object here without re-uploading (no R2 credentials needed; it reads size via HTTP and duration via ffprobe):

    ./publish_episode.py add-remote --file NAME.mp3 --title "Title" --description "Text" [--date ISO8601]

**Shared audio:** `podcast.json` sets `"shared_storage": true`, so `remove FILE|SLUG|GUID` here only drops the
feed entry and never deletes the R2 object (the main feed uses it too). Pass `--delete-file` only if you really
mean to delete the audio from storage.

Other commands: `add` (uploads a new MP3; needs `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_ACCOUNT_ID`
and boto3), `migrate`, `build` (rebuild feed only).
