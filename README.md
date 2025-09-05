We are going to query the Get Artist endpoint from the Spotify Web API.
First get your Client ID and Client Secret following instructions in the Spotify documentation. Also find the Spotify ID of the artist you want to find information on.
    
    client_id = "cf57b3d57fce410090a317ba93928ca8"
    
    client_secret = "36790883650d40518807e761ac5a54af"

    artist_id = "3gN8Ihw22Vt9mnK97gbwMQ?si=7PLM0wLDQO2waNyJg4I0Sg"

    
Next we need to generate an access token, which is like authorisation for us to query the API (it will last an hour, after which a new one would need to be generated). Getting this access token requires querying a different endpoint.
    
Import the Python requests library:
    import requests
    
Define the information we need to make the access token request. We are using the client_id and client_secret variables in our request:
    url = "https://accounts.spotify.com/api/token"
    headers = {"Content-Type": "application/x-www-form-urlencoded"}
    data = {
        "grant_type": "client_credentials",
        "client_id": client_id,
        "client_secret": client_secret
    }
   
Make a post request to the endpoint defined above. Save it in a variable called token_response_request. token_response_request is a status code telling us whether the API call was succesful or not - 200 indicates the call was successful.
    
    token_request_response = requests.post(url, headers=headers, data=data)
    print(token_request_response) 
    
To return data from the API call, use token_request_response.json(). To make the items in the json easier to reference, let's save this as a variable

    
    token_json = token_request_response.json()
    print(token_json)
    
Now we want to extract the access token from the json, and save it as access_token:
    
    access_token = token_json["access_token"]
    
    print(access_token)
    
Now we have our access token! So we have everything we need now to call Get Artist endpoint.
I want to return the artist's name, followers, popularity and a link to their profile image.
```python  
Import the requests library:
import requests 
    
Set the url variable. We use an f-string here to allow us to embed the artist_id varibale directly in a string.
    url_artist_api = f"https://api.spotify.com/v1/artists/{artist_id}"
    
We also need to set a header - public APIs don't require a header but APIs that require authentication do - a header is like packaged up authentication.
    headers_artist_api = {
        "Authorization": f"Bearer {access_token}"
    }
    
    
Set a response in a variable
    response = requests.get(url=url_artist_api, headers=headers_artist_api)
    
    print(response) # indicates whetehr the API call was successful
    print(response.json()) # prints whole json response
    
    // Let's pick out the bits of information we want. First we will put response.json() into a variable so its simpler to reference:
    
    data = response.json()
    
    print(data["followers"]["total"]) # prints total followers
    
    print(data["images"][0]["url"])  # prints the url of the artist's image
    
    print(data["name"]) # prints the artist's name
    
    print(data["popularity"]) # prints the artist's popularity
    
    
    # Putting this together into a sentance:
    
    print(data["name"], "has", data["followers"]["total"], "followers on Spotify, with an artist popularity of", data["popularity"], "- their photo can be viewed via", data["images"][0]["url"])
    
    
    
    



.. parsed-literal::

    <Response [200]>
    {'access_token': 'BQDqVtUcbnnHUyRfzWDuy_6AOOWe2fak5KLCQZVexNMU9Td84vTc-ow2NjS6cHtSNJyDPn0_nc5Dw3WTskwtQo2E8QCBJNC95fPRX0RxSz91t6HPIpvz_fHHBlBYa-lOeep8KuELlDQ', 'token_type': 'Bearer', 'expires_in': 3600}
    BQDqVtUcbnnHUyRfzWDuy_6AOOWe2fak5KLCQZVexNMU9Td84vTc-ow2NjS6cHtSNJyDPn0_nc5Dw3WTskwtQo2E8QCBJNC95fPRX0RxSz91t6HPIpvz_fHHBlBYa-lOeep8KuELlDQ
    <Response [200]>
    {'external_urls': {'spotify': 'https://open.spotify.com/artist/6eXZu6O7nAUA5z6vLV8NKI'}, 'followers': {'href': None, 'total': 1222461}, 'genres': [], 'href': 'https://api.spotify.com/v1/artists/6eXZu6O7nAUA5z6vLV8NKI', 'id': '6eXZu6O7nAUA5z6vLV8NKI', 'images': [{'url': 'https://i.scdn.co/image/ab6761610000e5eba22264dfbad2d96ffc6ee2e0', 'height': 640, 'width': 640}, {'url': 'https://i.scdn.co/image/ab67616100005174a22264dfbad2d96ffc6ee2e0', 'height': 320, 'width': 320}, {'url': 'https://i.scdn.co/image/ab6761610000f178a22264dfbad2d96ffc6ee2e0', 'height': 160, 'width': 160}], 'name': 'Little Simz', 'popularity': 69, 'type': 'artist', 'uri': 'spotify:artist:6eXZu6O7nAUA5z6vLV8NKI'}
    1222461
    https://i.scdn.co/image/ab6761610000e5eba22264dfbad2d96ffc6ee2e0
    Little Simz
    69
    Little Simz has 1222461 followers on Spotify, with an artist popularity of 69 - their photo can be viewed via https://i.scdn.co/image/ab6761610000e5eba22264dfbad2d96ffc6ee2e0

