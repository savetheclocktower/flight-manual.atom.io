---
title: Snippets
---
### Snippets
Snippets are an incredibly powerful way to quickly generate commonly needed code syntax from a shortcut.

In their simplest form, they act as abbreviations for longer strings of text. For instance, snippets let you type something like `habtm`, press the <kbd class="platform-all">Tab</kbd> key, and have that word become `has_and_belongs_to_many`.

But their more advanced features can help you cut down on typing even further. Here are a few things that can go into the body of a snippet:

* Slots (called "tab stops") that you can <kbd class="platform-all">Tab</kbd> through in order to type text of your choosing, and which can have default values (called "placeholders") that you can choose to overwrite or leave as-is
* A special tab stop called an "end stop" which determines where your cursor goes after the snippet has expanded
* Certain "magic" variables that expand into something that can vary — like today's date, or the current file name
* "Transformations" that can modify text in certain simple ways, and which can be applied to either variables or tab stops

#### Using snippets

You can use snippets without having to write them yourself. Many Core and Community packages come bundled with their own snippets that are specific to it. For example, the `language-html` package that provides support for HTML syntax highlighting and grammar comes with dozens of snippets to create many of the various HTML tags you might want to use. These snippets are "scoped" to HTML files so that they don't get suggested or invoked in non-HTML contexts.

If you create a new HTML file in Atom, you can type `html` and then press <kbd class="platform-all">Tab</kbd> and it will expand to:

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>

  </body>
</html>
```

It will also position the cursor in the `lang` attribute value so you can edit it if necessary. Many snippets have multiple focus points that you can move through with the <kbd class="platform-all">Tab</kbd> key as well.  For instance, in the case of this HTML snippet, after the cursor is placed in the `lang` attribute value, you can continue pressing <kbd class="platform-all">Tab</kbd> and the cursor will move to the `dir` attribute value, then to the middle of the `title` tag, then finally to the middle of the `body` tag.

To see all the available snippets for the file type that you currently have open, choose "Snippets: Available" in the Command Palette.

![View all available snippets](../../images/snippets.png "View all available snippets")

You can also use fuzzy search to filter this list down by typing in the selection box. Selecting one of them will execute the snippet where your cursor is (or multiple cursors are).

{{#tip}}

To work their magic, snippets apply a special mode to your editor that we'll call **interactive mode**. The only major difference between interactive mode and a normal editing mode is the behavior of <kbd class="platform-all">Tab</kbd> — it'll take you to the next tab stop in the snippet instead of inserting a literal tab character. <kbd class="platform-all">Shift+Tab</kbd> can also be used to cycle backwards through a snippet's tab stops.

Once a snippet is triggered, interactive mode is active until one of the following happens:

* You undo the expansion of the snippet.
* You tab all the way through the snippet until you reach the end stop.
* You move the cursor out of a tab stop with the arrow keys or the mouse.

After you leave interactive mode, the <kbd class="platform-all">Tab</kbd> key returns to its normal behavior.

If a snippet doesn't have any tab stops, or if it has only an end stop, then interactive mode is skipped — there's no need for it.

{{/tip}}

#### Creating Your Own Snippets

So that's pretty cool, but what if there is something the language package didn't include or something that is custom to the code you write? Luckily it's incredibly easy to add your own snippets.

There is a text file in your <span class="platform-mac platform-linux">`~/.atom`</span><span class="platform-windows">`%USERPROFILE%\.atom`</span> directory called `snippets.cson` that contains all your custom snippets that are loaded when you launch Atom. You can also easily open up that file by selecting the <span class="platform-mac">_Atom > Snippets_</span><span class="platform-linux">_Edit > Snippets_</span><span class="platform-windows">_File > Snippets_</span> menu.

##### Snippet Format

So let's look at how to write a snippet. The basic snippet format looks like this:

```coffee
'.source.js':
  'console.log':
    'prefix': 'log'
    'body': 'console.log(${1:"crash"});$0'
```

The leftmost keys are the selectors where these snippets should be active. The easiest way to determine what this should be is to go to the language package of the language you want to add a snippet for and look for the "Scope" string.

For example, if we wanted to add a snippet that would work for Java files, we would look up the `language-java` package in our Settings view and find its scope: `source.java`. Then we'd add a period to the beginning to end up with `.source.java`. (Internally, Atom puts a period before each segment of a scope name for technical reasons.) That's what we'd use as the top-level key for a Java snippet.

![Finding the selector scope for a snippet](../../images/snippet-scope.png "Finding the selector scope for a snippet")

The next level of keys are the snippet names. These are used for describing the snippet in a more readable way in the snippet menu. These names can have arbitrary text, including spaces.

Under each snippet name, the `prefix` key defines what you'd have to type to invoke the snippet (before pressing <kbd class="platform-all">Tab</kbd>), and the `body` key defines what gets inserted when the snippet is triggered.

{{#warning}}

Snippet keys, unlike CSS selectors, can only be repeated once per level. If there are duplicate keys at the same level, then only the last one will be read. See [Configuring with CSON](/using-atom/sections/basic-customization/#configuring-with-cson) for more information.

{{/warning}}

##### Multi-line Snippet Body

You can also use [CoffeeScript multi-line syntax](http://coffeescript.org/#strings) using `"""` for larger templates:

```coffee
'.source.js':
  'if, else if, else':
    'prefix': 'ieie'
    'body': """
      if (${1:true}) {
        $2
      } else if (${3:false}) {
        $4
      } else {
        $5
      }
    """
```

As you might expect, there is a snippet to create snippets. If you open up a snippets file and type `snip` and then press <kbd class="platform-all">Tab</kbd>, you will get the following text inserted:

```coffee
'.source.js':
  'Snippet Name':
    'prefix': 'hello'
    'body': 'Hello World!'
```

:boom: just fill that bad boy out and you have yourself a snippet. As soon as you save the file, Atom should reload the snippets and you will immediately be able to try it out.

##### Multiple Snippets per Source

You can see below the format for including multiple snippets for the same scope in your `snippets.cson` file. Just include the snippet name, prefix, and body keys for additional snippets inside the scope key:

```coffee
'.source.gfm':
  'Hello World':
    'prefix': 'hewo'
    'body': 'Hello World!'

  'Github Hello':
    'prefix': 'gihe'
    'body': 'Octocat says Hi!'

  'Octocat Image Link':
    'prefix': 'octopic'
    'body': '![GitHub Octocat](https://assets-cdn.github.com/images/modules/logos_page/Octocat.png)'
```

Again, see [Configuring with CSON](/using-atom/sections/basic-customization/#configuring-with-cson) for more information on CSON key structure and non-repeatability.

#### Snippet Syntax

Some of the snippets you've seen so far might be puzzling. That's because snippet bodies can use some special syntax to do powerful things, as we'll explain below.

##### Tab Stops

In a snippet body, each `$` followed by a number defines a **tab stop**. Tab stops define the places where the cursor will visit and order in which those places will be visited.

Tab stops will be visited in the order implied by their numbers; `$1` will be visited first, followed by `$2`, and so on. You can define multiple tab stops with the same number — they'll be visited at the same time, and Atom will create multiple cursors so that the characters you type are inserted into each tab stop simultaneously.

The end stop (`$0`) is a special tab stop. Despite its number, it will always be the _last_ tab stop in the snippet, and once you reach it, the snippet is finished. You can use it to mark the place you want the cursor to be at the end of the snippet.

Keep in mind that a snippet doesn't have to have _any_ tab stops. And a snippet that defines only an end stop is still useful. For example, a snippet with this body…

```
<blockquote>$0</blockquote>
```

…means "insert an opening and closing `blockquote` element, but put the cursor in between them."


##### Placeholders

Ordinary tab stops (`$1`, `$2`) don't have any content; they just mark a particular position for the cursor. But any tab stop can have a **placeholder** that defines the _default_ content that should be placed in the tab stop.

Remember the `console.log` snippet we defined above?

```
console.log(${1:"crash"});$0
```

If we invoke that snippet, here's what it immediately expands to:

![Example: Snippet Placeholders](../../images/snippets-example-placeholder.png "Example: Snippet Placeholders")

The text `"crash"` is present because it's the placeholder value for tabstop 1. And because we just expanded the snippet, tabstop 1 is the _active_ tab stop, which means that the placeholder content is selected. If you start typing at this point, `"crash"` will get overwritten by whatever you're typing. If you instead press <kbd class="platform-all">Tab</kbd>, the cursor will move to the end of the line and `"crash"` remains.

Placeholders are good for text that is _usually_ — but not always — what you want to type.

###### Tab Stops Inside Placeholder Text

Let's look at an advanced technique:

```
<div${1: class="$2"}>$0</div>
```

Here we've got a tab stop with a placeholder of ` class="$2"` — yes, that's another tab stop _inside_ the first tab stop's placeholder. How does this work?

Well, here's what this snippet will look like as soon as you expand it:

![Example: Snippet Placeholders (Advanced)](../../images/snippets-example-placeholder-advanced-1.png "Example: Snippet Placeholders (Advanced)")

Because it's a placeholder, the text `class=""` is pre-selected when you expand the snippet. From this point, one of two things can happen:

If you do want to give this `div` a `class`, you can press <kbd class="platform-all">Tab</kbd> and move your cursor to tab stop 2 inside the quotation marks:

![Example: Snippet Placeholders (Advanced)](../../images/snippets-example-placeholder-advanced-2.png "Example: Snippet Placeholders (Advanced)")

But if you don't need a `class` attribute on this `div`, you can press <kbd class="platform-all">Delete</kbd> or start typing to replace it with a different attribute. When you press <kbd class="platform-all">Tab</kbd> again, your cursor can't go to tab stop 2 — it marked a position in the text that doesn't exist anymore. Instead, the cursor jumps to the next tab stop — the end stop, in this case:

![Example: Snippet Placeholders (Advanced)](../../images/snippets-example-placeholder-advanced-3.png "Example: Snippet Placeholders (Advanced)")

So a placeholder can be more than just a string. It's also a way to branch off the behavior of your snippet in however complex a fashion as you want. Nearly any syntax which can go into a snippet body can go into a placeholder; keep that in mind as you learn about other features of snippet syntax.


##### Variables

The Snippets package makes certain values available as variables that can be used inside snippets.

For example, a snippet can insert whatever text you've got in your clipboard:

```
<a href="$CLIPBOARD">$1</a>$0
```

If you've just copied the URL `http://example.com` to your clipboard, expanding this snippet will display:

```
<a href="http://example.com"></a>
```

But variables aren't just a standalone feature — they can be combined with placeholders:

```
<a href="${1:$CLIPBOARD}">$2</a>$0
```

Expanding this snippet would instead produce…

```
<a href="http://example.com"></a>
```

…allowing you to treat the `$CLIPBOARD` value as a default that can be easily overridden by typing.

Here's the full list of supported variables:

* `TM_SELECTED_TEXT`: The currently selected text, if any; otherwise an empty string. (This variable will always be empty when you expand a snippet via <kbd class="platform-all">Tab</kbd>, but can hold text if a snippet is expanded via other means, like as part of a command.)
* `TM_CURRENT_WORD`: The contents of the word that the cursor is touching, if any; otherwise an empty string. (This variable will always contain all or part of the snippet prefix when you expand a snippet via <kbd class="platform-all">Tab</kbd>, but can hold different text if a snippet is expanded via other means, like as part of a command.)
* `TM_CURRENT_LINE`: The contents of the current line of text.
* `TM_LINE_NUMBER`: The number of the current line. (`1` is the number of the first line.)
* `TM_LINE_INDEX`: The index of the current line. (`0` is the index of the first line.)
* `TM_FILENAME`: The filename of the current document without its path.
* `TM_FILENAME_BASE`: The filename of the current document without its path _or_ file extension.
* `TM_DIRECTORY`: The full path of the current file without the filename.
* `CLIPBOARD`: The contents of the clipboard.
* `BLOCK_COMMENT_START`: The beginning of a block comment in the current language.
* `BLOCK_COMMENT_END`: The end of a block comment in the current language.
* `LINE_COMMENT`: The character(s) that start a line comment in the current lange.
* `CURRENT_YEAR`: The current four-digit year.
* `CURRENT_YEAR_SHORT`: The last two digits of the current year.
* `CURRENT_MONTH`: The current month as a two-digit, zero-padded number (between `01` and `12`).
* `CURRENT_MONTH_NAME`: The full name of the month in English (for example: `November`).
* `CURRENT_MONTH_NAME_SHORT`: The abbreviated name of the month in English (for example: `Nov`).
* `CURRENT_DATE`: The day of the month as a two-digit, zero-padded number (for example: `07` or `31`).
* `CURRENT_DAY_NAME`: The day of the week in English (for example: `Wednesday`).
* `CURRENT_DAY_NAME_SHORT`: The abbreviated day of the week in English (for example: `Wed`).
* `CURRENT_HOUR`: The current hour in 24-hour clock format (for example: `09`, `22`).
* `CURRENT_MINUTE`: The current minute (for example: `01`, `58`).
* `CURRENT_SECOND`: The current second (for example: `09`, `42`).

{{#note}}

Some of these variables begin with `TM_` because that's how they were named in TextMate, the editor that originated most of the snippet syntax that editors have standardized around.

{{/note}}

##### Transforms

Transforms are a powerful feature that allow you to manipulate text from variables or from other tab stops.

###### Transformed Tab Stops

Consider the following snippet:

```
ORIGINAL:    $1
TRANSFORMED: ${1/./-/g}
```

The syntax on line 2 is reminiscent of the replacement syntax from `sed` or `perl`. It transforms the input text in much the same way you'd transform text via your favorite language's `replace` or `gsub` method for strings. Here are the four parts to that syntax:

* `1`: The tab stop whose text is treated as the input.
* `.`: A regular expression matching part of the text — in this case, any (or each) character in the input.
* `-`: A literal hyphen to be used as the replacement for the input.
* `g`: A "global" regular expression flag — in this case, we want _each_ match in the input to get replaced, not just the first.

So if you triggered this snippet and then typed `something`, you'd get:

```
ORIGINAL:    something
TRANSFORMED: ---------
```

Despite its name, a transformed tab stop isn't actually a tab stop — it just uses a tab stop as its source of input. So it won't get "visited" by your cursor as you move through the snippet. In the above example, the transformed tab stop appeared after its source — tab stop 1 — but transformed tab stops can appear anywhere in a snippet body.

Consider this snippet:

```
<${1:div}></${1/[ ]+.*$//}>
```

Don't look too closely at the regular expression yet; just look at the parts of the snippet. We've got one tab stop and one transformed tab stop, and it appears to be the skeleton of an HTML element. So whatever we type up front for the opening tag will get transformed somehow for the closing tag.

This is what it looks like when you expand it.

![Example: Snippet Transform](../../images/snippets-example-transform-1.png "Example: Snippet Transform")

We can type a completely different tag name, and so far the output doesn't even get changed…

![Example: Snippet Transform](../../images/snippets-example-transform-2.png "Example: Snippet Transform")

…but what if you decide to add an attribute?

![Example: Snippet Transform](../../images/snippets-example-transform-3.png "Example: Snippet Transform")

Aha! Now the pattern in the transform makes more sense: it matches everything after the first space in the input. So the purpose of the transform is to mirror the tag name you typed, but not any of the attributes.

##### Transformed Variables

An identical syntax can be used to transform variables.

```
The extension of the current file is: ${TM_FILENAME/^(.*)\\.//}
```

Here are the four parts of this usage:

* `TM_FILENAME`: The variable whose value is treated as the input.
* `^(.*)\\.`: A regex that describes any number of characters at the beginning of the string, ending with a literal dot. (Why two backslashes? The regex needs a backslash, since a dot is a special character; but the snippet body is a string, so it needs `\\` to represent a literal backslash.)
* The empty string between the last two slashes means that we're replacing matches with nothing; they're just being discarded.
* The empty string after the trailing slash means that no regular expression flags are needed for this match.

Expand it and here's what you get:

```
The extension of the current file is: md
```

(Well, if you're typing inside of a Markdown file right now, the way I am, that's what you'll get.)

##### Named Transforms

In addition to transforms that use the triple-slash, `sed`-style syntax, both tab stops and variables can make use of certain named transforms that employ common string conversions:

```
ORIGINAL:    $1
TRANSFORMED: ${1/upcase}
```

This does what you'd expect — expanding this snippet and typing `something` would produce:

```
ORIGINAL:    something
TRANSFORMED: SOMETHING
```

The built-in named transforms are as follows:

* `/upcase`: Converts input to all-uppercase.
* `/downcase`: Converts input to all-lowercase.
* `/capitalize`: Converts the first character of the input to uppercase, otherwise leaving it untouched.

It is possible for other packages to define new named transforms; consult the Snippet module's documentation to learn more.

#### More Info

The snippets functionality is implemented in the [snippets](https://github.com/atom/snippets) package.

For more examples, see the snippets in the [language-html](https://github.com/atom/language-html/blob/master/snippets/language-html.cson) and [language-javascript](https://github.com/atom/language-javascript/blob/master/snippets/language-javascript.cson) packages.
