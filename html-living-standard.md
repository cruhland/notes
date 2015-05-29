
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

TODO outline, then fill in details as needed

## Semantics, structure, and APIs of HTML documents

TODO mostly read

## The elements of HTML

TODO outline, then fill in details as needed

## User interaction

TODO read in full

## Web application APIs

TODO outline, then fill in details as needed

## The HTML syntax

TODO read non-parsing sections
