_digitalocean-callback_
=======================

[DigitalOcean](https://digitalocean.com/) authorization helper, built with [Scotty](https://github.com/scotty-web/scotty).  Intended to supply the callback URL for the [authorization code flow](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2#grant-type:-authorization-code).


Usage
-----

Listens for HTTP `GET` requests at `/callback`.  All requests are forwarded to the DigitalOcean [`/v1/oauth/token`](https://developers.digitalocean.com/oauth/#request-access-token) endpoint.

If an incoming request includes an authorization code, the helper requests an access token.  If an access token is granted, the helper redirects the user to the target URL, with additional `access_token`,  `expires_in`, and `refresh_token` parameters.

On failure, the user is also redirected to the target URL, with an `error` parameter.

The `state` parameter is included both on success and on failure, if it was supplied with the authorization code request.

| Query parameter | Description
| :-------------- | :----------
| `access_token`  | Access token.  Included on success.
| `expires_in`    | Access token expiry time.  May be included on success.
| `refresh_token` | Refresh token.  May be included on success.
| `error`         | Either `no_code` or `no_token`.  Included on failure.
| `state`         | Arbitrary string.  Optional.


### Configuration

Authentication credentials and defaults can be configured by setting environment variables.

| Environment variable         | Description
| :--------------------------- | :----------
| `DIGITALOCEAN_CLIENT_ID`     | Application identifier.  Required.
| `DIGITALOCEAN_CLIENT_SECRET` | Authentication token.  Required.
| `CALLBACK_URL`               | Helper’s own URL, including `/callback`.  Required.
| `TARGET_URL`                 | URL to which the user is redirected.  Required.
| `PORT`                       | HTTP listening port.  Defaults to `8080`.


### Deployment

Installs in seconds on most Linux and OS X machines, using [Halcyon](https://halcyon.sh/).

```
$ halcyon install https://github.com/mietek/digitalocean-callback
$ export DIGITALOCEAN_CLIENT_ID=…
$ export DIGITALOCEAN_CLIENT_SECRET=…
$ export CALLBACK_URL=…
$ export TARGET_URL=…
$ digitalocean-callback
```


#### Deploying to Heroku

Ready to deploy in one click to the [Heroku](https://heroku.com/) web application platform, using [Haskell on Heroku](https://haskellonheroku.com/).

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/mietek/digitalocean-callback)

Clicking the button is equivalent to executing the following commands:

```
$ git clone https://github.com/mietek/digitalocean-callback
$ cd digitalocean-callback
$ heroku create -b https://github.com/mietek/haskell-on-heroku
$ heroku config:set DIGITALOCEAN_CLIENT_ID=…
$ heroku config:set DIGITALOCEAN_CLIENT_SECRET=…
$ heroku config:set CALLBACK_URL=…
$ heroku config:set TARGET_URL=…
$ git push heroku master
$ heroku ps:scale web=1
$ heroku open
```


About
-----

Made by [Miëtek Bak](https://mietek.io/).  Published under the [MIT X11 license](https://mietek.io/license/).
