.. figure:: images/white-logo.png
   :alt: SlamData Logo

   SlamData Logo

Reference - SQL²
================

Introduction
------------

SQL2 is a subset of ANSI SQL, designed for queries into NoSQL databases.

SQL2 has support for every major SQL SELECT clause, such as ``AS``,
``WHERE``, ``JOIN``, ``GROUP BY``, ``HAVING``, ``LIMIT``, ``OFFSET``,
``CROSS``, etc. It also contains many standard SQL functions and
operators. It follows PostgreSQL where SQL dialects diverge.

--------------

Data Types
----------

The following data types are used by SQL2.

**Note:** > Some data types are not natively supported by all databases.
Instead, they are emulated by SlamData, meaning that you can use them as
if they were supported by the database.

**MDB** = Native MongoDB Support

**XYZ** = Native XYZ DB Support (example for future databases)

+----------+-----------------------------------+---------------------------------------+-----+-----+
| Type     | Description                       | Examples                              | MDB | XYZ |
+==========+===================================+=======================================+=====+=====+
| Null     | Indicates missing information.    | ``null``                              | Yes | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Boolean  | true or false                     | ``true``, ``false``                   | Yes | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Integer  | Whole numbers (no fractional      | ``1``, ``-2``                         | Yes | ??? |
|          | component)                        |                                       |     |     |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Decimal  | Decimal numbers (optional         | ``1.0``, ``-2.19743``                 | Yes | ??? |
|          | fractional components)            |                                       |     |     |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| String   | Text                              | ``"221B Baker Street"``               | Yes | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| DateTime | Date and time, in ISO8601 format  | ``TIMESTAMP("2004-10-19T10:23:54Z")`` | Yes | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Time     | Time in the format HH:MM:SS.      | ``TIME("10:23:54")``                  | No  | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Date     | Date in the format YYYY-MM-DD     | ``DATE("2004-10-19")``                | No  | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Interval | Time interval, in ISO8601 format  | ``INTERVAL("P3DT4H5M6S")``            | No  | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Object ID| Unique object identifier.         | ``OID("507f1f77bcf86cd799439011")``   | Yes | ??? |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Ordered  | Ordered list with no duplicates   | ``(1, 2, 3)``                         | No  | ??? |
| Set      | allowed                           |                                       |     |     |
+----------+-----------------------------------+---------------------------------------+-----+-----+
| Array    | Ordered list with duplicates      | ``[1, 2, 2]``                         | Yes | ??? |
|          | allowed                           |                                       |     |     |
+----------+-----------------------------------+---------------------------------------+-----+-----+

--------------

Clauses, Operators, and Functions
---------------------------------

The following clauses are supported:

+---------------+---------------------------------------------------------------------------------------+
| Type          | Clauses                                                                               |
+===============+=======================================================================================+
| Basic         | ``SELECT``, ``AS``, ``FROM``                                                          |
+---------------+---------------------------------------------------------------------------------------+
| Joins         | ``LEFT OUTER JOIN``, ``RIGHT OUTER JOIN``, ``INNER JOIN``, ``FULL JOIN``, ``CROSS``   |
+---------------+---------------------------------------------------------------------------------------+
| Filtering     | ``WHERE``                                                                             |
+---------------+---------------------------------------------------------------------------------------+
| Grouping      | ``GROUP BY``, ``HAVING``                                                              |
+---------------+---------------------------------------------------------------------------------------+
| Conditional   | ``CASE`` , ``WHEN``, ``DEFAULT``                                                      |
+---------------+---------------------------------------------------------------------------------------+
| Paging        | ``LIMIT``, ``OFFSET``                                                                 |
+---------------+---------------------------------------------------------------------------------------+
| Sorting       | ``ORDER BY`` , ``DESC``, ``ASC``                                                      |
+---------------+---------------------------------------------------------------------------------------+

The following operators are supported:

+--------------+------------------------------------------------------------------+
| Type         | Operators                                                        |
+==============+==================================================================+
| Numeric      | ``+``, ``-``, ``*``, ``/``, ``%``                                |
+--------------+------------------------------------------------------------------+
| String       | ``~`` , ``~*``, ``!~``, ``!~*``, ``LIKE``, ``||``                |
+--------------+------------------------------------------------------------------+
| Array        | ``||``, ``[ ... ]``                                              |
+--------------+------------------------------------------------------------------+
| Relational   | ``=``, ``>=``, ``<=``, ``<>``, ``BETWEEN``, ``IN``, ``NOT IN``   |
+--------------+------------------------------------------------------------------+
| Boolean      | ``AND``, ``OR``, ``NOT``                                         |
+--------------+------------------------------------------------------------------+
| Projection   | ``foo.bar``, ``foo[2]``, ``foo{*}``, ``foo[*]``                  |
+--------------+------------------------------------------------------------------+
| Date/Time    | ``TIMESTAMP``, ``DATE``, ``INTERVAL``, ``TIME``                  |
+--------------+------------------------------------------------------------------+
| Identity     | ``OID``                                                          |
+--------------+------------------------------------------------------------------+

**Note:** ``~`` , ``~*``, ``!~``, and ``!~*`` are regular expression
operators. ``~*``, ``!~``, and ``!~*`` are preliminary and may not
work in the current release.

**Also Note:**  The ``||`` operator for strings will concatenate two
strings; for example, you can create a full name from a first and last
name property: \ ``c.firstName || ' ' || c.lastName``. The ``||``
operator for arrays will concatenate two arrays; for example, if ``xy``
is an array with two values, then ``c.xy || [0]`` will create an array
with three values, where the third value is zero.

The following functions are supported:

+---------------+---------------------------------------------------------------------------+
| Type          | Functions                                                                 |
+===============+===========================================================================+
| String        | ``CONCAT``, ``LOWER``, ``UPPER``, ``SUBSTRING``, ``LENGTH``, ``SEARCH``   |
+---------------+---------------------------------------------------------------------------+
| DateTime      | ``DATE_PART``, ``TO_TIMESTAMP``                                           |
+---------------+---------------------------------------------------------------------------+
| Nulls         | ``COALESCE``                                                              |
+---------------+---------------------------------------------------------------------------+
| Arrays        | ``ARRAY_LENGTH``, ``FLATTEN_ARRAY``                                       |
+---------------+---------------------------------------------------------------------------+
| Objects       | ``FLATTEN_OBJECT``                                                        |
+---------------+---------------------------------------------------------------------------+
| Set-Level     | ``DISTINCT``, ``DISTINCT_BY``                                             |
+---------------+---------------------------------------------------------------------------+
| Aggregation   | ``COUNT``, ``SUM``, ``MIN``, ``MAX``, ``AVG``                             |
+---------------+---------------------------------------------------------------------------+
| Identity      | ``SQUASH``                                                                |
+---------------+---------------------------------------------------------------------------+

--------------

Syntax Changes in Version 2.5
-----------------------------

When SlamData 2.5 was released in early 2016 the SQL2 syntax changed
slightly. Please make note of the following syntax to avoid problems in
queries:

Query paths must be surrounded by backward-ticks \` on both ends of the
path.

Example:

::

    SELECT * FROM `/users`

When specifying a string in any context surround it by quotes ``"`` on
both ends.

Example:

::

    SELECT * FROM `/users` WHERE first_name = "Joe"

When specifying a single character, and not a full string, in any
context, surround it by single ticks ``'`` on both ends.

Example:

::

    SELECT * FROM `/users` WHERE middle_initial = 'A'

When using functions such as ``DATE``, ``DATETIME``, ``TIMESTAMP``, etc
that require a value it must be surrounded by ``(`` and ``)``. Single
character ``'`` and string ``"`` quotes still apply.

Example:

::

    SELECT * FROM `/users`
    WHERE
        last_login > DATE("2016-01-01") AND
        subscribed = 'Y'

Basic Selection
---------------

The ``SELECT`` statement returns a result set of records from one or
more tables.

Select all values from a path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To select all values from a path, use the asterisk (``*``).

Example:

::

    SELECT * FROM `/users`

Select specific fields from a path
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To select specific fields from a path, use the field names, separated by
commas.

Example:

::

    SELECT name, age FROM `/users`

Give a path an alias to refer to in the query
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow the path name with an ``AS`` and an alias name, and then you can
use the alias name when specifying the fields. This is especially useful
when you have data from more than one source.

Example:

::

    SELECT c.name, c.age FROM `/users` AS c

--------------

Filtering a Result Set
----------------------

You can filter a result set using the WHERE clause. The following
operators are supported:

-  Relational: ``-``, ``=``, ``>=``, ``<=``, ``<>``, ``BETWEEN``,
   ``IN``, ``NOT IN``
-  Boolean: ``AND``, ``OR``, ``NOT``

Filtering using a numeric value:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

::

    SELECT c.name FROM `/users` AS c WHERE c.age > 40

Filtering using a string value:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

::

    SELECT c.name FROM `/users` AS c WHERE c.name = "Sherlock Holmes"

Filtering using multiple Boolean predicates:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Example:

::

    SELECT
      c.name FROM `/users` AS c
    WHERE
      c.name = "Sherlock Holmes" AND
      c.street = "Baker Street"

--------------

Numeric and String Operations
-----------------------------

You can use any of the operators or functions listed in the `Clauses,
Operators, and Functions <#clauses-operators-and-functions>`__ section on
numbers and strings. Some common string operators and functions include:

+------------------------+----------------------------+
| Operator or Function   | Description                |
+========================+============================+
| ``||``                 | Concatenates               |
+------------------------+----------------------------+
| ``LOWER``              | Converts to lowercase      |
+------------------------+----------------------------+
| ``UPPER``              | Converts to uppercase      |
+------------------------+----------------------------+
| ``SUBSTRING``          | Returns a substring        |
+------------------------+----------------------------+
| ``LENGTH``             | Returns length of string   |
+------------------------+----------------------------+

**Examples:**

Using mathematical operations:

::

    SELECT c.age + 2 * 1 / 4 % 2 FROM `/users` AS c

Concatenating strings:

::

    SELECT c.firstName || ' ' || c.lastName AS name FROM `/users` AS c

Filtering by fuzzy string comparison using the ``LIKE`` operator:

::

    SELECT * FROM `/users` AS c WHERE c.firstName LIKE "%Joan%"

Filtering by regular expression:

::

    SELECT * FROM `/users` AS c WHERE c.firstName ~ "[sS]h+""

--------------

Dates and Times
---------------

Filter by dates and times using the ``TIMESTAMP``, ``TIME``, and
``DATE`` operators. The ``DATEPART`` operator can also be used
to select part of a date, such as the day.

**Note:**  Some databases will automatically convert strings into dates
or date/times. SlamData does not perform this conversion, since the
underlying database has no schema and no fixed type for any field. As a
result, an expression like ``WHERE ts > "2015-02-10"`` compares
string-valued ``ts`` fields with the string ``"2015-02-10"`` instead of
a date comparison.

    If you want to embed literal dates, timestamps, etc. into your SQL
    queries, you should use the time conversion operators, which accept
    a string and return value of the appropriate type. For example, the
    above snippet could be converted to
    ``WHERE ts > DATE("2015-02-10")``, which looks for date-valued
    ``ts`` fields and compares them with the date ``2015-02-10``.

*NOTE for MongoDB Users*:

    If your MongoDB data does not use MongoDB's native date/time type,
    and instead, you store your timestamps as epoch milliseconds in a
    numeric value, then you should either compare numbers or use the
    ``TO_TIMESTAMP`` function.

Filter based on a timestamp (date and time)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``TIMESTAMP`` operator to convert a string into a date and time.
The string should have the format ``YYYY-MM-DDTHH:MM:SSZ``.

Example:

::

    SELECT * FROM `/log/events` AS c WHERE c.ts > TIMESTAMP("2015-04-29T15:16:55Z")

Filter based on a time
~~~~~~~~~~~~~~~~~~~~~~

Use the ``TIME`` operator to convert a string into a time. The string
should have the format ``HH:MM:SS``.

Example:

::

    SELECT * FROM `/log/events` AS c WHERE c.ts > TIME("15:16:55")

Filter based on a date
~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE`` operator to convert a string into a date. The string
should have the format ``YYYY-MM-DD``.

Example:

::

    SELECT * FROM `/log/events` AS c WHERE c.ts > DATE("2015-04-29")

Filter based on part of a date
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE_PART`` function to select part of a date. ``DATE_PART``
has two arguments: a string that indicates what part of the date or time
that you want and a timestamp field. Valid values for the first argument
are century, day, decade, ``dow`` (day of week), ``doy`` (day of year),
``hour``, ``isodoy``, ``microseconds``, ``millenium``, ``milliseconds``,
``minute``, ``month``, ``quarter``, ``second``, and ``year``.

Example:

::

    SELECT DATE_PART("day", c.ts) FROM `/log/events` AS c

Filter based on a Unix epoch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``TO_TIMESTAMP`` function to convert Unix epoch (milliseconds)
to a timestamp.

Example:

::

    SELECT * FROM `/log/events` AS c WHERE c.ts > TO_TIMESTAMP(1446335999)

--------------

Grouping
--------

SQL2 allows you to group data by fields and by date parts.

Group based on a single field
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use ``GROUP BY`` to group results by a field.

Example:

::

    SELECT
        c.age,
        COUNT(*) AS cnt
    FROM `/users` AS c
    GROUP BY c.age

Group based on multiple fields
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can group by multiple fields with a comma-separated list of fields
after ``GROUP BY``.

Example:

::

    SELECT
        c.age,
        c.gender,
        COUNT(*) AS cnt
    FROM `/users` AS c
    GROUP BY c.age, c.gender

Group based on date part
~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``DATE_PART`` function to group by a part of a date, such as the
month.

Example:

::

    SELECT
        DATE_PART("day", c.ts) AS day,
        COUNT(*) AS cnt
    FROM `/log/events` AS c
    GROUP BY DATE_PART("day", c.ts)

Filter within a group
~~~~~~~~~~~~~~~~~~~~~

Filter results within a group by adding a ``HAVING`` clause followed by
a Boolean predicate.

Example:

::

    SELECT
        DATE_PART("day", c.ts) AS day,
        COUNT(*) AS cnt
    FROM `/prod/purger/events` AS c
    GROUP BY DATE_PART("day", c.ts)
    HAVING c.gender = "female"

Double grouping
~~~~~~~~~~~~~~~

Perform double-grouping operations by putting operators inside other
operators. The inside operator will be performed on each group created
by the ``GROUP BY`` clause, and the outside operator will be performed
on the results of the inside operator.

Example:

This query returns the average population of states. The outer
aggregation function (AVG) operates on the results of the inner
aggregation (``SUM``) and ``GROUP BY`` clause.

::

    SELECT AVG(SUM(pop)) FROM `/population` GROUP BY state

--------------

Nested Data and Arrays
----------------------

Unlike a relational database many NoSQL databases allow data to be
nested (that is, data can be objects) and to contain arrays.

Nesting
~~~~~~~

Nesting is represented by levels separated by a period (``.``).

Example:

::

    SELECT c.profile.address.street.number FROM `/users` AS c

Arrays
~~~~~~

Array elements are represented by the array index in square brackets
(``[n]``).

Example:

::

    SELECT c.profile.allAddress[0].street.number FROM `/users` AS c

Flattening
~~~~~~~~~~

You can extract all elements of an array or all field values
simultaneously, essentially removing levels and flattening the data. Use
the asterisk in square brackets (``[*]``) to extract all array elements.

Example:

::

    SELECT c.profile.allAddresses[*] FROM `/users` AS c

Use the asterisk in curly brackets (``{*}``) to extract all field
values.

Example:

::

    SELECT c.profile.{*} FROM `/users` AS c

Filtering using arrays
~~~~~~~~~~~~~~~~~~~~~~

You can filter using data in all array elements by using the asterisk in
square brackets (``[*]``) in a ``WHERE`` clause.

Example:

::

    SELECT DISTINCT * FROM `/users` AS c WHERE c.profile.allAddresses[*].street.number = "221B"

--------------

Pagination and Sorting
----------------------

Pagination
~~~~~~~~~~

Pagination is used to break large return results into smaller chunks.
Use the ``LIMIT`` operator to set the number of results to be returned
and the ``OFFSET`` operator to set the index at which the results should
start.

Example (Limit results to 20 entries):

::

    SELECT * FROM `/users` LIMIT 20

Example (Return the 100th to 119th entry):

::

    SELECT * FROM `/users` OFFSET 100 LIMIT 20

Sorting
~~~~~~~

Use the ``ORDER BY`` clause to sort the results. You can specify one or
more fields for sorting, and you can use operators in the ``ORDER BY``
arguments. Use ``ASC`` for ascending sorting and ``DESC`` for decending
sorting.

Example (Sort users by ascending age):

::

    SELECT * FROM `/users` ORDER BY age ASC

Example (Sort users by last digit in age, descending, and full name,
ascending):

::

    SELECT * FROM `/users`
    ORDER BY age % 10 DESC, firstName + lastName ASC

--------------

Joining Collections
-------------------

Use the ``JOIN`` operator to join two or more different collections.

The ``JOIN`` operator is a powerful way to implement joins in
non-relational databases such as MongoDB. There is no enforced limit to
how many collections or tables can be joined in a query but common sense
should prevail based on the size of collections.

Examples:

This example returns the names of employees and the names of the
departments they belong to by matching up the employee deparment ID with
the department's ID, where both IDs are ObjectID types.

::

    SELECT
        emp.name,
        dept.name
    FROM `/employees` AS emp
    JOIN `/departments` AS dept ON dept._id = emp.departmentId

If one of the IDs is a string, then use the ``OID`` operator to convert
it to an ID.

::

    SELECT
        emp.name,
        dept.name
    FROM `/employees` AS emp
    JOIN `/departments` AS dept ON dept._id = OID(emp.departmentId)

Join Considerations
~~~~~~~~~~~~~~~~~~~

On ``JOIN``\ s with more than two collections or tables, the standard
rule of thumb is to place the tables in order from smallest to largest.
If the collections ``a``, ``b``, and ``c`` have ``4``, ``8``, and ``16``
documents respectively, then ordering ``FROM `/a`, `/b`, `/c``` is most
efficient with ``WHERE a._id = b._id``.

If, however, the filter condition is ``WHERE b._id = c._id`` then the
appropriate ordering would be
``FROM `/b`, `/c`, `/a` WHERE b._id = c._id``. This is because without
the filter \|a ⨯ b\| = 32 which is less than \|b ⨯ c\| = 128, but with
the filter, \|b ⨯ c\| is limited to the number of documents in b, which
is 8 (and which is lower than the unconstrained \|a ⨯ b\|).

--------------

Conditionals and Nulls
----------------------

Conditionals
~~~~~~~~~~~~

Use the ``CASE`` expression to provide if-then-else logic to SQL2. The
``CASE`` sytax is:

::

    SELECT (CASE <field>
        WHEN <value1> THEN <result1>
        WHEN <value2> THEN <result2>
        ...
        ELSE <elseResult>
        END)
    FROM `<path>`

Example:

The following example generates a code based on gender string values.

::

    SELECT (CASE c.gender
        WHEN "male" THEN 1
        WHEN "female" THEN 2
        ELSE 3
        END) AS genderCode
    FROM `/users` AS c

Nulls
~~~~~

Use the ``COALESCE`` function to evaluate the arguments in order and
return the current value of the first expression that initially does not
evaluate to ``NULL``.

Example:

This example returns a full name, if not null, but returns the first
name if the full name is null.

::

    SELECT COALESCE(c.fullName, c.firstName) AS name FROM `/users` AS c

--------------

Database Specific Notes
-----------------------

MongoDB
~~~~~~~

The _id Field
^^^^^^^^^^^^^^

By default, the ``_id`` field will not appear in a result set. However,
you can specify it by selecting the ``_id`` field. For example:

::

    SELECT _id AS cust_id FROM `/users`

MongoDB has special rules about fields called ``_id``. For example, they
must remain unique, which means that some queries (such as
``SELECT myarray[*] FROM foo``) will introduce duplicates that MongoDB
won't allow. In addition, other queries change the value of ``_id``
(such as grouping). So SlamData manages ``_id`` and treats it as a
special field.

**Note:** To filter on ``_id``, you must first convert a string to an
object ID, by using the ``OID`` function. For example:

::

    SELECT * FROM `/foo` WHERE _id = OID("abc123")
