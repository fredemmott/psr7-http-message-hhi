HHI Definitions For PSR7, HTTP Messages
=======================================

This repository contains .hhi files for the interfaces defined in PSR-7.
It is based on the PHP interface definitions and comments available here:

https://github.com/php-fig/http-message

HHI files are ignored by HHVM, however they give the typechecker information
about the interfaces. This allows usage of the canonical PHP interface at
runtime, while also giving the benefits of static type checking.

Installation
------------

```
composer require hack-psr/psr7-http-message-hhi
```

Notes
-----

- `RequestInterface::getRequestTarget()` is marked as `@return string`, however
  this is not always accurate, given that
  `RequestInterface::withRequestTarget()` explicitly allows any type for the
  request target, which must be preserved verbatim - https://github.com/php-fig/http-message/issues/67
- `ServerRequestInterface::getParsedBody()` is specified as returning
  `null|array|object` so is typed here as `mixed`. There has been some
  discussion about banning the `object` case as any usage couples you to a
  specific implementation or stack, which would allow it to be typed as
  `?array<string,mixed>`. This is not done because PHP implementations would
  not have to honor this type.
- `ServerRequestInterface::getQueryParams()` and `getUploadedFiles()` return
  untyped arrays as they return a tree-like structure instead of a
  key-value structure - eg `?a[b][c]=123`. They could be typed as
  `array<string,mixed>`, however this would make them much more
  inconvenient to use.
- The return value of these functions is not defined in PSR7; this project
  defines them as returning `void` to prevent users depending on unspecified
  behavior (https://github.com/php-fig/http-message/issues/68):
   - `StreamInterface::seek()`
   - `StreamInterface::rewind()`
   - `UploadedFileInterface::moveTo()`
- The combination of these means that the most user-friendly (and safe) hack definition of this interface isn't suitable for use by implementations (see #2)

Future Work
-----------

Create a derived standard for Hack, addressing the above notes. Ideas
include:

 - banning non-string request targets
 - banning `object` parsed bodies
 - flattening `getUploadedFiles`, `getParsedBody`, `getQueryParams`

This would be a separate project.
