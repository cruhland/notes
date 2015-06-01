
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

#### 3.2.4 Special operations

### 3.3 Dictionaries

### 3.4 Exceptions

### 3.5 Enumerations

### 3.6 Callback functions

### 3.7 Typedefs

### 3.8 Implements statements

### 3.9 Objects implementing interfaces

### 3.10 Types

### 3.11 Extended attributes

## 4 ECMAScript binding

## 5 Common definitions

## 6 Extensibility

## 7 Referencing this specification

## 8 Acknowledgements

## A IDL Grammar

## B References

## C Changes
