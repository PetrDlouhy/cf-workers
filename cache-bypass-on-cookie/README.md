# Bypass Cache on Cookie
Example of how to bypass cache based on request cookies or URL path. The specific cookies and URL paths can be modified as needed at the top of the script.

There is no explicit way to bypass the cache when "Cache Everything" is enabled and an explicit Edge TTL is configured (yet anyway) so the worker modifies the request URL to make it unique so it will not match the current cache and will go all the way through to the origin. It does require that the origin just ignore unknown query parameters but most do so it should work for most deployments.

As-configured it is set up for a default WordPress deployment with the default WordPress cookies configured for bypass as well as the /wp-admin/ path.

The worker currently bypasses ALL requests, not just HTML requests. It can be modified to only inspect HTML requests or pretty much any other arbitrary rules for when to bypass the cache.

The worker does not actively manage the cache (extending cache times, purging, etc) = it strictly bypasses any existing caches when the configured rules are matched.


## INSTALLATION:

1) Copy content of the cache-bypass-on-cookie.js file to the script section in worker editor.
2) Configure variables at the beginning to fit your needs.
3) Add route to fit URLs where you need to bypass your cache (i.e. `*.example.com/*`) under Workers menu.
4) Add new page rule like `https://*.example.com/*cf_cache_bust*` with `Cache Level: Bypass` before your other caching rules. This is needed if you want your headers to be delivered from server to user when bypassing cache (by both `BYPASS_URL_PATTERNS` and `BYPASS_COOKIE_PREFIXES`)

### Cache control from server

If you need more precise control over what will be cached from the server, you can set `cache-cookie=no-cache` or `cache-cookie=cache` cookie to achieve that. Be aware, that if going from cached mode to non-cached mode you will need to go over page listed in `BYPASS_URL_PATTERNS`.
