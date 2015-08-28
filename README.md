HHI Definitions For PSR7, HTTP Messages
=======================================

This repository contains .hhi files for the interfaces defined in PSR-7.
It is based on the PHP interface definitions and comments available here:

https://github.com/php-fig/http-message

HHI files are ignored by HHVM, however they give the typechecker information
about the interfaces. This allows usage of the canonical PHP interface at
runtime, while also giving the benefits of static type checking.

Notes
-----

- The return value of `StreamInterface::seek()` is undefined in PSR7. This
  project defines it as `void` to avoid users depending on a non-standard
  return value.
- The return value of `StreamInterface::rewind()` is undefined in PSR7. This
  project defines it as `void` to avoid users depending on a non-standard
  return value.
- `RequestInterface::getRequestTarget()` is marked as `@return string`, however
  this is not always accurate, given that
  `RequestInterface::withRequestTarget()` explicitly allows any type for the
  request target, which must be preserved verbatim.
