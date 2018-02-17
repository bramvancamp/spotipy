
# Connecting to Spotify API

### Aim of notebook

* Connect to Spotify API using Client Credential Flow
* Explore content of the playlist object for next projects


```python
import os
```


```python
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
```

### Client Credential Flow


```python
def get_creds():
    """ Client credentials flow is appropriate for requests that do not require access to a user’s private data. 
    To support the Client Credentials Flow Spotipy provides a class "SpotifyClientCredentials".
    """
    creds = SpotifyClientCredentials(client_id = os.environ['spotipy_id'],
                                     client_secret = os.environ['spotipy_secret'])
    return creds
```


```python
# get playlists from user
creds = get_creds()
sp = spotipy.Spotify(client_credentials_manager=creds)
playlists = sp.user_playlists('bramvcamp')
print "n playlists=", len(playlists)
```

    n playlists= 7
    


```python
# explore structure of playlists 
print type(playlists)
print playlists.keys()
print playlists['items'][0].keys()
```

    <type 'dict'>
    [u'items', u'next', u'href', u'limit', u'offset', u'total', u'previous']
    [u'name', u'collaborative', u'external_urls', u'uri', u'public', u'owner', u'tracks', u'href', u'snapshot_id', u'images', u'type', u'id']
    


```python
playlist_ids = []
for elem in playlists['items'][0:3]:  # explore the first 3 playlists
    print elem['name'], "n tracks=", elem['tracks']['total']
```

    Elektronik n tracks= 6
    groevvie n tracks= 6
    Sleepy beats n tracks= 9
    
