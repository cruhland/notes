
# [HTML Living Standard (WHATWG spec)](https://html.spec.whatwg.org/multipage/)

## Summary of contents

1. [**Introduction**](#introduction): informative overview
1. [**Common infrastructure**](#common-infrastructure): definitions
   used throughout the spec
1. [**Semantics, structure, and APIs of HTML documents**](#semantics-structure-and-apis-of-html-documents):
   the core concepts
1. [**The elements of HTML**](#the-elements-of-html): detailed
   definition of all tags
1. **Microdata**: labeling key-value data in a document
1. [**User interaction**](#user-interaction)
1. **Loading Web pages**: how browsers should handle navigation between
   pages
1. [**Web application APIs**](#web-application-apis): useful info for
   building web apps
1. **Communication**: sending and receiving asynchronous messages
1. **Web workers**: background (async) scripting
1. **Web storage**: mechanisms for client-side data persistence
1. [**The HTML syntax**](#the-html-syntax): rules for writing and
   parsing HTML
1. **The XHTML syntax**: HTML in XML
1. **Rendering**: recommendations for display
1. **Obsolete features**
1. **IANA considerations**: filetype specs
1. **Index**: useful reference tables
1. **References**: external supporting documents
1. **Acknowledgements**

## Introduction

### Where does this specification fit?

- Specs I definitely want to read: JavaScript, DOM, CSS, HTTP
- Specs I might want to read: SVG, WebIDL, MathML, Unicode, MIME, URL

### Audience

- Knowledge of DOM is required for a complete understanding of the
spec
- Helpful but optional knowledge: Web IDL, HTTP, XML, Unicode,
character encodings, JavaScript, CSS

### Scope

The spec is focused on specifying semantics of "normal" Web pages and
Web applications. Applications of HTML in areas that require
high-performance software are not covered. Customized presentation of
HTML documents is also not covered by the spec (but see the CSS
spec).

### History

Things seem to be a mess for Web specifications. The W3C stopped
evolving HTML in the late 1990s and early 2000s, and instead focused
on a replacement technology, called XHTML. But of course, the Web
itself never adoped XHTML in any real way, and browser vendors (Opera,
Mozilla, and Apple) split off from the W3C in 2004 to form the WHATWG,
which started improving the HTML spec once again.

Eventually the W3C got on board with the new HTML spec and worked with
the WHATWG for a while, but in 2011 the efforts split again. The W3C
wanted to create a final spec for HTML5, while the WHATWG preferred to
continuously maintain a "Living Standard".

In my opinion, given the immaturity of the Web, the WHATWG has the
right approach. Perhaps there needs to be a more formal way of saying
which parts of the standard an implementation conforms to, but an
immutable spec with a definite version number is too rigid: no browser
will ever implement all of HTML5, or only HTML5, as the W3C spec
defines it. Rather than introduce artifical separations between
versions of a spec, a living standard seems more in line with the
practice of software development.

### Design notes

Due to the development history of HTML, the spec has many
inconsistencies and redundancies. Some will take a while to fix, and
some others will probably never be fixed, as they are too heavily
relied on to change now. Still, now that efforts at making better
specs are succeeding, some design goals are being pursued:

- Serializability of script execution: no script should be able to
  tell if other scripts are executing concurrently, to prevent Web
  developers from dealing with multithreaded programming.
- Compliance with other specs: if this cannot be achieved, the
  violations must be explicitly noted.
- Extensibility: there are a number of built-in mechanisms for
  extending the meaning of HTML documents, while still conforming to
  the spec.

### HTML vs XHTML

- HTML is used by most Web pages, and has lenient parsing so as to be
backwards-compatible with older documents.
- XHTML is a specific kind of XML that represents HTML documents. It
has very strict parsing requirements.
- The DOM is an in-memory representation of HTML.

Not all documents in one representation can be converted to documents
in other representations.

### How to read this specification

From this section, where producers are document authors and consumers
are things like Web browsers:

> Requirements on producers have no bearing whatsoever on consumers.

These notes will focus on the producer side since I'm interested in
writing Web pages, not implementing a browser.

### Privacy concerns

Some features of HTML can be used to identify individual users on the
Internet. This can be a good thing, such as using cookies to maintain
a user's session with a Web site. But it can also be a bad thing,
revealing demographic information that could be used against the
user. Features of HTML that could be used for fingerprinting are
marked in the spec with a fingerprint symbol. Note that this symbol is
not comprehensive, as almost anything can be used for fingerprinting
with enough analysis.

### A quick introduction to HTML

Good summary of the basics. One interesting statement:

> Since DOM trees are used as the way to represent HTML documents when
> they are processed and presented by implementations (especially
> interactive implementations like Web browsers), this specification
> is mostly phrased in terms of DOM trees, instead of the markup
> described above.

#### Writing secure applications with HTML

- Interactive applications created with HTML can have vulnerabilities
- Many potential attacks are through cross-origin actions
- Example: not validating and/or escaping user input
    - Attacks: cross-site scripting (XSS), SQL injection
    - If the input is displayed via HTML, and the input was itself
      HTML, then there are various ways for that HTML to execute a
      script.
- Example: cross-site request forgery (CSRF)
    - Forms can be submitted from other origins, impersonating a user
    - Prevent attacks by using hidden tokens or checking `Origin`
      headers
- Example: clickjacking
    - Tricking a user into peforming an unwanted action
    - Can be done by putting the dangerous interface in an iframe
    - Prevent by having your site become inactive if rendered in an
      iframe

#### Common pitfalls to avoid when using the scripting APIs

Make sure elements and their event handlers are created in the same
script, so that the event is always caught. Otherwise, an event might
fire after an element is created but before its handler is added.

#### How to catch mistakes when writing HTML: validators and conformance checkers

See list at https://validator.whatwg.org/.

### Conformance requirements for authors

- Presentational markup causes problems and has been removed from
HTML. Use CSS instead.
- Some syntax that would technically be valid has been designated
invalid to avoid causing problems for authors. Use a conformance
checker.

## Common infrastructure

### 2.1 Terminology

Definitions of terms used throughout the spec.

### 2.2 Conformance requirements

Describes what needs to be done to meet the specification, and how the
spec relates to other specs, including which terms are defined
elsewhere.

### 2.3 Case-sensitivity and string comparison

Defines precise terminology.

### 2.4 Common microsyntaxes

Precise definitions of the data types used in the spec, including how
to parse their serialized values.

### 2.5 URLs

Precise definitions of URL terminology and algorithms, such as
resolving URLs and dynamically changing document base URLs.

### 2.6 Fetching resources

Precise definitions and algorithms for retrieving remote data, and
related security concerns.

### 2.7 Common DOM interfaces

Basic rules and data types for the DOM.

### 2.8 Namespaces

Provides namespace URLs.

## Semantics, structure, and APIs of HTML documents

### 3.1 Documents

Details about the `Document` DOM interface.

### 3.2 Elements

Description of the common parts of all element definitions.

#### 3.2.1 Semantics

- Elements have defined semantics, which allows them to be processed
by automated tools to convey those semantics to the user. Document
authors, and user agent developers, must not use or present elements
in a way inconsistent with their semantics.
- Document authors must not use elements, attributes, or attribute
values in an invalid way, since this prevents HTML from being
extended.
- User agents must update their display of the document if the
document structure is changed dynamically.

#### 3.2.2 Elements in the DOM

Nodes in the DOM represent elements in the document, and also have the
same semantics as their elements do. The `HTMLElement` interface
(defined here) must be implemented by all DOM element nodes.

#### 3.2.3 Element definitions

Description of the element definition format used in section 4. This
section is short enough that I won't summarize it here, just read it
from the spec.

#### 3.2.4 Content models

The **content model** of an element defines what that element is
allowed to contain.

Elements are grouped into various **categories** based on shared
features. Here are the main categories (there are others). See the
spec for which elements belong to each category.

- _Metadata content_: elements that convey meaning about the document
itself.
- _Flow content_: elements that appear in the body of documents.
- _Sectioning content_: elements that delimit the where headings and
footers can be used.
- _Heading content_: elements that define section headers.
- _Phrasing content_: elements that appear in the text of the
document, including withing paragraphs.
- _Embedded content_: elements that insert resources into the
document.
- _Interactive content_: elements that the user can interact with.
- _Palpable content_: seems to be elements that generally contain
meaningful data, with respect to the intent of the document.
- _Script-supporting elements_: elements that enable scripting
functionality.

_Transparent_ elements inherit their content models from their
parents.

A _paragraph_ is a contiguous sequence of phrasing content. There are
some subtleties in this definition that I won't describe here. The `p`
element can be used to make explicit paragraphs where there would
otherwise be no distinction between them. This can be especially
helpful when paragraphs would otherwise overlap, such as with fallback
content.

#### 3.2.5 Global attributes

The _global attributes_ can be specified on any HTML element. There
are also a collection of _event handler content attributes_ that can
be specified on any HTML element; they provide JavaScript code that
runs when the event they designate occurs on the element. Any
attribute starting with the string `data-` is considered a _custom
data attribute_ and can be placed on any HTML element.

The following _global attributes_ are described in more detail in this
section of the spec. I'll summarize only the ones that are interesting
to me for now.

- `id`: An identifier for the element. It must be unique in the
document.
- `title`: Provides _advisory information_ for the element. This is
what creates a tooltip in browsers. Most browsers aren't very good
about making this data accessible, though.
- `lang`, `xml:lang`: Specifies the language of the element's contents.
- `translate`: Specifies whether translation should be done on this
element's contents during localization.
- `xml:base`
- `dir`: Specifies text directionality (e.g., right-to-left).
- `class`: Specifies a space-separated set of CSS classes this element
belongs to. In the DOM, use `className` or `classList` to access
this attribute.
- `style`: Specifies CSS for this element. Use is discouraged.

The _custom data attributes_ are intended for private use by the
document and its scripts. They are not intended to be processed by
generic external tools. In the DOM, all of these attributes can be
accessed with the `dataset` member.

#### 3.2.6 Requirements relating to the bidirectional algorithm

Ignoring this section.

#### 3.2.7 Annotations for assistive technology products (ARIA)

Ignoring this section.

## 4 The elements of HTML

### 4.1 The root element

Describes features of the `html` element. (TODO more info)

### 4.2 Document metadata

Covers the following elements:

- `head` (TODO)
- `title` (TODO)
- `base` (TODO)
- `link` (TODO)
- `meta` (TODO)
- `style` (TODO)

### 4.3 Sections

Covers the following elements:

- `body` (TODO)
- `article` (TODO short)
- `section` (TODO short)
- `nav` (TODO short)
- `aside` (TODO short)
- `h1`-`h6` (TODO short)
- `hgroup` (TODO short)
- `header` (TODO short)
- `footer` (TODO short)
- `address`

There's also a lengthy set of examples about how to use these elements
appropriately.

### 4.4 Grouping content

Covers the following elements:

- `p` (TODO)
- `hr` (TODO short)
- `pre` (TODO)
- `blockquote` (TODO short)
- `ol` (TODO)
- `ul` (TODO)
- `li` (TODO)
- `dl` (TODO)
- `dt` (TODO)
- `dd` (TODO)
- `figure` (TODO short)
- `figcaption` (TODO short)
- `main` (TODO short)
- `div` (TODO)

### 4.5 Text-level semantics

Covers the following elements:

- `a` (TODO short)
- `em` (TODO short)
- `strong` (TODO short)
- `small` (TODO short)
- `s` (TODO short)
- `cite` (TODO)
- `q` (TODO)
- `dfn` (TODO short)
- `abbr` (TODO short)
- `ruby`
- `rt`
- `rp`
- `data` (TODO short)
- `time` (TODO short)
- `code` (TODO short)
- `var` (TODO)
- `samp` (TODO)
- `kbd` (TODO)
- `sub`, `sup` (TODO short)
- `i` (TODO short)
- `b` (TODO short)
- `u` (TODO short)
- `mark` (TODO short)
- `bdi`
- `bdo`
- `span` (TODO)
- `br` (TODO)
- `wbr` (TODO)

A
[usage summary](https://html.spec.whatwg.org/multipage/semantics.html#usage-summary)
is provided at the end of this section.

### 4.6 Links

TODO Short summary

### 4.7 Edits

TODO Short summary

### 4.8 Embedded content

TODO Short summary

TODO MathML

### 4.9 Tabular data

TODO In full

### 4.10 Forms

TODO Short summary

### 4.11 Interactive elements

TODO Short summary

TODO Commands, specifically for buttons

### 4.12 Scripting

TODO Subsections 1-3

### 4.13 Common idioms without dedicated elements

TODO Outline subsections

### 4.14 Disabled elements

Specifies which elements can be _actually disabled_.

### 4.15 Matching elements using selectors

Details about various ways HTML elements can be found using
selectors.

## User interaction

TODO read in full

## Web application APIs

TODO outline, then fill in details as needed

## The HTML syntax

TODO read non-parsing sections
