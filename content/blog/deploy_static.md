---
title: "Auto-Deploying Static Site to S3"
date: 2018-09-22T14:44:04-07:00
draft: true
---

Static websites have a lot of advantages compared to dynamic web applications: easier to develop, cheaper to host, and better indexing for SEO. Performance is arguable; you might be able to match the TTFB and throughput of a static site with a well-configured reverse proxy, but why bother?

<!--more-->

## My Approach

There are a lot of options for developing and hosting a static site but this is what I went with for my website (the one you're on!):

- [Hugo](https://gohugo.io/getting-started/quick-start/) static site generator because I like working with markdown. [Jekyll](https://jekyllrb.com/docs/) is a good alternative.
- Bitbucket for built-in CI/CD via Bitbucket Pipelines. GitHub + Travis CI will accomplish the same thing.
- S3 for hosting and serving the generated files. Storage and bandwidth are fairly cheap ... as long as you're not serving too many image or video files.
- CloudFront for TLS, HTTP/2 support, and edge caching (not too relevant for my use case).
- Route 53 for domain registrar and DNS. I'm already on AWS, so why not.

## Set-Up

Before auto-deploying with Pipelines, a couple of things need to be set up. There are a lot of guides on generating a basic site with Hugo, so I won't go over it. The structure of the content directory is the most important thing because it will determine the site layout; this is what I'm using:

```
$ tree content
content
├── _index.md
└── blog
    ├── deploy_static.md
    ├── ...
    └── ...
```

All that's needed before deploying and hosting on S3 is generating the actual web artifacts with the `hugo` command. The contents of the generated `public/` directory can then be uploaded to S3.
