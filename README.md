# blog.ethereum.org

This is the repository for blog entries to [blog.ethereum.org](https://blog.ethereum.org). A blog editor (currently Hudson Jameson) is required to approve draft posts and merge them into the gh-pages branch to be published on the live site. If these instructions are confusing to you or you'd rather just submit your blog directly to a blog editor instead of this repository reach out to the current editors.

## Current blog editors
#### Hudson Jameson - @Souptacular in the EF Discord or hudson@ethereum.org

## Features
Blogs need to be written in [Markdown format](https://guides.github.com/features/mastering-markdown/). If you are unfamiliar with Markdown, see the **Useful Markdown Links** section at the bottom.

You have the ability to embed images, YouTube videos, Twitter posts, code snippets, and GitHub gists into your blogs.

### Embed an image
![alt text](https://github.com/ethereum/blog/blob/master/img/doge.jpg "Mouseover Text Here. Wow!")

Please put the image files in the `img/[YEAR]/[MONTH]` directory in the repo.

```
![alt text](https://github.com/ethereum/blog/blob/master/img/doge.jpg "Title Text Here. Wow!")
```

If this step if confusing you can just send a blog editor the pictures and let them know where you want them in the blog.

### Embed YouTube clips
Paste the following into your blog entry, replacing T5RcjYPTG9g with the video ID found in the video URL.

`{% include youtubePlayer.html id="T5RcjYPTG9g" %}`

### Embed Twitter posts
Navigate to the Twitter post you want to embed. Clock on the arrow in the top-right corner of the tweet and click `Embed Tweet`. Copy the code and paste it into your blog.

### Embed GitHub Gists
Paste the following into your blog entry, replacing 8387126 with the Gist ID found in the Gist URL.

`{% gist 8387126 %}`

### Create a code snippet
If you don't want to use a GitHub gist, you can highlight a code snippet. Replace "ruby" with a supported Rouge language.
Visit [the Rouge site](http://rouge.jneen.net/) to see what languages are supported.

```
{% highlight ruby %}

def foo

  puts 'foo'

end

{% endhighlight %}
```

or

\```
code snippet goes here
\```

## How to submit a blog post
Create a .md document in the `_posts` folder in the `master branch` with a naming convention and header content (title, date, author, layout) similar to blog entries in the `_posts` folder (see examples below).

You can create a new file by navigating to the `_posts` folder above and clicking `Create File`.

![Example create post](https://blog.ethereum.org/img/2019/readme/new_file.png)

Trouble seeing the image or text? [Click here for the full size image.](https://blog.ethereum.org/img/2019/readme/new_file.png)

### Naming convention and header content
When naming a file use the following format:

`YYYY-MM-DD-blog-title-in-all-lowercase-without-symbols.md`

Example: `2019-09-19-example-title.md`

When adding the header content use the following format:

```
---
layout: post
published: true
title: 'Example Title!'
subtitle: 'Example subtitle!'
date: '2019-09-19'
author: Hudson Jameson
---
```

For an demonstration of the naming convention and the header content please see the GIF below:

![Example post GIF](https://blog.ethereum.org/img/2019/readme/create_post_ani.gif)

Trouble seeing the GIF or text? [Click here for the full size image.](https://blog.ethereum.org/img/2019/readme/create_post_ani.gif)

Additionally please look at other blog posts in the `_posts` folder to make sure you are doing it correctly.

## Publishing your blog post
Once you create your blog post and put it in the `_posts` folder contact a blog editor who will look over the post to make sure it is formatted correctly. Once it is approved it will be merged into the `gh-pages` branch where it will be broadcast live to the interwebz. Only blog editors can push to that branch. It can take up to ten minutes for the blog post to propagate and display on [blog.ethereum.org](blog.ethereum.org). If you let Hudson Jameson or Joseph Schweitzer know ahead of time we can post a link to your blog on the official @ethereum Twitter, LinkedIn, Facebook, and /r/ethereum subreddit.

## Useful Markdown links
- [Markdown GitHub Page](https://guides.github.com/features/mastering-markdown/)
- [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Alternative Markdown Editor (useful for creating tables)](https://hackmd.io/)
