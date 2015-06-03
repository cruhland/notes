
# [Web IDL draft standard](http://heycam.github.io/webidl/)

## 1 Introduction

## 2 Conformance

## 3 Interface definition language

### 3.1 Names

### 3.2 Interfaces

We can relate an interface to a `trait` in Scala:

```webidl
interface Example {
  // interface members...
};
```

```scala
trait Example {
  // interface members...
}
```

Inheritance of interfaces can be mapped to trait inheritance:

```webidl
interface InheritanceExample : Example {
  // interface members...
}
```

```scala
trait InheritanceExample extends Example {
  // interface members...
}
```

#### 3.2.1 Constants

#### 3.2.2 Attributes

A _regular attribute_ in WebIDL corresponds to a mutable field in
Scala:

```webidl
attribute Type identifier;
```

```scala
var identifier: Type
```

A _read only_ attribute in WebIDL corresponds to an immutable field in
Scala:

```webidl
readonly attribute Type identifier;
```

```scala
val identifier: Type
```

#### 3.2.3 Operations

A _regular operation_ in WebIDL corresponds to a method in Scala:

```webidl
ReturnType identifier(arguments...);
```

```scala
def identifier(arguments...): ReturnType
```

The return type `void` in Web IDL corresponds to the type `Unit` in
Scala.

Each argument to an operation is given in Web IDL as `ArgType
argName`. The equivalent in Scala is `argName: ArgType`.

In Web IDL, an _optional argument_ with a _default value_ is specified
as `optional ArgType argName = argValue`; in Scala, it is specified as
`argName: ArgType = argValue`.

#### 3.2.4 Special operations

### 3.3 Dictionaries

### 3.4 Exceptions

### 3.5 Enumerations

### 3.6 Callback functions

### 3.7 Typedefs

### 3.8 Implements statements

### 3.9 Objects implementing interfaces

### 3.10 Types

#### 3.10.1 any

#### 3.10.2 boolean

The Web IDL values of this type are **true** and **false**.

The equivalent Scala type is `Boolean`, with corresponding `true` and
`false` values.

#### 3.10.3 byte

#### 3.10.4 octet

#### 3.10.5 short

#### 3.10.6 unsigned short

#### 3.10.7 long

The Web IDL values of this type are integers _n_ such that
-2,147,483,648 ≤ _n_ ≤ 2,147,483,647. The equivalent Scala type is
`Int`.

#### 3.10.8 unsigned long

The Web IDL values of this type are integers _n_ such that 0 ≤ _n_ ≤
4,294,967,295. There is no directly equivalent Scala type (all numeric
primitive types in Scala are signed), but a `Long` should be used
because it covers the entire range of values of this type.

#### 3.10.9 long long

#### 3.10.10 unsigned long long

#### 3.10.11 float

#### 3.10.12 unrestricted float

#### 3.10.13 double

#### 3.10.14 unrestricted double

#### 3.10.15 DOMString

#### 3.10.16 ByteString

#### 3.10.17 USVString

#### 3.10.18 object

#### 3.10.19 Interface types

#### 3.10.20 Dictionary types

#### 3.10.21 Enumeration types

#### 3.10.22 Callback function types

#### 3.10.23 Nullable types — _T_?

In Web IDL, given a type **T**, the nullable version of that type is
**T?**. It has the same values as **T**, with the additional value
**null**. The Scala equivalent of **T?** is `Option[T]`, and the
equivalent of **null** is `None`. A non-null value **t** of type
**T?** in Web IDL corresponds to `Some(t)` in Scala, where `t` is the
Scala equivalent of **t**.

#### 3.10.24 Sequences — sequence<_T_>

#### 3.10.25 Arrays — _T_[]

#### 3.10.26 Promise types — Promise<_T_>

#### 3.10.27 Union types

#### 3.10.28 Date

#### 3.10.29 RegExp

#### 3.10.30 Error

#### 3.10.31 DOMException

#### 3.10.32 Buffer source types

### 3.11 Extended attributes

## 4 ECMAScript binding

## 5 Common definitions

## 6 Extensibility

## 7 Referencing this specification

## 8 Acknowledgements

## A IDL Grammar

## B References

## C Changes
