---
title: Date and Time on the Internet
---

# Date and Time on the Internet

### Ujjwal Sharma (ryzokuken@igalia.com)
### IETF 110, March 2021

---

## Wait, who are you again?

* ECMA TC39
  * Standardizing ~ECMAScript~ JavaScript
  * Google, Microsoft, Bloomberg, PayPal, …
* Temporal Champions Group
  * Standardizing `Date`
  * Modern, ergonomic API for building date/time applications
  * JavaScript veterans, Internationalization experts, …

---

## So what’s so special about Temporal anyway?

* Modern, ergonomic API
* Addresses long-standing weaknesses of `Date`
* Use RFC 3339 as base interchange format
* First-class time zone support
* First-class calendar support
* Timezones and calendars in the data model
* How does one persist that?

---

## 2021-03-04T02:32:09+05:30

---

## The Time Zone Conundrum

* By far not the first to uncover this problem
* RFC3339 and ISO8601 allow absolute offsets from UTC
* Many applications work in the context of a “human” time zone
* Need to encode in the timestamp for persistence or interchange
* Databases? Round Tripping?
* What is a “human time zone”? IANA? Unicode?
* Java, Linux, Databases, Calendars

---

## 2021-03-04T02:32:09+05:30 IST

---

## 2021-03-04T02:32:09+05:30 Asia/Kolkata

---

## 2021-03-04T02:32:09+05:30[Asia/Kolkata]

---

## Phew! We’re done, right?

* Not quite
* The format was never standardized
* More information that can be encoded
* For Temporal, calendars are a priority
* More? CLDR Timezones? Numbering Systems?
* Need for a generalized format
* Process to specify keys and acceptable values

---

## 2021-03-04T02:32:09+05:30[Asia/Kolkata][x-foo=bar]

---

![](./persistence-model.svg)

---

## [draft-ryzokuken-datetime-extended](https://github.com/ryzokuken/draft-ryzokuken-datetime-extended)

* Share our observations with IETF
* Keep folks from CalConnect, ISO in the loop
* Standardize generalized, optional extensions
* Modernize RFC 3339, in sync with ISO 8601
  * Extended years syntax
  * Deprecating two/three-digit years
* Standardize a mechanism for registering keys
* Work with Unicode in parallel regarding the `u` namespace

---

## How do we move ahead with this?

* Brought our findings to CALEXT, CalConnect
* Authored a draft, aiming to obsolete RFC 3339
* Included updates and optional extensions
* Some pushback to obsoleting RFC 3339
* Updates and extensions separated into distinct drafts
* Can both be adopted? By which WGs?
* Do we need a new WG?

---

## Thank You!
