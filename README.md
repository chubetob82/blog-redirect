# blog-redirect

This repository exists to hold one hostname. It serves `blog.bolivarjesus.com` and forwards
every path to the same path on `www.bolivarjesus.com`.

It exists because **a GitHub Pages site carries exactly one custom domain**, and that is a
routing rule, not a DNS one. GitHub decides what to serve from the request's `Host` header
against the repository's `CNAME` file, so pointing a second hostname at `chubetob82.github.io`
in DNS does not work: it returns 404. Measured, not assumed:

```
Host: blog.bolivarjesus.com   (the CNAME file's value)   200
Host: chubetob82.github.io    (the Pages site's own name) 404
Host: anything.else                                       404
```

So the redirect has to live in a second repository that claims the hostname. That is all this is.

`404.html` does the work. GitHub Pages serves it for any path the repository does not have,
which here is every path, so one file covers the whole host. `index.html` is a copy of it, and
exists only so the bare hostname answers 200 while it redirects.

This is a client-side redirect, not a server 301: static hosting cannot send a real redirect
status. It carries `rel=canonical` and `robots: noindex, follow` so crawlers pass authority on
rather than indexing it, and a `meta refresh` for anything with JavaScript disabled.

The site itself is built from the private repository `chubetob82/bolivarjesus-site`, which
publishes `chubetob82/blog`. Nothing here is generated, and nothing here should grow.
