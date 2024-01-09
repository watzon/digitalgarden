---
{"dg-publish":true,"permalink":"/self-hosted-cloudinary-alternative/","noteIcon":""}
---

There are several open source projects which claim to be alternatives to Cloudinary and [[Bookmarks and Highlights/Collections/Articles/A handier Cloudinary alternative Uploadcare. Review and comparison — Uploadcare Blog\|Uploadcare]], but all of them (that I can find) are completely API based and work on URLs to existing images. What I would like to see is a full self-hosted website which allows you to upload images, apply modifications, and grab links to the cached image.

## Stack
- [Athena](https://athenaframework.org/) backend (Crystal web framework inspired by Symfony)
- [Nuxt](https://nuxt.com/) frontend

Since I'll be using Crystal, it only makes sense to use [Pixie](https://github.com/watzon/pixie) as an ImageMagick library. If I didn't dislike Django and Flask so much I would probably use [Pillow-SMID](https://github.com/uploadcare/pillow-simd) like Uploadcare does, but _c'est la vie_.

## Ideas
- I want to make the URL format very similar to what Cloudinary does (`/:uuid/-/:transform/:options`). This should be pretty easy to do in Athena [^1].
- This should include support for [[Bookmarks and Highlights/Collections/Programming/Progressive JPEG images What Is It and How It Can Improve Website Performance\|progressive JPEG's]], [auto format conversion](https://uploadcare.com/docs/transformations/image/compression/#operation-format-auto), [metadata stripping](https://uploadcare.com/docs/transformations/image/compression/#meta-information-control), and more.
- S3 and minio should be supported as storage backends.
- To start off with I'm imagining something pretty simple:
	- There needs to be a way to upload files and manage uploaded files.
	- Clicking a file should take you to a page where you can apply transformations to the image and get a link.
- In the future things can get more advanced:
	- Analytics
	- Real-time editor
	- JavaScript libraries for popular frameworks
	- Embeddable uploader
	- Webhooks
	- Support for videos

## Needed Routes

- `GET /image/:uuid/:transformations` - fetch the given image by id and apply the requested transformations. The `/image` prefix will make it easier to transform this route in NGINX or Caddy.
- `/api`
	- `GET /health` - return API status information
	- `/projects`
		- `GET /` - list all projects
		- `GET /:uuid` - return information about the given project
		- `POST /` - create a project
		- `PATCH /:uuid` - update a project
		- `DELETE /:uuid` - delete a project
		- `/files`
			- `GET /` - list all files for the given project
			- `GET /:uuid` - return information about the given file
			- `POST /` - upload an file
			- `PATCH /:uuid` - modify an files's name, metadata, etc.
			- `DELETE /:uuid` - delete a file
			- `GET /:uuid/cached` - return information about cached versions of a file
			- `GET /:uuid/analytics` - return analytics about a file
		- `/analytics`
			- `GET /traffic` - CDN bandwidth usage
			- `GET /storage` - storage usage
			- `GET /requests` - API requests
## Inspiration

![Pasted image 20240108150411.png](/img/user/attachments/Pasted%20image%2020240108150411.png)
![Pasted image 20240108150601.png](/img/user/attachments/Pasted%20image%2020240108150601.png)

[^1]: [[https://athenaframework.org/Routing/Route/#Athena::Routing::Route--slash-characters-in-route-parameters\|https://athenaframework.org/Routing/Route/#Athena::Routing::Route--slash-characters-in-route-parameters]]