# blog.ethereum.org
-------------------
This is the repository for blog entries to blog.ethereum.org. A blog editor is required to approve draft posts and merge them into the gh-pages branch to be published on the live site.

# How to submit a blog post.
## Preferred Method:
Follow this guide: https://docs.google.com/document/d/1TrId4aaham0MVFDQ9O7xi_HidKB_2RO15kvbdvLSW2k/edit?usp=sharing

## Alternate Method:
Create a .md document in the _drafts folder with a naming convention and header content (title, date, author, layout) similar to blog entries in the _posts folder.

# Publishing your blog post
Once you create your draft blog post, contact a blog editor who will look over the post to make sure it is formatted correctly. Once it is approved it will be merged into the gh-pages branch where it will be broadcast live to the interwebz.

# Features
Blogs need to be written in [Markdown format](https://guides.github.com/features/mastering-markdown/). If you are unfamiliar with Markdown, see the **Useful Markdown Links** section below.

## Extra features
You have the ability to embed YouTube videos, code snippets, and GitHub gists into your blogs.

### Embed YouTube clips
Paste the following into your blog entry, replacing T5RcjYPTG9g with the video ID found in the video URL.
{% include youtubePlayer.html id="T5RcjYPTG9g" %}

### Embed GitHub Gists
Paste the following into your blog entry, replacing 8387126 with the Gist ID found in the Gist URL.
{% gist 8387126 %}

### Create a code snippet
If you don't want to use a GitHub gist, you can highlight a code snippet. Replace "ruby" with a supported Rouge language.
Visit [the Rouge site](http://rouge.jneen.net/) to see what languages are supported.

{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

or

\```
code snippet goes here
\```


# Useful Markdown links
- [Markdown GitHub Page](https://guides.github.com/features/mastering-markdown/)
- [Markdown Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
- [Alternative Markdown Editor (useful for creating tables)](https://hackmd.io/)
