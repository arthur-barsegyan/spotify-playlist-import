# Spotify Playlist Import

A small python script to create a Spotify playlist from a m3u playlist file or iTunes XML file.

It will:

  - Read each entry in the playlist file
  - Read the IDv3 tags from each MP3 file
  - If there are no IDv3 tags it will attempt guess the artist and title from the file name
  - Use this data to find a track on Spotify
  - Create a Spotify playlist using the results

## Installation and requirements

Install python modules:

```
pip install -r requirements.txt
```

Take 5 mins to register an app to get access to the Spotify API:

https://developer.spotify.com/my-applications/#!/

The Redirect URI doesn't need to be valid, it can be a non-existant domain. (Usually it's "http://localhost")

Export Spotify related environment variables from your new app:

```
export SPOTIPY_CLIENT_ID='your-spotify-client-id'
export SPOTIPY_CLIENT_SECRET='your-spotify-client-secret'
export SPOTIPY_REDIRECT_URI='your-app-redirect-url'
```


## Migration from Apple Music into Spotify

In order to export your entire library from Apple Music, you must:

1. Open iTunes
2. Select "File" menu and select "Library" -> "Export library"
3. Save this XML file in any folder
4. Start this script and specify the path to this file like:

```
$ ./spotify-playlist-import.py -f <path_to_your_xml_library> -u <spotify_username>
``` 

## Example

```
$ ./spotify-playlist-import.py --help
usage: spotify-playlist-import.py [-h] -f FILE -u USERNAME [-d]

A script to import a m3u playlist into Spotify

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  Path to m3u playlist file
  -u USERNAME, --username USERNAME
                        Spotify username
  -d, --debug           Debug mode
$ 
$ ./spotify-playlist-import.py -f my_playlist.m3u -u my_username
Parsed 3 tracks from my_playlist.m3u

tracks/inspectah deck - the movement - 12 - vendetta.mp3
IDv3 tag data: Inspectah Deck - Vendetta
Guess from filename: Not required
Spotify: Inspectah Deck - Vendetta, 23GoX2Usy1Ios5zCVRIIAO

tracks/darude-sandstorm.mp3
IDv3 tag data: None
Guess from filename: darude - sandstorm
Spotify: Darude - Sandstorm - Extended, 7ikiyBfgcVuAKAwZXXkWVT

tracks/dave spoon - at night (shadow child & t. williams re-vibe).mp3
IDv3 tag data: None
Guess from filename: dave spoon - at night (shadow child & t. williams re
Spotify: Dave Spoon - At Night - Shadow Child & T. Williams Re-vibe, 1JEA273o693GwuI39gayHk

3/3 of tracks matched on Spotify, creating playlist "my_playlist.m3u" on Spotify... done
```

## Problems

If you have Python ImportError exception with message: "raise ImportError('failed to find libmagic.  Check your installation')" read next. libmagic library may requires additional dependencies on your environment.

For Mac users it can be resolved like (thx https://gist.github.com/eparreno/1845561):
```
brew install libmagic
brew link libmagic
sudo env ARCHFLAGS="-arch x86_64" gem install ruby-filemagic -- --with-magic-include=/usr/local/include --with-magic-lib=/usr/local/lib/
```