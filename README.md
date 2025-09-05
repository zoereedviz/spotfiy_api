### 🎶 Querying the Spotify Get Artist API 🎶

This is a step-by-step guide to calling the Get Artist endpoint from the Spotify Web API.

## Ingredients

You will need:
- A Client ID and Client Secret (find these following instructions here https://developer.spotify.com/documentation/web-api/tutorials/getting-started#create-an-app)
- The Spotify ID of your artist of choice (more details here https://developer.spotify.com/documentation/web-api/concepts/spotify-uris-ids)

```python
client_id = "xxx"
    
client_secret = "yyy"

artist_id = "3gN8Ihw22Vt9mnK97gbwMQ?si=7PLM0wLDQO2waNyJg4I0Sg"
```
    
Next we need to generate an access token, which is like authorisation for us to query the API (it will last an hour, after which a new one would need to be generated). Getting this access token requires querying a different endpoint.

```python
# Import the Python requests library:
import requests
    
# Define the information we need to make the access token request. We are using the client_id and client_secret variables in our request:
url = "https://accounts.spotify.com/api/token"
headers = {"Content-Type": "application/x-www-form-urlencoded"}
data = {
    "grant_type": "client_credentials",
    "client_id": client_id,
    "client_secret": client_secret
}
   
# Make a post request to the endpoint we just defined, and save it in a variable called token_response_request. This returns a status code telling us whether the API call was succesful - status code 200 indicates that the call was successful.    
token_request_response = requests.post(url, headers=headers, data=data)

# To return data from the API call, we need token_request_response.json(). To make the items in the json easier to reference, let's save this as a variable: 
token_json = token_request_response.json()
    
# Now we want to extract the access token from the json, and save it as access_token:    
access_token = token_json["access_token"]
```
   
Now we have our access token! So we have everything we need now to call the Get Artist endpoint.
Our goal is to return the artist's name, followers, popularity and a link to their profile image.
```python  
#Import the requests library:
import requests 

# Set the url variable. We use an f-string here to allow us to embed the artist_id varibale directly in a string.
url_artist_api = f"https://api.spotify.com/v1/artists/{artist_id}"
    
# We also need to set a header - a header is like packaged up authentication. Public APIs don't require them but APIs that require authentication do.
headers_artist_api = {
    "Authorization": f"Bearer {access_token}"
}
   
# Save the response in a variable - again, this is a status code indicating whether the API call was successful.
response = requests.get(url=url_artist_api, headers=headers_artist_api)
    

    print(response.json()) # prints whole json response
    
# response.json() with return json data from the API. Let's save this as a variable so that it's simpler to reference:
data = response.json()

# Pick out the bits of information we want from the json: 

followers = (data["followers"]["total"])
    
image_url = print(data["images"][0]["url"])
    
name = (data["name"])
    
popularity = (data["popularity"])
    
    
    # Putting this together into a sentance:
    
    print(name, "has", followers, "followers on Spotify, with an artist popularity of", popularity, "- their photo can be viewed via", image_url)
```
