# Static Asset Caching

Normally, you need to run a web server in your container to serve static assets like HTML files, CSS, Javascript or images. To make things faster, you will probably take advantage of a CDN to serve these assets to end users. Fly is not exactly a CDN but we operate CDN-like infrastructure which runs apps closer to the end user.

By adding a `[[statics]]` block to your `fly.toml` configurations like this:

```
[[statics]]
  guest_path = "/app/public"
  url_prefix = "/public"
```

You can serve static files from your container. The `[[statics]]` block maps a url prefix to a path inside your container. When you deploy, `/app/public` will be pulled out of your packaged app and requests will be served from `https://<app>/public`. These requests will be cached on Fly's edge servers.

Fly's Anycast network looks for static mappings defined in your configuration and satisfies requests directly from our proxy, without passing the actual HTTP request to your app. This is much faster and easier.

We don't stop there. You can have up to 10 statics mappings in your application. The `guest_path`, the path inside your container where the static files to be served are located, can overlap with other static mappings however, the `url_prefix` should not i.e you can have two mappings `/public/test` and /`public/train` but you can't have more than one mapping to `/public`. You don't have worry about things like cache purging and expiration, everything works out of the box.

Its important to note the static cache service doesn't honor symlinks at the moment so, you'll want to stick with absolute original paths in your static configuration.

Finally, this is not a replacement for a CDN like Netlify and you will probably need to assess your need. The static cache service is specifically designed for assets that get deployed alongside Phoenix/Rails/Django/ apps. In other words if you are looking for just a static host you are probably better served by Netlify or similar. Luckily most people don't need CDNs, so this solution works great.
