## 🎶 Querying the Spotify Get Artist API using Python 🎶

This is a step-by-step guide to calling the Get Artist endpoint from the Spotify Web API.

### Ingredients

You will need:
- A Client ID and Client Secret (find these following instructions here https://developer.spotify.com/documentation/web-api/tutorials/getting-started#create-an-app)
- The Spotify ID of your artist of choice (more details here https://developer.spotify.com/documentation/web-api/concepts/spotify-uris-ids)

### How-to

```python
# Save your Client ID, Client Secret and Artist ID in variables
client_id = "xxx"
    
client_secret = "yyy"

artist_id = "zzz"
```
    
Next you need to generate an access token, which is like authorisation to call the API (each access token is valid for an hour, after which a new one needs to be generated). Getting this access token requires calling a different endpoint of the Spotify Web API.

```python
# Import the Python requests library
import requests
    
# Save the information needed to make the access token request in variables
url = "https://accounts.spotify.com/api/token"
headers = {"Content-Type": "application/x-www-form-urlencoded"}
data = {
    "grant_type": "client_credentials",
    "client_id": client_id,
    "client_secret": client_secret
}
   
# Make a post request to the endpoint using the varibales you just defined, and save this request in another variable. This request returns a status code indicating whether the API call was succesful (status code 200 indicates success)    
token_request_response = requests.post(url, headers=headers, data=data)

# To return data from the API call, rather than the status code, use token_request_response.json(). To make the values in the json easier to reference, save this as a variable.
token_json = token_request_response.json()
    
# Extract the access token from token_json, and save it in a varibale 
access_token = token_json["access_token"]
```
   
This is the access token obtained! So you have everything you need to call the Get Artist endpoint.
This guide will show you how to return the artist's name, followers, popularity and a link to their profile image.
```python  
#Import the requests library
import requests 

# Set the url variable. An f-string is used here to embed the artist_id varibale in a string.
url_artist_api = f"https://api.spotify.com/v1/artists/{artist_id}"
    
# Set the header. A header is like packaged up authentication - public APIs don't require them.
headers_artist_api = {
    "Authorization": f"Bearer {access_token}"
}
   
# Make a get request and save the response in a variable - again, this returns a status code indicating whether the API call was successful.
response = requests.get(url=url_artist_api, headers=headers_artist_api)

# To return data from the API call, use response.json(). Save this in a variable so that it's simpler to reference.
data = response.json()

# Pick out the desired pieces of information from the json data, and save it in variables 
followers = data["followers"]["total"]
    
image_url = data["images"][0]["url"]
    
name = data["name"]
    
popularity = data["popularity"]
    
# Put this together into a sentance, and use the Python print function to output a string
print(name, "has", followers, "followers on Spotify, with an artist popularity of", popularity, "- their photo can be viewed via", image_url)
```
You should get an output in the format '*artist's name* has *x* followers on Spotify, with an artist popularity of *y* - their photo can be viewed via *url link*.'
