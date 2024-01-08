---
{"dg-publish":true,"permalink":"/self-hosted-cloudinary-alternative/","noteIcon":""}
---

There are several open source projects which claim to be alternatives to Cloudinary, but all of them (that I can find) are completely API based and work on URLs to existing images. What I would like to see is a full self-hosted website which allows you to upload images, apply modifications, and grab links to the cached image.

## Stack
- [Athena](https://athenaframework.org/) backend (Crystal web framework inspired by Symfony)
- [Nuxt](https://nuxt.com/) frontend

Since I'll be using Crystal, it only make sense to use [Pixie](https://github.com/watzon/pixie) as an ImageMagick library. If I didn't dislike Django and Flask so much I would probably use [Pillow-SMID](https://github.com/uploadcare/pillow-simd) like Uploadcare does, but _c'est la vie_.

## Ideas
- I want to make the URL format very similar to what Cloudinary does (`/:uuid/-/:transform/:options`). This should be pretty easy to do in Athena[^1].


[^1]: [[https://athenaframework.org/Routing/Route/#Athena::Routing::Route--slash-characters-in-route-parameters\|https://athenaframework.org/Routing/Route/#Athena::Routing::Route--slash-characters-in-route-parameters]]