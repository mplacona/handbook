# Creating & editing blog posts with Markdown and GitHub

## Process overview

> NOTE: This documentation covers only the mechanics of adding a blog post, not considerations such as who needs to approve your post or when it should be published.

1. Create the blog post file
1. Create a pull request
1. Confirm via the preview deployment site that everything is looking good (the `deploy/netlify — Deploy preview ready!` link in the checks section)
1. Request a review from the editor who reviewed your draft

## Adding a blog post

Add a blog post by creating a Markdown file in one of the `sourcegraph/about` repository `content/blogposts` child directories using the following template as a starting point:

```markdown
---
title: The title
description: A 300 character limit field for describing your post. Use this is you want to specially craft the excerpt shown on the index page. Uses the first 300 characters of text from your post if this field does not exist.
authors:
  - name: The author name
    url: https://example.com/
  - name: Second authors name (optional)
    url: https://example-2.com/
publishDate: YYYY-MM-DDT10:00-07:00
tags: [blog]
slug: the-blog-slug
heroImage: /blog/thumbnail-image.jpg
socialImage: Use to set large social image i.e.  https://about.sourcegraph.com/blog/sourcegraph-social-img.png
canonical: Use to override the canonical link i.e. https://www.fastcompany.com/90565930/im-deaf-and-this-is-what-happens-when-i-get-on-a-zoom-call
published: true
videoID: 'dQw4w9WgXcQ'
---

Your markdown content goes here
```

The data between the `---` is called front matter and is used to provide post metadata. Important to note about this metadata, is that:

The `description` field is used as an excerpt for your post on the blog the index page.

- The `authors` field is for any author of the blog. The `url` field is optional but recommended. \* The indentations on this field are important to keep matching the example.
- The `tags` field should be left as `blog` until we incorporate filtering posts via tags.
- The `publishDate` field must be in the exact format above. Don't worry about the time, just change the date.
- As long as `published` is true, your post will be visible, even if the value of `publishDate` is set in the future.
- The `canonical` field is optional and only required to override the canonical link. Important for cross-posting blogs from personal blogs or published news sites. By default, set to `https://about.sourcegraph.com/blog/the-blog-slug`.
- The `socialImage` field is optional. Use the full path to image in order to be read properly on Twitter and Facebook. Ideal image size: 1,200 x 628 pixels. <a href="https://sproutsocial.com/insights/social-media-image-sizes-guide/" rel="nofollow" target="_blank">Latest social size guidelines</a>.
- The `videoID` field is an optional YouTube video ID and will take priority even if the `socialImage` is present. This will generate an inline video preview card when sharing on social media. This is supported for all types of posts; blog, podcast, and release posts.

## Adding images and other media

### Sizing images

- Make images as small as possible (aim for less than 200Kb).
- Images should be no larger than 1600px wide (if you want @2x retina quality) but often, this isn't needed and 800px is fine.
- JPEG images should be compressed at no larger than 80% quality to reduce file size.
- The [ImageOptim](https://github.com/ImageOptim/ImageOptim) app and CLI is great for significantly reducing the size of PNG files and JPEG files.

### Uploading images

- If you do not have a custom hero or social image, use this [default hero image](https://storage.googleapis.com/sourcegraph-assets/blog/default_hero_social.png).
- Small images can be placed in the `website/static/blog` directory and have the url of `/blog/example-image.jpg` in your markdown.
- Large images, GIFs, and other binary assets should be uploaded to the `sourcegraph-assets` Google Cloud Storage bucket. You can use the UI uploader at [https://console.cloud.google.com/storage/browser/sourcegraph-assets/blog](https://console.cloud.google.com/storage/browser/sourcegraph-assets/blog) or you can use the CLI with `gsutil cp local/path/to/myasset.png gs://sourcegraph-assets/`, with the image `src` being `https://sourcegraphstatic.com/blog/myasset.png`.
  - Note: You may need to request permission to upload files to the GCP bucket. If you see an error message that additional permissions are required, you can ask for help in #it-tech-ops on Slack.
- Please use lower case letters and hyphens instead of spaces in folder and image names:

<div className="usage">
  <div className="item yes">
    <h5>Yes</h5>
    <ul>
      <li>api-docs-hero.png</li>
    </ul>
  </div>
  <div className="item no">
    <h5>No</h5>
    <ul>
      <li>API docs hero.png</li>
    </ul>
  </div>
</div>

### YouTube video embed code

Uses Bootstrap for responsive sizing and adequate whitespace between adjacent elements, and that only Sourcegraph videos are shown on the end screen.

<!-- prettier-ignore -->
```html
<div className="container my-4 video-embed embed-responsive embed-responsive-16by9">
  <iframe
    className="embed-responsive-item"
    src="https://www.youtube-nocookie.com/embed/${YOUTUBE_ID}?autoplay=0&amp;cc_load_policy=0&amp;start=0&amp;end=0&amp;loop=0&amp;controls=1&amp;modestbranding=0&amp;rel=0"
    allowFullScreen=""
    allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
    frameBorder="0"
  ></iframe>
</div>
```

### HubSpot form embed code

Inserts HubSpot form wherever the `targetId` div tag is placed.

```html
<EmbeddedHubSpot portalId="2762526" formId="your-form-id" targetId="#your-target-id" />
<div id="your-target-id"></div>
```

### Adding a screenshot or screen recording

You can read about [embedding GIFs and videos](process/adding_screenshots_screen_recording.md) here.

## Previewing your blog post

It's recommended to run the development site to preview your blog post locally.

Once your pull request is created, you can preview your blog post through the netlify build. To do so:

- In your PR, on the 'conversation' tab
- Find the checks at the bottom
- Find the deploy/netlify check and click the details link
- This will open a build of the Sourcegraph marketing website
- Add /blog to the end of the url

## Publishing your post

Once your pull request has been approved and merged, a new build of the production site will be triggered and your post will be live in 5 minutes.

### Troubleshooting: If your blog post is not appearing on the blog index page

If you're not seeing your blog post on the index page, check that:

- Your blog post file has a `.md` file extension
- That it's in a child directory of the `blogposts` directory
- That your frontmatter data matches that of the template, e.g., make sure the `publishDate` format is correct

## Editing blog posts

Fixing, editing, and updating a blog post on [about.sourcegraph.com](https://about.sourcegraph.com/blog/) is easy, can be done in minutes, and does not require running code locally.

This video shows the process from start to finish, although only those with repository push access will be able to squash and merge the change.

<p className="container">
  <div style={{ padding: '56.25% 0 0 0', position: 'relative'}}>
    <iframe src="https://www.youtube-nocookie.com/embed/15hE2BCyMCQ" style={{ position: 'absolute', top: '0', left: '0', width: '100%', height: '100%' }} frameBorder="0" webkitAllowFullscreen="" mozAllowFullScreen="" allowFullScreen=""></iframe>
  </div>
  <p style={{ textAlign: 'center' }}>
    <a href="https://www.youtube.com/watch?v=15hE2BCyMCQ" target="_blank" rel="noreferrer">Watch on YouTube</a>
  </p>
</p>