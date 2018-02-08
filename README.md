# What Is The Asset Pipeline?

## Objectives

1. State the general purpose of the Asset Pipeline
1. Explain how concatenation, minification, and compression affect performance

## Introduction

The Asset Pipeline (AP) was a highly contentious technology when introduced in 2011.
Over time, it has proven to be a wise move and has been emulated in a host of
other technologies (Django for Python, Webpack for Node). It essentially helps
improve users' experience of page load time by:

> ... [providing] a framework to concatenate and minify or compress JavaScript
> and CSS assets. It also adds the ability to write these assets in other
> languages and pre-processors such as CoffeeScript...[and]... Sass... It
> allows assets in your application to be automatically combined with assets
> from other gems.

[Source][APRG]

Let's dig deeper into this statement.

## Parsing the Docs

The AP provides a means to **concatenate**. This is a technology-word which
means "to join together into one" or to "pour into the same container." It also
uses the word **asset** when the word **file** would probably do just as well.
Computers don't need whitespace in code, but human programmers like it. But if
you're trying to save space you can remove all the whitespace (Space, Carriage
Return) and make the whole file as if it were one _very long_ line; this is
called **minification**. Some web servers will even let you send your "assets"
as if they were in a [zipfile][] across the Internet; this is called
**compression**.

So the first sentence is:

> The AP provides a framework to join together JavaScript and CSS files into
> one file. It can squish out whitespace by minification and can shrink the
> thing you transfer over the internet with compression.

Let's try that second sentence now:

> It also adds the ability to write these (files' information) in other
> languages and to preprocess them, before being joined together, from
> languages like CoffeeScript or SASS into a languages that the browser
> natively understands like JavaScript (for CoffeeScript) and CSS (for SASS).

Let's try that third sentence, at last:

> [The AP] allows (files) from your application to be automatically combined
> with assets from other gems.

It's important to try to come up with your own summary of what these
"translated" sentences mean _for you_ &mdash; _in your own words_. Take the
time to re-read the original introduction and see if you can make sense of this
in _your_ interpretation. If you need help, ask for it now.

## The General Purpose

A typical interview question might be to explain the general purpose of the
Asset Pipeline, so be sure you can answer it before reading ours. Keep in mind
this definition isn't as _formal_ but it might be easier to remember.

> The Asset Pipeline is a program, which is part of Rails, which searches
> several file locations (in my Rails app, in Gem paths) for CSS files or
> JavaScript files. It even has the ability to take files written in other
> languages and mangle them from some weird fancy-language like SASS or
> CoffeeScript and to turn them into good old CSS or JavaScript. Once all the
> data are gathered, it can then join them into one file which it then can
> "minify" or compress.

## Why Concatenate, Minify, and Compress

Let's answer the question of "Why Concatenate, Minify, and Compress?" by asking
the reverse, why separate CSS work into separate files? As we learned in "Intro
to MVC" it's helpful, as a human programmer, to separate types of work by file.
It's helpful to keep CSS about the top bar of our web site in a file called
`header.css`; it's helpful to keep information about the bottom-page styling in
`footer.css`; it's helpful to keep information about the body in `body.css`.

Let's consider a snippet from an imaginary `example.html` file.  HTML obviously
doesn't care if you:

```html
<link rel="stylesheet" type="text/css" href="./assets/header.css">
<link rel="stylesheet" type="text/css" href="./assets/body.css">
<link rel="stylesheet" type="text/css" href="./assets/footer.css">
```

```html
<link rel="stylesheet" type="text/css" href="./assets/combined_megacss_file.css">
```

The motivation is not for a _technical reason_ from within the browser. The
problem which the Asset Pipeline solves is best understood from the _user's_
perspective.

## The User Experience of Requesting Multiple Assets

Imagine a user visits our `example.html` page in a browser. Let's imagine a
cost must be paid for each network connection.

1. Cost to connect to example.html and receive the page
2. Blank screen is shown
3. Cost to find `<link>` tags that call for other assets
4. Cost to connect to the web server to grab `header.css`
5. Cost to connect to the web server to grab `body.css`
6. Cost to connect to the web server to grab `footer.css`
7. Cost to process the CSS files and work out the cascade (what rule overrides
   and controls the render)
8. Fully "rendered" page is displayed and the user perceives "Page Ready."

Obviously if `example.html` has more CSS pages (or JavaScript pages) each
_additional_ page has a _fixed cost_ associated with it and causes a
_degradation in time to page load_ as perceived by the end user.

Now let's imagine that the Asset Pipeline does its "concatenation."

1. Cost to connect to example.html and receive the page
2. Blank screen is shown
3. Cost to find `<link>` tags that call for other assets
4. Cost to connect to the web server to grab `combined-mega-css-file.css`
5. Gone!
6. Gone!
7. Cost to process the CSS files and work out the cascade (what rule overrides
   and controls the render)
8. Fully "rendered" page is displayed and the user perceives "Page Ready."

Imagine making `combined-mega-css-file.css` _**even**_ smaller by **minifying**
it and then by **compressing** it. We're moving a smaller package which still
provides us programmers all the advantages of separation of concerns.

## Conclusion

Concatenation, minification, and compression of assets held across multiple
directories to shrink the size of data transferred to end users thus decreasing
page load time is the primary benefit of the Asset Pipeline.

Once this primary benefit of the Asset Pipeline was realized, the clever folks
behind Rails found additional uses and, sigh, problems with this "mega asset."

We'll finish up thinking about concatenation, minification and compression by
discussing how to configure where assets to be processed are found. We'll then
talk about how the concatenation process could also be paired with
transformation. Finally we'll discuss how "Asset Fingerprinting" helps handle a
tricky issue.
