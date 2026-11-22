---
tags:
  - Theory
---
[https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME\_types](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types)

A media type (also known as a Multipurpose Internet Mail Extensions or MIME type) indicates the nature and format of a document, file, or assortment of bytes. MIME types are defined and standardized in IETF's RFC 6838\.

The Internet Assigned Numbers Authority (IANA) is responsible for all official MIME types, and you can find the most up-to-date and complete list at their Media Types page.

# Structure of a MIME type

A MIME type most commonly consists of just two parts: a type and a subtype, separated by a slash (/) — with no whitespace between:

```
type/subtype
```

* Type \= general category into which the data type falls, such as video or text.  
* Subtype \= exact kind of data of the specified type the MIME type represents. For example, for the MIME type text, the subtype might be plain (plain text), html (HTML source code), or calendar (for iCalendar/.ics) files.

An optional parameter can be added to provide additional details:

```
type/subtype;parameter=value
```

For example, for any MIME type whose main type is text, you can add the optional charset parameter to specify the character set used for the characters in the data. If no charset is specified, the default is ASCII (US-ASCII) unless overridden by the user agent's settings. To specify a UTF-8 text file, the MIME type text/plain;charset=UTF-8 is used.

