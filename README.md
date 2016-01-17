# docker-openresty-passenger-bug

To reproduce segmentation fault:

1. Clone this repo
2. Build images with:
    * docker build -t openresty-passenger:with-ssl openresty-passenger-debug-with-ssl/
    * docker build -t openresty-passenger:without-ssl openresty-passenger-debug-without-ssl/
3. Start container built without SSL support by running `docker run -p 8181:80 openresty-passenger:with-ssl` and check URLs.
4. Start SSL-enabled container with `docker run -p 8181:80 openresty-passenger:with-ssl` and check URLs.

## Test URLs

* http://127.0.0.1:8181/ - you will see 'hello world' sent by sinatra
* http://127.0.0.1:8181/blabla.txt - you will see 'static file test' from .txt file in app's public diriectory.
* http://127.0.0.1:8181/lua-cosocket - you will see 'hello world' which nginx got from sinatra using LUA cosockets
* http://127.0.0.1:8181/lua-capture-static - you will see 'static file test' which nginx got from passenger using LUA location.capture
* http://127.0.0.1:8181/lua-capture-cosocket - you will see 'hello world' which nginx got from /lua-cosocket using location.capture
* http://127.0.0.1:8181/lua-capture-passenger - you will see 'hello world' which nginx got from sinatra using location capture if nginx doesn't have SSL support OR 'connection was reset' error if nginx built with SSL support.

