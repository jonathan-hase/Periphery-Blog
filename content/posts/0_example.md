---
title: "My 1st post"              # The page title, shown in the browser and in listings
description: "The first post of periphery"         # Meta description for SEO and social sharing
date: 2020-09-15T11:30:03+00:00    # Publication timestamp, used for sorting and display
author: "periphery"               # Author name; can also be a list for multiple authors

draft: false                      # Hugo: if true, page is omitted unless built with --buildDrafts
searchHidden: true               # PaperMod: omit this page from client-side search

comments: true
bluesky_post_uri: "https://bsky.app/profile/washingtonpost.com/post/3lrl42ja75k2x"
bluesky_post_author: "volution.bsky.social"

cover:                            # Page cover image settings
  image: "https://upload.wikimedia.org/wikipedia/commons/1/18/Dog_Breeds.jpg"       # Path or URL to the cover image
  alt: "a dog"               # Alt text for accessibility
  caption: "a picture of a dog"               # Caption displayed under the cover
  relative: false                 # true if image is in a page bundle; false for static files
  hidden: false                   # true to hide the cover only on this page

---


### Heading

# H1
## H2
### H3

### Bold

**bold text**

### Italic

*italicized text*

### Blockquote

> blockquote

### Ordered List

1. First item
2. Second item
3. Third item

### Unordered List

- First item
- Second item
- Third item

### Code

`code`

### Horizontal Rule

—

### Link

[periphery blog](https://periphery.blog)

### Image

![alt text](/images/eiffeltower-global.jpg „Title“)

### Table

| Syntax | Description |
| ———— | ———— |
| Header | Title |
| Paragraph | Text |

### Fenced Code Block -> better use annotated code bloc, that has more styling options and can get annotated

``` LANG [OPTIONS]
{
  „firstName“: „John“,
  „lastName“: „Smith“,
  „age“: 25
}
```
MORE ABOUT THE LANG CODES AND OPTIONS HERE: https://gohugo.io/content-management/syntax-highlighting/

### Annotated Code Block
{{< highlight go "linenos=inline, hl_lines=3 6-8" >}}
package main

import „fmt“

func main() {
    for i := 0; i < 3; i++ {
        fmt.Println(„Value of i:“, i)
    }
}
{{< /highlight >}}

MORE https://gohugo.io/shortcodes/highlight/

### Footnote

Here’s a sentence with a footnote. [^1]

[^1]: This is the footnote.

### Heading ID

Wichtig für webhooks (URLs zu Überschriften)

### My Great Heading {#custom-id} 

### Definition List

term
: definition

### Strikethrough

~~The world is flat.~~

### Task List

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### Emoji

copy paste rein da 📯


### Horizontal Line -> eine Leerzeile über und unter der Line

---


