---
layout: default
title: Snippets
parent: Bunch Files
---
# Avoid Repitition With Reusable Snippets

If you have a series of tasks that are often repeated between Bunches, you can separate them into their own "snippet" file and include them in any Bunch. 

A snippet file can be named with any extension other than `.bunch`, and can be stored in the same folder as your Bunches or in a subfolder.

To include a snippet in a bunch, use `< snippet.name` on a line in the Bunch.

Snippet contents are treated like part of their calling Bunch, so any apps that are launched by the snippet will be closed when the Bunch closes.

## Snippet Variables {#snippetvariables}

To make snippets flexible, Bunch handles variables defined in the containing Bunch and replaced within the snippet. These are defined like files on the lines following the `<` line.

    < generic.snippet
    - proj_path=~/Code/MyProject

Now you can use `${proj_path}` anywhere in your snippet file, allowing you to use the same snippet for different projects.

    TaskPaper
    - ${proj_path}/todo.taskpaper
    %iTerm
    - ${proj_path}

### Default Variable Values {#defaultvalues}

If a snippet has variable placeholders but no values are provided when it's called, the placeholders will be removed. You can instead provide default values that will be used if no matching key/value pair is provided. To do this, just use a colon followed by the default within the placeholder:

    ${proj_path:~/projects}

## Referencing Partial Snippets (fragments) {#fragments}

You can define multiple snippets together in one file and label the sections with a hash and square brackets:

    #[Section Label]#####
    %nvUltra
    &myWorkflow

    #[Another Section]#####
    MoreStuff

Then you can reference the snippet with a fragment identifier, like this:

    <MySnippet.snippet#Section Label

If you load a snippet containing sections without using a fragment id, i.e. just `<MySnippet.snippet`, it will run all of the sections in the snippet.

If you feel like being creative, Bunch allows the use of either dashes or hashes to create the section dividers, and you can have a variable number of them on either side of the brackets (you do need one dash or hash on the left, and the head and tail characters should match). The following all work:

```
###[Section]
#[Section]#########
-------[Section]-----------
-[Section]-
------------------------------------------[Section]-
```

You get the idea. When you amass a lot of snippets in one file because you're  making great use of sections and fragments, it's just nice to make them look pretty...

## Optional Snippets With Dialog {#optionalsnippets}

You can have Bunch ask whether a snippet should be loaded when opening a Bunch by adding a query at the end of it. The format for the query is a question mark (?) immediately followed by a double-quoted string. The string within the quotes will be used as the text of the dialog, with buttons "Yes" and "No".

    < MySnippet.snippet ?"Load My Snippet?"

When the Bunch is launched, a dialog will ask "Load My Snippet?" and request user interaction. Clicking "Yes" will load the referenced snippet, clicking "No" will skip loading it. This can be used with variables and fragments, as well:

    <General.snippets#Spotify ?"Play some music while you work?"
    - url=spotify:playlist:3cSpIL4Q0H3uqdBMbT6c9x
    - autoplay=true

{% img mx-auto /bunch/images/optionalsnippet.gif 806 454 %}

You can include multiple optional snippets, but --- due to the asynchronous way Bunches are launched --- the questions may not be asked in file order. Be sure to make the queries descriptive! 

Also, options only apply to regular snippets, not snippets specified for "on close" with `!`. If you apply a query to an "on close snippet," the question will still be asked on launch.

## Run When Closing

Like most script types in Bunch, you can precede a snippet line with an exclamation point (`!`) to run it when the Bunch closes instead of when it opens.

## Wait Until Apps Have Launched

If you indent a snippet line by 4 spaces or one tab, it will automatically try to wait until all of the apps in the bunch have launched (or quit, if they're `!apps`). There's a dropdead timeout in case not all apps properly report their launch/termination to the OS.

```
Skype
Audio Hijack
    <useful.snippets#Position Podcast
```

This is especially handy for running window management scripts ([ala Moom](/bunch/docs/integration/moom/)) that need all of the apps to have windows present. It's more flexible than just putting a hard delay on the script, as it will take into account unusually long (or short) launch times. Just put the script line into a snippet or fragment and call it with an indented line.

You can have multiple indented snippets in a Bunch. Indented snippets also work with [additional time delays](/bunch/docs/bunch-files/delay/) as well as interactive optional snippets (see above).

