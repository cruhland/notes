
# [DOM Living Standard](https://dom.spec.whatwg.org/)

## Goals

Explains how the document standardizes the DOM.

## 1 Conformance

Specific requirements for implementations to be conforming. A summary:

- Normativity
    - All diagrams, examples, and notes in the document are
    non-normative
    - All sections explicitly stated as non-normative are
    non-normative
    - Everything else is normative
- This specification conforms to RFC 2119
- Imperative statements in algorithms should be read as including the
same RFC 2119 keyword that was used to introduce the algorithm.
- Algorithms may be implemented in any way, as long as the same
results are produced
- Implementations may impose limits on inputs not specified to have
them
- Calls to methods or attributes from user agent code must invoke an
internal API, rather than the user-facing JavaScript code, which can
be overridden.
- String comparison is case-sensitive.

## 2 Terminology

The _context object_ refers essentially to the _this_ value when
discussing a method or attribute. It's the object where the code
"lives".

### 2.1 Trees

Here's an attempt to define the terms in this section more concretely
using code and/or math.

```
tree: Type
obj: Type // an object that participates in a tree

data Nullable: (A: Type) -> Type where
  Some: (a: A) -> Nullable A
  Null: Nullable A

parent: obj -> Nullable obj

data List: (A: Type) -> Type where
  Nil: List A
  _::_: (head: A) -> (tail: List A) -> List A

children: obj -> List obj

data Bool: Type where
  True: Bool
  False: Bool

_isChildOf_: obj -> obj -> Bool
a isChildOf b = (parent a) == Some b

root: obj -> obj
root x = match (parent x) where
  Some p => root p
  Null => x

_||_: Bool -> Bool -> Bool
True || q = True
False || q = q

_isDescendantOf_: obj -> obj -> Bool
a isDescendantOf b = directChild || indirectChild where
  directChild = a isChildOf b
  indirectChild = match (parent a) where
    Some c => c isDescendantOf b
    Null => False

_isInclusiveDescendantOf_: obj -> obj -> Bool
a isInclusiveDescendantOf b = (a == b) || (a isDescendantOf b)

_isAncestorOf_: obj -> obj -> Bool
a isAncestorOf b = b isDescendantOf a

_isInclusiveAncestorOf_: obj -> obj -> Bool
a isInclusiveAncestorOf b = (a == b) || a isAncestorOf b

_isSiblingOf_: obj -> obj -> Bool
a isSiblingOf b = match (parent a, parent b) where
  (Some pa, Some pb) => pa == pb
  _ => False

root: tree -> obj

treeOrder: tree -> List obj
treeOrder t = treeOrder (root t)

_++_: List a -> List a -> List a
Nil ys = ys
(x :: xs) ys = x :: (xs ++ ys)

flatMap: List a -> (a -> List b) -> List b
flatMap Nil _ = Nil
flatMap (x :: xs) f => (f x) ++ (xs flatMap b)

treeOrder: obj -> List obj
treeOrder x = x :: ((children x) flatMap treeOrder)

indexOf: a -> List a -> Nullable Integer
indexOf x ys = accumIndexOf x ys 0

accumIndexOf: a -> List a -> Integer -> Nullable Integer
accumIndexOf _ Nil _ => Null
accumIndexOf a (x :: xs) i =>
  if a == x then Some i else accumIndexOf a xs (i + 1)

isBefore: a -> a -> List a -> Bool
isBefore a b xs = match (indexOf a xs, indexOf b xs) where
  (Some i, Some j) => i < j
  _ => False

isAfter: a -> a -> List a -> Bool
isAfter a b xs = match (indexOf a xs, indexOf b xs) where
  (Some i, Some j) => i > j
  _ => False

_preceding_: obj -> obj -> Bool
a preceding b = isBefore a b (treeOrder (root a))

_following_: obj -> obj -> Bool
a following b = isAfter a b (treeOrder (root a))

firstChild: obj -> Nullable obj
firstChild x = first (children x)

lastChild: obj -> Nullable obj
lastChild x = last (children x)

first: List a -> Nullable a
first xs = match xs where
  Nil => Null
  (x :: _) => Some x

last: List a -> Nullable a
last xs = match xs where
  Nil => Null
  (x :: Nil) => Some x
  (_ :: tail) => last tail

before: a -> List a -> List a
before x Nil => Nil
before x (y :: ys) => if x == y then Nil else y :: before x ys

after: a -> List a -> List a
after x Nil = Nil
after x (y :: ys) = if (x == y) ys else after x ys

previousSibling: obj -> Nullable obj
previousSibling x = last (before x (siblings x))

nextSibling: obj -> Nullable obj
nextSibling x = first (after x (siblings x))

length: List a -> Integer
length Nil = 0
length (x :: xs) = 1 + length xs

index: obj -> Integer
index x = length (before x (siblings x))
```

### 2.2 Strings

### 2.3 Ordered sets

### 2.4 Namespaces

## 3 Events

## 4 Nodes

## 5 Ranges

## 6 Traversal

## 7 Sets

## 8 Historical

## Acknowledgements

## Index

## References

## IDL Index
