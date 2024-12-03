# Authenticating against multiple OpenID-Connect IdPs with Caddy

[Caddy](https://caddyserver.com/) is a modern, flexible web server written in
Go that can be extended with the
[caddy-security plugin](https://github.com/greenpau/caddy-security)
to implement authentication and authorization.

This repository provides a [`Caddyfile`](Caddyfile) that demonstrate how to set
up two different OpenID-Connect identity providers in Caddy, and protect some
arbitrary resource such that it can only be accessed if the user has been
successfully authenticated against one of the two IdPs.

## How to run it

Launch Caddy in a container using the [`Caddyfile`](Caddyfile) provided here:

    docker run --rm -p 8080:80 -v ./Caddyfile:/etc/caddy/Caddyfile ghcr.io/authcrunch/authcrunch
    xdg-open http://localhost:8080

Click on one of the two configured IdP, opening its login form. Enter any
arbitrary credentials, the mock IdP will not actually validate them and will
redirect you back to Caddy which will then store a session cookie tracking
which IdP has logged you in.

## How it works

This relies on [oauth.wiremockapi.cloud](https:////oauth.wiremockapi.cloud/)
as it provides a OpenID-Connect IdP that can be used without any setup and will
unconditionally log you in successfully regardless of the credentials used.

Two generic identity providers are set up using [oauth.wiremockapi.cloud](https:////oauth.wiremockapi.cloud/)
and the authentication portal is configured to list both of them.

Then a dedicated role is set up depending on which IdP is used.

The authorization policy then accepts users that have one of the above roles.

In the routes section, `/auth` hosts the authentication portal, and on the root
the authorization policy requires users to be logged in before returning some
static content.

## License

This configuration is under the term of the
[Unlicense](https://unlicense.org) to ensure you can do
whatever you want with it.
