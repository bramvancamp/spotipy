
# Connecting to Spotify API

### Aim of notebook

1) Connect to Spotify API using two different authentication methods:
- Client Credential Flow
- Authorization Code Flow

2) Show difference in nr playlists found for user when using different authentication methods (as not all playlists are public)

3) Explore content of the playlist object for next projects


```python
import os
```


```python
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials
import spotipy.util as util
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
# connect
creds = get_creds()
sp = spotipy.Spotify(client_credentials_manager=creds)
```


```python
# get playlists from some user (me in this case)
playlists = sp.user_playlists('bramvcamp')
```


```python
# count nr playlists (public)
print "n playlists=", len(playlists['items'])
```

    n playlists= 37
    


```python
# show first 3 playlists, show count n tracks
playlist_ids = []
for elem in playlists['items'][0:3]:  # explore the first 3 playlists
    print elem['name'], "n tracks=", elem['tracks']['total'] 
```

    Elektronik n tracks= 6
    Sleepy beats n tracks= 9
    2000s trash can n tracks= 14
    

### Authorization Code Flow


```python
def get_token(scope=None):
    '''
    To support the Authorization Code Flow Spotipy provides a utility method "util".
    This creates a token while allowing the user to set the scope of using the api.
    Info on scope: https://developer.spotify.com/web-api/using-scopes/
    
    If credentials are stored as local variables the function will fetch these. For windows: https://tinyurl.com/y78qpmn8
    If not stored locally, the user has to enter the credentials manually.
    
    '''        
    redirect_uri = 'http://example.com/callback/'
    
    try:
        try:
            username = os.environ['spotipy_user']
            client_id = os.environ['spotipy_id']
            client_secret = os.environ['spotipy_secret']
        
        except:  # credentials not stored as system variables
            username = raw_input('user=')
            client_id = raw_input('id=')
            client_secret = raw_input('secret=')
        token = util.prompt_for_user_token(username, scope, client_id, client_secret, redirect_uri)
    
    except:  # cache file for user already exists
        os.remove('.cache-'+username)
        token = util.prompt_for_user_token(username, scope, client_id, client_secret, redirect_uri)

    return token
```


```python
# connect
token = get_token(scope='playlist-read-private')
sp = spotipy.Spotify(auth=token)
```


```python
# get playlists from current user (me in this case)
playlists = sp.current_user_playlists()
```


```python
# count nr playlists (private)
print "n playlists=", len(playlists['items'])
```

    n playlists= 50
    
