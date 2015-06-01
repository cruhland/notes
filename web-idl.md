
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
