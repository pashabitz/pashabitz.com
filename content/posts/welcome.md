---
title: "Welcome"
date: 2020-07-13T15:30:17-07:00
draft: false
---
It's been a while. Running my blog off of a WordPress on EC2 was just very... undelightful :)
It was a pain to maintain, it cost a lot to run (in relative terms), it was ugly and slow.

I kept wanting to write but the experience was so cringy I never did.

So I decided it was time for a change.

I came up with what I think a blog should be like in 2020:
1. It is fast and scales
1. It looks great on the phone
1. It is easy to post to from the web, phone and camera
1. It intelligently posts to social media
1. It shows conversations around posts from social media
1. It works well with short, Twitter-like posts, as well as long-form posts

I decided to use a static site generator + serving off of S3.

# Hugo: Static Generator
After a 15 min exploration I settled on [Hugo](hohugo.io): it is fast, has great free templates and a great ecosystem. Setting up was a breeze and I was up-and-running on my laptop in 5 minutes.

Everything is open source and GitHub-based, content is all markdown. Perfect.

[Forestry](https://forestry.io/) is a CMS that tightly integrates with Hugo but that didn't seem necessary for me. 

My idea was to just store the content as a public GitHub repo.

# Importing from WordPress
This was, of course, a bit of a pain. WordPress can export posts to an XML file, with posts as HTML.

I used a little utility called [blog2md](https://github.com/palaniraja/blog2md) to convert from this export format to individual Markdown files. This was a life saver. Bless you, [palaniraja](https://github.com/palaniraja), I owe you a beer.

The other thing was that those WordPress posts pointed to images in its Media Library. Exporting those wasn't trivial. I had to install a plugin just to do that. Then, I copied those over to the content directory in the new blog and manually replaced referenced in the Markdown. This probably took 45 minutes. Good riddance.

I had my all my content properly imported.

# Hosting
Generating the final static files with Hugo is as easy as just the `hugo` command.

Hugo also supports [deploying to a variety of hosting setups](https://gohugo.io/hosting-and-deployment/). S3 websites are natively supported, as well as all other popular methods of serving a thing on the internet.

All it takes is [telling Hugo which bucket](https://gohugo.io/hosting-and-deployment/hugo-deploy/#configure-the-deployment) to push to and running `hugo deploy`.

Now I needed to set up the necessary resources in AWS:
1. S3 buckets
1. SSL certificate
1. CloudFront distributions (to speed things up and support SSL)
1. DNS

I worked off [this template](https://github.com/rogerchi/cloudformation-s3-website-ssl-with-redirect/blob/master/cloudformation-s3-website-ssl-with-redirect.yaml) and only made the most minimal changes. Thank you, [rogerchi](https://github.com/rogerchi/)!

With everything in place, I was ready to point my domain to the new name servers on AWS.

Three immediate gratifications:
1. The entire front page is tiny and downloads very fast
1. SSL lock icon in the address bar; not having it previously was so embarassing
1. No more EC2 instance to maintain and pay

# Automatic Deployments
At this point, the workflow for publishing new content was:
1. Write the content
1. Run `hugo && hugo deploy`

This was good but not great. I still depend on having a machine with Hugo on it. Obviously can't happen on my phone.

So, I added a GitHub Action to automatically deploy to S3 upon a push to the master branch. I haven't worked with GitHub Actions before, but for someone coming from a background of using CircleCI or similar, the learning curve was not steep at all. GitHub did a really good job with Actions!

I used the [Hugo setup](https://github.com/marketplace/actions/hugo-setup) Action to set up and just added my AWS credentials as secrets to the repository. The resulting [workflow file](https://github.com/pashabitz/pashabitz.com/blob/7a02620014186106c36fe782e55dd88d05641c5e/.github/workflows/main.yml) is very simple.

Now, the workflow is just push to master and the site gets generated and deployed automatically.

Some things still missing:
1. A way to quickly post a picture from my phone and have it become a post.
1. Automatically share new posts on social media.

