---
title: URLs
description: Set custom urls for pages using the `url` variable.
order: 8
---

As explained in [Page files](../creating-pages/page-files.md), the output
filename is generated using the original source file path:

```txt
posts/my-first-post.md  =>  /posts/my-first-post/index.html
```

By default, the pages are saved as "pretty URLs", using directories for the path
and a `index.html` file. So the final URL is `/posts/my-first-post/`. To disable
this behaviour, set the option `prettyUrls` to `false` in your `_config.js` file
(see [Configuration](../configuration/config-file.md#prettyurls)).

```js
const site = lume({
  prettyUrls: false,
});
```

```txt
/posts/my-first-post.md  =>  /posts/my-first-post.html
```

## The `url` variable

The variable `url` defined in a page allows customizing the output file
individually. For example:

```yml
---
title: My first post
url: /posts/welcome/
---
```

In this example, the `url` value changes the output file name:

```txt
/posts/my-first-post.md  =>  /posts/welcome/index.html
```

Note that manually defining the URL of a page means that the `prettyUrls` option
**won't have any effect.** For example:

```yml
# Use a trailing / to create pretty urls.
# For example, this outputs /posts/welcome/index.html
url: /posts/welcome/

# This outputs /posts/welcome (a file without extension)
url: /posts/welcome

# This outputs /posts/welcome.html
url: /posts/welcome.html
```

## Relative URLs

If you only want to change the last part of the URL, you can use relative paths.
For example:

```yml
---
title: My first post
url: ./welcome/
---
```

In this example, the page will be saved using the directory path where the
source file is saved but adding `welcome` in the last part of the URL.

```txt
/posts/my-first-post.md  =>  /posts/welcome/index.html
```

Using `../welcome/` as URL will also remove the last directory.

```txt
/posts/my-first-post.md  =>  /welcome/index.html
```

## URLs as functions

The variable `url` also accepts a function to generate the final value
dynamically. This function has the current page object as the first argument.

For example, let's say that we want to automatically generate all URLs of our
posts using the title value. We can create a `_data.js` file in the `/posts/`
directory with the following code:

```js
export function url(page) {
  return `./${page.data.title}/`;
}
```

Now, all pages in this directory share the same `url` function. The function
returns the title of the page as a relative URL, for example ,`./My first post/`
(See [Shared data](../creating-pages/shared-data.md)).

Because the URL is relative, the current directory is appended automatically (it
will be resolved to `/post/My first post/`). And if you are using the
`slugify_urls` plugin, all output paths are slugified automatically, so the
final URL would be `/post/my-first-post/`.

Using functions as URLs gives a lot of flexibility to generate the URLs as you
want.

## Setting url to `false`

Setting the `url` variable to `false` prevents the page being processed by Lume.

```yml
---
title: This is a title
url: false # Ignore this page for now
---
```
