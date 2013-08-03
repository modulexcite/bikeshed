Quick-Start Guide
=================

Starting from an empty file, do the following:

1. Add an `<h1>` with the spec's title as the very first line.
2. Add a `<pre class='metadata'>` block, with at least the following keys (each line in the format "[key]:[value]"):
	1. "Status" - the shortcode for the spec's status (ED, WD, UD, etc.)
	2. "ED" - link to the Editor's Draft
	3. "Shortname" - the spec's shortname, like "css-flexbox".
	4. "Level" - an integer for the spec's level.  If you're unsure, just put "1".
	5. "Editor" - an editor's personal information, in the format "[name], [company], [email or contact url]".
	6. "Abstract" - a short (one or two sentences) description of what the spec is for.
3. You *should* add an `<h2>Introduction</h2>` section.
4. Write the rest of the spec!

The processor expects that your headings are all `<h2>` or higher (`<h1>` is reserved for the spec title).
You probably want to add explicit id attributes to your headings;
the processor will add them for you if you don't,
but they're autogenerated from the text of the heading,
and often quite bad.

You don't need to use `<p>` tags in most cases -
plain paragraphs are automatically inferred by linebreaks,
and starting one with "Note:" makes it add `class="note"`.

Linking
-------

When you first download the processor, it'll come with the necessary crossref data needed to do cross-spec linking,
but there's no telling how recent it is.
You probably want to start by running `preprocess --update-cross-refs`, which'll fetch the latest data.

To use autolinks, just define things with `<dfn>`,
then link to them with `<a>` (no `href` attribute).
It matches up the contained text by default,
which can be overridden by using `title` on either the `<dfn>` or `<a>`.
Definitions and autolinks have a few extra attributes that you can specify;
check out the details in the [Definitions and Linking](definitions-autolinks.md) doc.

There are a few textual shortcuts to use as well:
* `'foo'` (apostophes/straight quotes) is an autolink to a property or descriptor named "foo"
* `''foo''` (double apostrophes) is an autolink to any of the CSS definition types except property and descriptor
* `<<foo>>` is an autolink to a type/production named "&lt;foo>"
* `[[foo]]` is an autolink to a bibliography entry named "foo", and auto-generates an informative reference in the biblio section.
    Add a leading exclamation point to the value, like `[[!foo]]` for a normative reference.

When you first preprocess, there's a good chance you'll get several errors.
While the preprocessor is forgiving about most things and will gladly to automatic fixup,
there are some things that it needs to be strict about to ensure it can generate correct documents.
In particular, all of your "value" type definitions *must* have a `for=''` attribute naming what they're a value for:
the name of the property they're a value of, the name of the type they're defined as a part of, etc.
(Alternately, put a `dfn-for=''` attribute on a container to have it apply to all the definitions without an explicit `for`.)
Similarly, some of your autolinks may throw errors or warnings because they can't tell which thing you're referring to.
Just add `for=''` and/or `spec=''` attributes to the link.

Defining Properties and Descriptors
-----------------------------------

If defining a property/descriptor, rather than writing out the table markup explicitly, just add a propdef or descdef block, like so:

~~~~html
<pre class='propdef'>
Name: var-*
Values: [ <value> | <CDO> | <CDC> ]
Initial: (nothing, see prose)
Applies To: all elements
Inherited: yes
Computed Value: specified value with variables substituted (but see prose for "invalid variables")
Media: all
</pre>
~~~~