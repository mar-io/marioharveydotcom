# marioharveydotcom

My personal repo for my blog/microsite, [marioharvey.com](https://marioharvey.com)

Built with [Hugo!](https://gohugo.io/)

This blog is using a combination of cloudflare and backblaze.

cloudfare workers forward requests to backblaze buckets. this is essentially a serverless archtecture that rivals AWS S3 built in static page at an even lower cost.

## Pull in etch theme submodule

`git submodule update --init --recursive`

## Build Hugo static blog

`hugo -v -t etch`

## Push public folder (static html) to backblaze

`cd \public`
`b2 sync . b2://marioharvey/`

## Purge CDN cache if necessary
