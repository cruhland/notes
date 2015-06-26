
# Generic notes on Web technologies

## JavaScript 6

### `Array.prototype.push(...items)`

The given `items` are concatenated at the end of the array, and the length of the result is returned.

## The DOM

In this section I'll describe the DOM interfaces using Scala syntax, since I'm familiar with it.

```scala
/** Integral type. Min value is 0. Max value is 2<sup>32</sup>-1. */
type UnsignedInt

/** TODO */
type DOMString
```

```scala
case class DOMError(name: DOMString, message: DOMString = "")
```

```scala
/** Only available in the `Window` thread. */
trait HTMLCollection {
  /** The number of elements in this collection. */
  val length: UnsignedInt
  
  /** Return the element at the given index (in tree order), or `null` if no such element exists. */
  def item(index: UnsignedInt): Option[Element]
  
  /** Return the element at the given index (in tree order), or `undefined` if no such element exists. */
  def apply(index: UnsignedInt): Option[Element]
  
  // TODO incomplete
}
```
### [`ParentNode`](https://dom.spec.whatwg.org/#interface-parentnode)
```scala
/** This trait cannot have a companion object defined. This trait is only mixed-in to objects in
  * the main JavaScript thread, where `Window` is the global interface.
  */
trait ParentNode {
  /** The child `Element`s of this node. */
  val children: HTMLCollection
  
  /** The first child of this node that is an `Element`; `null` otherwise. */
  val firstElementChild: Option[Element]

  // TODO incomplete
}
```

### [`Node`](https://dom.spec.whatwg.org/#interface-node)

```scala
/** This trait is only available where `Window` is the global object. */
trait Node extends EventTarget {

  /** Add `node` as the last child of this node, and return it. */
  def appendChild(node: Node): Node
  
  // TODO incomplete
}
```

## Web IDL

### Extended attributes

- `[Exposed=<global-interface-set>]`: indicates that the members of the interface are only available when
code is executing with a global interface specified in `global-interface-set`. As an example,
`[Exposed=Window]` means that the interface is only availabe in the main thread, while `[Exposed=Worker]`
means that the inferface is only available in a web worker thread.
- `[NoInterfaceObject]`: essentially means that there is no concrete object in JavaScript with the name
of the interface. Such _interface objects_ are used to store constants and static methods for interfaces.
The recommended use of this attribute is on "supplemental" interfaces that just provide additional methods
for other interfaces (like mixins in Scala).
- `[Unscopeable]`: the labeled interface member cannot be referenced by its name alone when used in a `with`
statement on its context object. Probably done to prevent common programming mistakes.

## DOM events

The `input` event is fired whenever the state of a user input element is modified. The `change` event is fired
in slightly different ways depending on the particular type of user input, but the rough condition is when the
state of the element has been more permanently changed. For example, when a user is typing in a `text` input,
`input` events will be fired for each keystroke (approximately). The `change` event will only be fired when
the element loses focus, indicating that the user will not be making further edits.

## HTML elements

### The `input` element

The `input` element represents many different forms of user-provided data, distinguished by the `type` attribute.

#### File upload (`type=file`)

Allows the user to select one or more files. Each file has a name, type, and body (contents).

More than one file can be selected only if the `multiple` attribute is set.

Both the `input` and `change` events are fired at this element when its state changes.

The `accept` attribute is a hint about what kinds of files are acceptable. Its value is a comma-separated list
of unique tokens, where a token can be one of:

- `audio/*`, `video/*`, or `image/*`
- MIME type without parameters
- File extensions (e.g. `.txt`)

After the user has selected a file or files, the `files` DOM attribute provides access to a `FileList` of them.

## HTML File API

To read files, create an instance of `FileReader`:

```JavaScript
var fileReader = new FileReader();
```

Reading text files asynchronously is accomplished with `fileReader.readAsText(blob, label="utf-8")`, where `blob` is
a `Blob` instance (usually a `File`) and `label` is the expected encoding of the data.

After the read finishes successfully, the result can be obtained with `fileReader.result`, which will be a
`DOMString` of the file contents.

If the read fails, a `DOMError` describing the problem can be obtained with `fileReader.error`.

Instances of `ProgressEvent` are fired at `fileReader` to report on asynchronous reads:
- `load`: when a read completes successfully;
- `error`: when a read fails.

These events can be handled using the corresponding handler attributes on `fileReader`, e.g. `fileReader.onload`.

## Miscellaneous

### Saving client-side app data to local files from the browser

After Googling around for a bit, I found
[this blog post](http://eligrey.com/blog/post/saving-generated-files-on-the-client-side/). However, it's from
2011 and was likely to be out of date. Indeed, the W3C File API pages it linked to were deprecated. But the
[_current_ W3C File API](http://www.w3.org/TR/FileAPI/) had some promising information, specifically in
[Section 12, Requirements and Use Cases](http://www.w3.org/TR/FileAPI/#requirements):

> - User agents should provide the ability to save a local file programmatically given an amount of data
and a file name.

>> **Note**
>>
>> While this specification doesn't provide an explicit API call to trigger downloads, the HTML5
specification has addressed this. The `download` attribute of the `a` element
[[HTML](http://www.w3.org/TR/FileAPI/#HTML)] initiates a download, saving
a [`File`](http://www.w3.org/TR/FileAPI/#dfn-file) with the name specified. The combination of this API and the
`download` attribute on `a` elements allows for the creation of files within web applications, and the ability
to save them locally.

However, the `a` element needs a URL to download the data _from_. Poking around in the HTML spec for `a` led to
[RFC 2397](http://tools.ietf.org/html/rfc2397) on the `data:` URL scheme. This enables a kind of
["immediate"](http://programmedlessons.org/AssemblyTutorial/Chapter-11/ass11_2.html) hyperlink, where
the data is provided as part of the URL. Here's an example:

```html
<a href="data:text/plain,hello world" download="hello.txt">Download text</a>
```

That would render `Download text` as a normal hyperlink, except that clicking on it would cause your browser to
"download" a file with content `hello world` and attempt to save it with the name `hello.txt`. Depending on how
your browser is configured it might save it to a "Downloads" directory or ask you where you want to put it.
Unfortunately, it might not work in your browser as apparently not all of them support the `data:` URL scheme.

Linked from that initial blog post is the [FileSaver.js repo](https://github.com/eligrey/FileSaver.js), which
solves the problem in a cross-browser way. Although keep in mind that the `FileSaver` API is no longer a proposed
standard and so the library will not have any official support.

### Loading local files into the browser so they can be processed by JavaScript

This makes use of `FileReader` and related parts of the [W3C File API](http://www.w3.org/TR/FileAPI/).

```JavaScript
// Given an <input type="file"> element `input`:
input.addEventListener("change", function () {
    var file = input.files[0];
    if (!file) return;
    
    var fileReader = new FileReader();
    
    fileReader.onload = function () {
        processFileContents(fileReader.result);
    };
    fileReader.onerror = function () {
        var error = fileReader.error;
        alert("A " + error.name + " occurred when reading " + file.name + ": " + error.message);
    };
    
    fileReader.readAsText(file);
});
```
