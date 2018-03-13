---
layout: post
title: document-ch-controlfields
date: 2018-03-13
tag: iOSre
site: https://zhangkn.github.io
---

### 前言

>* 本文重点说明Syntax of control files的three types of fields:simple、folded、multiline；
```
---因为一不小心就容易出错：missing final newline、Status: install ok half-configured、 EOF before value of field `Icon' (missing final newline)
 end of file during value of field 'Depiction' (missing final newline)
```
### 正文


>* simple
```
The field, including its value, must be a single line. 
```
>* folded
```
The value of a folded field is a logical line that may span several lines. The lines after the first are called continuation lines and must start with a space or a tab.
Whitespace, including any newlines, is not significant in the field values of folded fields.
```

>* multiline
```
The value of a multiline field may comprise multiple continuation lines. The first line of the value, the part on the same line as the field name, often has special significance or may have to be empty. Other lines are added following the same syntax as the continuation lines of the folded fields. Whitespace, including newlines, is significant in the values of multiline fields.
```


>* The fields in the general paragraph (the first one, for the source package) are:
```
Source (mandatory)
Maintainer (mandatory)
Uploaders
Section (recommended)
Priority (recommended)
Build-Depends et al
Standards-Version (recommended)
Homepage
Version Control System (VCS) fields
Testsuite
```
>* The fields in the binary package paragraphs are:
```
Package (mandatory)
Architecture (mandatory)
Section (recommended)
Priority (recommended)
Essential
Depends et al
Description (mandatory)
Homepage
Built-Using
Package-Type
```

### Sections

>* value
```
Section: System
Section: Tweaks
```


### see also

- [https://www.debian.org/doc/debian-policy/#document-ch-controlfields](https://www.debian.org/doc/debian-policy/#document-ch-controlfields)

```

<!-- 5.2. Source package control files – debian/control¶ -->
<!-- 7.6.2. Replacing whole packages, forcing their removal¶ -->
Second, Replaces allows the packaging system to resolve which package should be removed when there is a conflict (see Conflicting binary packages - Conflicts). This usage only takes effect when the two packages do conflict, so that the two usages of this field do not interfere with each other.

Provides: mail-transport-agent
Conflicts: mail-transport-agent
Replaces: mail-transport-agent
```