# md2metafilter

This is a simple bash script to convert some Markdown text to HTML suitable for
a [MetaFilter](https://www.metafilter.com/) post.

The idea is that you can have a file (it doesn't have to be a Markdown file but
it should be plain text) to collect links, notes, etc. and then assemble them
into a Markdown formatted post within the same file. When you are satisfied with
it, you can mark the post section with lines containing STARTCUT and STOPCUT,
run this script, and it will create an HTML formatted (with some MetaFilter
quirks) file of the post that you can copy and paste.

## Requirements:

It will run on macOS and GNU/linux.

The main requirement is [pandoc](https://pandoc.org/installing.html), which
handles the Markdown --> HTML conversion.

Also needed are a few things that are normally installed (e.g. `awk`.)
The script will tell you if any of those are missing.

## To use:

**Download the script.** On Github click on the Code button and either clone the
project or download and unpack the zip file.

**Make executeable.** In a terminal, navigate to the script and enter
`chmod +x md2metafilter`

**Run the script.** You can test it on this file: `./md2metafilter README.md`
It will create and open a text file with an HTML version of the Example text
below.

If you want it generally available, move the script to `/usr/local/bin`.

### What it does

The script will look for an input file on the command line. If it doesn't find
one it will check for a default file. This is set to
`Documents/MetaFilterDrafts.md` by default, but you can change this by editing
the script (see the note below.)

It will then cut out the part between "STARTCUT" and "STOPCUT" (each must be
alone on their own line.)

It will "smarten" (make curly) any quotes. One caveat is that it doesn't
understand leading apostrophes (as in **’til**) and will make them opening
single quotes instead.

It then converts the Markdown to HTML and writes to a file. This output file has
the same name as the input file, plus ".mefi.txt" at the end. For example, if
you use `README.md` as the input it will output `README.md.mefi.txt` (and
overwrite any previous file with that name.) By default it will be in the same
folder as the input file.

It then edits the file to add double spaces between paragraphs and remove the
`<p>` tags. This isn't strictly necessary, but it works on MetaFilter and it
makes it easier to read/edit the output.

Finally it tries to open the file in a text editor, so you can copy and paste
the resulting code into a MetaFilter post.

**Note:** You can easily customize the script, often by uncommenting alternate
versions of lines. See the "CONSTANTS" section on line 50. (If you have `nano`
open the script with `nano +50 md2metafilter`)

## Example:

This is the part the script will convert. You may want to look at this section
in a text editor and again in a "New Post" (draft!) on MetaFilter to see everything
that's happening.

STARTCUT

Title of Post (no markup allowed, but you can use emoji 😀)

This first paragraph is the "Description," or the part on the front page.
It needs at least [one link](https://www.youtube.com/watch?v=dQw4w9WgXcQ).

If there's 'more inside,' it goes in the "Extended Description". Note how the
quotes are "smartened." I generally just dump the whole post into the Extended
Definition to check on formatting etc.

MetaFilter treats newlines as `<br>`
(as does the Pandoc setting used.)
So you generally want to
separate paragraphs
with a blank line.



More than one blank line will get shortened to one.

Pandoc will convert the Markdown you throw at it ([Pandoc Flavor](https://pandoc.org/demo/example33/8-pandocs-markdown.html#pandocs-markdown) here,)
so remember what MetaFilter will allow. Generally you're good with inline tags
like **bold**, *italic*, ***bold italic***, ~~strike~~, and `code`.

1. Ordered lists are OK.
2. As are Unordered and nested lists
   * Just start the sub-list at least even with the first text in the parent.

Definition Lists
  ~ They work, at least with Pandoc Markdown.
  ~ They just look *really* weird.

Note the format. The kind that start with a colon or tilde on the *definition*
don't seem to work.

> Blocklists work fine.

Tables are no good on MetaFilter, so build plain text tables inside a code
fence:
```
| Tag    | OK? |
|--------|-----|
|<small> | Yes |
|<big>   | No  |

```

You can also use HTML tags directly, like <sub>Subscript</sub>, <sup>Superscript</sup>,
and <ins>Underline</ins>. Metafilter allows <small>small</small>, but not <big>big</big>.

Avoid HTML tags that use quotes, like `href`, since those will be 'smartened'
and break. There's usually a Markdown equivalent for those.



I like to keep the list of tags at the end. I don't fill in the "Tags" section
until I'm ready to post (to prevent accidental posting.)

TAGS: Example DontPostThis

STOPCUT

Nothing before the STARTCUT or after the STOPCUT will be converted, so you can
have notes before and after the post you're building.

However, the script will start and stop cutting again if you have them multiple
times! So if you use one file for several posts, remove the STARTCUT and STOPCUT
when you're done. I just change them to START and STOP so I can see where the
old posts began and ended.
