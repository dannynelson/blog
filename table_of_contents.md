# Creating a Table of Contents in Ghost with Markdown

A table of contents is very helpful for organizing a long blog post into sections. Turns out, it's really easy to do this in markdown with the Ghost blogging platform. Here's an example...

1. [Linking to a Section within a Page](#linkingtoasectionwithinapage)
2. [Using Markdown Syntax](#usingmarkdownsyntax)
3. [Go Crazy](#gocrazy)
4. [Top of Page]()

## Linking to a Section Within a Page

Basically, a table of contents is just a collection of links pointing to IDs within the same page. In plain HTML, we would do this by adding an ID to each section we want to link to:

  <h1 id="myheader">My Header</h1>

Then create a link with this ID tag attached to the end of the page URL:

  <a href="http://example.com/mypage#myheader"></a>

Ghost (and all markdown) supports raw HTML, so you could use this method if want. But there's an even easier way.

## Using Markdown Syntax

It turns out, Ghost adds ID tags to all the headings for you. The ID is just the heading text without spaces and in lowercase. For instance, a heading named **My Heading** would look like:

  <h1 id="myheading">My Heading</h1>

How convenient! Now, we can write a markdown-style link pointing to this id:

  [Link to My Heading](#myheading)

Or a list of links:

  1. [Link to My Heading 1](#myheading1)
    2. [Link to My Heading 2](#myheading2)
    3. [Link to My Heading 3](#myheading3)

Or even a link back to the top of the page

  [Top of Page](#)

## Conclusion

That's it! Now you can have fun writing an epic long blog post with a table of contents.

