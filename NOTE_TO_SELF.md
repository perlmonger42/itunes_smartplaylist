Notes to Myself
========================================================================

This repository is copied from `https://github.com/cvzi/itunes_smartplaylist`.
It is python3 code that can read smart playlist data from the iTunes XML export
format.

iTunes-generated XML contains the smart playlist specifications, but it's in an
opaque encoding. It's no easy matter to extract meaning from those data blocks.
(An example of the data is shown at the bottom of this file.) Fortunately, the
code in the `itunes_smartplaylist` repo is able to crack open that encoding.

This project's `utils/export.py` reads your `~/Music/iTunes/iTunes Music
Library.xml` and dumps its smart playlists in an XML-based `*.xsp` format,
suitable for an app called Kodi.  That's not useful to me, but the code also
supports exporting the info as a JSON expression tree.

By the way: for regular playlists, the output is pretty much useless -- it
creates an export file for each of them, but the content just refers to the
playlist ID, without showing the constituent tracks.


Download, Hack, and Export
------------------------------------------------------------------------

Here's how I converted my smartlists to JSON.

```bash
cd ~/tmp
git clone https://github.com/cvzi/itunes_smartplaylist.git
```

Then I lightly edited `utils/export.py`. The code for dumping as JSON (and
several other formats) is present, but commented out. So I commented out the
`*.xsp` format generator, uncommented the JSON and "query" outputs, and added
descriptive headers so you can tell where each kind of format starts. `git diff`
in this copy of the repo will show my edits.

`utils/export.py` reads the XML file named in the first commandline, but
defaults to `~/Music/iTunes/iTunes Music Library.xml`. It always writes its
output to a directory named `out`, creating it if necessary.

```bash
python3 utils/export.py
ls out
```


Example Smart Playlist Data Blocks
------------------------------------------------------------------------
```xml
  <key>Playlist ID</key><integer>51317</integer>
  <key>Playlist Persistent ID</key><string>959D3C8AE30F420C</string>
  <key>Distinguished Kind</key><integer>65</integer>
  <key>All Items</key><true/>
  <key>Name</key><string>Downloaded</string>
  <key>Smart Info</key>
  <data>
  AQEAAwAAAAIAAAAZAAAAAAAAAAcAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAA==
  </data>
  <key>Smart Criteria</key>
  <data>
  U0xzdAABAAEAAAADAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADwAAAQAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABEAAAAAAAQIbEAAAAAAAAAAAAAAAAAAAAB
  AAAAAAAQIbEAAAAAAAAAAAAAAAAAAAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA8AgAEAAAA
  AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAARAAAAAAAIIAE
  AAAAAAAAAAAAAAAAAAAAAQAAAAAAIIAEAAAAAAAAAAAAAAAAAAAAAQAAAAAAAAAAAAAAAAAA
  AAAAAAAAAAAAhQAABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
  AAAAAAAAAEQAAAAAAAAAAQAAAAAAAAAAAAAAAAAAAAEAAAAAAAAAAQAAAAAAAAAAAAAAAAAA
  AAEAAAAAAAAAAAAAAAAAAAAAAAAAAA==
  </data>
```
