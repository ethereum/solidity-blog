---
layout: post
published: false
title: Markdown Example
subtitle: Some resources to write posts in GitHub Markdown format.
date: '2018-04-03'
author: Hudson Jameson
---
It's very easy to make some words **bold** and other words *italic* with Markdown. You can even [link to Google!](http://google.com).

# Markdown Guide
[Click here](https://guides.github.com/features/mastering-markdown/)

# You can add images using the Insert Image button above.
The image will be auto-uploaded to the /img directory in the repo.

![dogethereum.png](https://github.com/ethereum/blog/blob/master/img/dogethereum.png "Mouseover Text Here. Wow!")

# You can use emojis!
:shiptit:
:rainbow:
:sunny:

https://help.github.com/articles/basic-writing-and-formatting-syntax/#using-emoji

# You can add GitHub gists when you want to add code samples.
{% gist 8387126 %}

Use the Gist ID.

## If you don't want to use a GitHub gist, you can highlight a code snippet.
{% highlight ruby %}
def foo
  puts 'foo'
end
{% endhighlight %}

Visit [the Rouge site](http://rouge.jneen.net/) to see what languages are supported.

# You can embed YouTube videos.
{% include youtubePlayer.html id="T5RcjYPTG9g" %}
