# Social Media Post Scheduler

A web application built with Django that lets users connect their social media accounts and schedule posts to be published automatically across multiple platforms, including Facebook, Instagram, LinkedIn, and X.

## Features

- Connect social media accounts via OAuth2
- Schedule posts with text, images, or links
- Automatic publishing at the scheduled time
- Simple dashboard to create, view, and track post status

## Tech Stack

- **Backend**: Django
- **Frontend**: AlpineJS
- **Task Runner**: Custom background poster process

## Quickstart

**1. Install dependencies**

```bash
uv sync
```

**2. Configure environment**

```bash
cp .env.example .env
```

Fill in your `SECRET_KEY`, `APP_URL`, and any social platform OAuth credentials you need.

**3. Run migrations**

```bash
make migrate-all
```

**4. Start the app**

```bash
make dev       # Django dev server
make poster    # Background post publisher (separate terminal)
```

App runs at `http://127.0.0.1:8000`.

## Docker

```bash
docker network create web   # one-time
make build
make start
```

## Environment Variables

See `.env.example` for the full list. Key ones:

| Variable | Description |
|---|---|
| `SECRET_KEY` | Django secret key |
| `APP_URL` | Your app's public URL (use ngrok for local dev) |
| `FACEBOOK_CLIENT_ID/SECRET` | Meta app credentials |
| `LINKEDIN_CLIENT_ID/SECRET` | LinkedIn app credentials |
| `X_CLIENT_ID/SECRET` | X (Twitter) app credentials |
| `SOCIAL_AUTH_GOOGLE_OAUTH2_KEY/SECRET` | Google OAuth credentials |


## Google SignIn Configuration

- Google [google console](https://console.cloud.google.com/);
- Create a new project;
- Configure OAuth2 consent page;
- Setup Branding (name, support email, logo, app domain, authorized domain) - that's when app goes to prod;
- In Clients page click `Create client` button add name set authorized JS origins (http://ngrok-for-dev for dev, http://ngrok-for-dev/complete/google-oauth2/ for dev redirect uri). Change it with your domain once app is ready for prod.
- Download secrets (you have later access to them as well);
- In the `.env` file fill `SOCIAL_AUTH_GOOGLE_OAUTH2_KEY` and `SOCIAL_AUTH_GOOGLE_OAUTH2_SECRET` with Cliend ID and Client Secret taken from previously downloaded json file with secrets;


## Facebook && Instagram

- You'll need a Facebook Public Page and an Instagram Professional account linked into Meta bussiness account;
- Google [meta developer social technologies](https://developers.facebook.com/);
- Create an app;
- Setup `Facebook login for business` there in add Settings:
    - OAuth Redirect URIs: https://ngrok-or-prod-url.app/facebook/callback;
    - Allowed Domains for the JavaScript SDK: ngrok-or-prod-url.app;
    - Deauthorize callback URL: https://ngrok-or-prod-url.app/facebook/uninstall/;
    - Data Deletion Request URL: https://ngrok-or-prod-url.app/facebook/uninstall/;
    - Save changes;
- Get App ID (FACEBOOK_CLIENT_ID) and App secret (FACEBOOK_CLIENT_SECRET) from App Settings > Basic and fill .env file;


## X

- A single X account is enough here;
- Google [x developer portal](https://developer.x.com/en/portal/dashboard);
- Create an app and generate OAuth 2.0 Client ID and Client Secret save them to .env;
- On User authentication settings click Edit button;
- Select Type of App > Web App and fill Callback URI / Redirect URL > https://ngrok-or-prod-url.app/X/callback/;
- Add your website url;
- Click Save button;


## Linkedin

- You'll need a Linkedin Company Page, posts will be made to the Linkedin account not the company page;
- Google [linkedin developer](https://developer.linkedin.com/);
- Create an app, link the Company Page;
- For Oauth2 select the following scopes: openid, profile, w_member_social, email;
- In Auth tab set the callback url: https://ngrok-or-prod-url.app/linkedin/callback/;
- Update .env file with Client ID (LINKEDIN_CLIENT_ID) and Primary Client Secret (LINKEDIN_CLIENT_SECRET);

App stores only the fields necessary for scheduling and publishing TikTok content on behalf of users.

When a user connects their TikTok account, the app saves:
- user_id / open_id (TikTok user identifier).
- access_token and refresh_token (to enable API calls).
- access_expire and refresh_expire (expiration timestamps).
- username and avatar_url (for account display in dashboard).

For each scheduled/published post, we save:
- comment_disabled (boolean) - to respect user’s comment preferences.
- duet_disabled (boolean) - to respect duet permissions.
- stitch_disabled (boolean) - to respect stitch permissions.
- privacy_level_options (list) - available visibility options for the user.
- max_video_post_duration_sec (integer) - to validate allowed video length.
- creator_username (string) - to identify the TikTok account linked.
- creator_nickname (string) - display name for reference.
- creator_avatar_url (string) - profile image for UI display.

The app store or process any unnecessary personal data beyond what is required for authentication, account identification, and content scheduling.

## Screenshots

Allow ImPosting to post on your behalf. Click on Authorize button. 
If you don't see your posts published come on this page and click on Authorize button again (the token expired and the app needs a new one). 

<img width="2520" height="1696" alt="image-1" src="https://github.com/user-attachments/assets/a28bb4a5-f959-4d08-9af0-bfe923d3cf3a" />


A simple calendar where you can see the days you have posts or not. Select day or click Post Today button.

<img width="2520" height="1696" alt="image-2" src="https://github.com/user-attachments/assets/db637875-047d-4211-8414-7f5284912893" />


Just a simple form to write your post and add a image.

<img width="2520" height="1696" alt="image-3" src="https://github.com/user-attachments/assets/7515fd3f-9d48-44ef-b0c5-89ed36c95569" />



You can view posts for selected day below the schedule form. 
The social media icons will turn green once published (click on them to view the post on those platforms).
