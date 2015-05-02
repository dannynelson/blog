Snippets are a fantastic way to speed up your workflow in Sublime. They allow you to flesh out a lot of boilerplate code with just a few keystrokes.

{<1>}![Snippet Example](/content/images/2014/Jan/snippet_example.png)

Sublime Text 2 comes with many great snippets preinstalled. But it's easy to create more if you can't find the one you're looking for.

##Open a New Snippet

Within the tools menu, click "New Snippet".

{<2>}![open a new snippet](/content/images/2014/Jan/open_snippet.png)

This will bring up a page that looks something like this...

{<3>}![Edit snippet](/content/images/2014/Jan/empty_snippet.png)

##Edit your snippet

Change the code between the two `<content>` tabs (and after the `!CDATA[`). This is your boilerplate code.

Note that the `${}` are the sections you can edit by pressing tab. The number indicates the editing order, and the text after the ":" is the default content. You can also leave the default text empty.

As an example, `${2:snippet}` will be editted second, and has a default text of "snippet".

If I wanted to make a snippet for a jasmine describe block, it might look something like this:

{<4>}![Describe Snippet](/content/images/2014/Jan/Screen_Shot_2014_01_15_at_12_50_54_AM.png)

## Set tabTrigger and Scope

You can uncomment the tabTrigger and scope sections.

{<5>}![Uncomment tab trigger](/content/images/2014/Jan/Screen_Shot_2014_01_15_at_12_58_37_AM.png)

Tab trigger is the default text that shows up when you start typing your snippet.

And scope determines which types of files to activate the snippet on. For instance, `source.js` only shows up on javascript files, and `source.css` only shows up on CSS.

## Save your Snippet
Finally, save your snippet with **cmd-s**. Be sure to save it with a **.sublim-snippet** extension within the **Packages/User** directory of your Sublime installation.

{<6>}![Save](/content/images/2014/Jan/Screen_Shot_2014_01_15_at_1_07_41_AM.png)

You could also save it within a sub-directory of your /User folder if you want to keep all your snippets organized.

## Install More with Package Manager

If you enjoy using Snippets, you should check out snippet collections available from the Sublime Package Installer.

Once you have [Package Control installed](https://sublime.wbond.net/), press **cmd-shift-P**, and search for **Package Control: Install Package**.

Search for "snippet", and choose a collection to download.

## Conclusion

And that's it! Open a file and test it out your new snippets!

Have any good snippet packages you recommend? Let me know in the comments below.




