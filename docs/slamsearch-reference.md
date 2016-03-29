---
layout: support
title:  "SlamSearch Reference"
---

SlamData gives you the ability to make search-engine-style searches into your data
using a syntax called SlamSearch. Click the search icon to run searches
![Search icon](/images/screenshots/front-end-search-icon.png)

**Note: In general, searches are not case-sensitive.**

This reference contains the following sections:

* [Simple searches](#simple)
* [Specifying field names](#fields)
* [Numeric searches](#numeric)
* [String searches](#string)


## Simple searches



### Search for a String

To search for everything that contains a string, simply use that string. Alternatively,
you can prefix it with a plus sign (`+`).

Examples:

Search for any data with the text "open".

    open

or

    +open

### Exclude a String

You can exclude strings by prefixing them with a minus sign (`-`).

Example:

Search for any data with the text "open", but not "test".

    open -test



## Specifying Field Names

Specify a field name by having the field name followed by a colon (`:`).
To indicate a field within a field, separate the nested fields with colons.

Examples:

Search for field names of "status" that contain the value "closed":

    status:closed

Search for field names of "user" which have a field called "name" which
contains the value "smith":

    user:name:smith




## Numeric Searches

Use greater than and less than signs to return search results that are
greater than or less than a number. Use two periods (`..`) to indicate a
range. Use the minus sign (`-`) to return exceptions to the comparison.

Examples:

** DOESN'T WORK**

Search for any data greater than 10,000:

    >10000

Search for data where the value of field "age" is greater than 30:

    age:>30

Search for data where the value of field "age" is between 30 and 40:

    age:30..40

Search for data where the value of field "age" is *not* between
30 and 40:

    -age:30..40    


## String Searches

Use the asterisk (`*`) to indicate a wild card.

Example:

Search for data starting with "new".

    new*
