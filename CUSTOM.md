# Deploying to Docker in HomeLab

- Decide on a custom domain
  @box464@bookmarks.box464.com

- Make configuration changes
  /src/util.js
  `export const domain = "bookmarks.box464.com"; // edit this if you have a custom domain`

/account.json (copied from /account.json.example)
`{
  "username": "box464",
  "avatar": "https://cdn.glitch.global/8eaf209c-2fa9-4353-9b99-e8d8f3a5f8d4/postmarks-logo-white-small.png?v=1693610556689",
  "displayName": "Postmarks",
  "description": "An ActivityPub bookmarking and sharing site built with Postmarks"
}`

# Test

Test that it works as is with `npm start`

# Create Docker Image

On Apple Silicon Mac, you have to do some extra things.

First, this goes to a private docker hub entry.
Login to Docker Hub and create the private hub entry, noting the name.

```
docker build --platform linux/amd64 -t box464/postmarks .
docker images postmarks:latest
docker push box464/postmarks
```

# Portainer Setup

Pull in the new docker image
Create a random port masking - I used 3005 but originally used 3000 and there was some kind of error
Restart Policy: Unless stopped
The port issue was a big one.

Set Environment Variables:

```
ADMIN_KEY={Admin password}
SESSION_SECRET={Create a hash for encrypting the password}
ENVIRONMENT={dev or production - dev does not require SSL}
PORT={use what you setup in Portainer - it must match!}
```

Verify the image works locally
Create Cloudflare domain for bookmarks.box464.com

Create Nginx Proxy Manager Entry
Pointing to public Portainer IP and the PORT above.
Create SSL certificate
Again, I had some type of issue with postmarks.box464.com, maybe because I tried it so many times? Had to give up on that domain.

Noticed that webfinger doesn't work - Cloudflare? Not sure.
