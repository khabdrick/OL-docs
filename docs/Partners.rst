Partners API (Formerly "Read" API)
===================================

Partners API, formerly the "Read" API, fetches one or more books by library identifiers (ISBNs, OCLC, LCCNs). The Open Library Read API is intended to make it easy to turn book identifier information (ISBN, LCCN etc.) into direct links to Open Library's online-readable or borrowable books.
The Read API is inspired by the `Hathi Trust Bibliographic API <https://www.hathitrust.org/bib_api>`_ and is intended to be compatible with it. The Read API results extend the Hathi API in several ways:

* The results may offer links to borrowable books, available through the Open Library `lending program <https://openlibrary.org/help/faq/borrow>`_.
* Returned items may include readable matches for different editions of the same work. 
* Results also include information from the `Data API(needs update) <http://openlibrary.org/dev/docs/api/books#data>`_- everything Open Library knows about a book!

Demo and Simple Sample Code
---------------------------

Here's a `simple demo page <https://web.archive.org/web/20130128051054/https://internetarchive.github.com/read_api_extras/readapi_demo.html>`_. This page demonstrates adding (hidden) 'book' links with ID information, then turning them into links to Open Library books by including a `script <http://internetarchive.github.com/read_api_extras/readapi_automator.js>`_ that automates calling the Read API.

There are two parts to this:

1. A page-level JS link that calls ``readapi_automator.js``:

.. code-block:: html

    <script src="readapi_automator.js"></script>

2. One (empty) <div> tag per book, like this:

.. code-block:: html

    <div class="ol_readapi_book" isbn="039471752X" lccn="75009828"><div>

See the `demo page code <https://github.com/internetarchive/read_api_extras/blob/gh-pages/readapi_demo.html>`_ for how it works!

API
---

The API is used to request information on one or more books using library identifiers: ISBNs, OCLC Numbers, LCCNs and OLIDs (Open Library Identifiers). Requests can be made in two formats: single and multiple.

The single-request format
~~~~~~~~~~~~~~~~~~~~~~~~~

To request information about readable versions of a single book edition, GET a URL in the following format:
``http://openlibrary.org/api/volumes/brief/<id-type>/<id-value>.json``

``<id-type>`` can be ``isbn``, ``lccn``, ``oclc`` or ``olid`` (Open Library Identifier).
``<id-value>`` is the actual library identifier.

For example:
http://openlibrary.org/api/volumes/brief/isbn/0596156715.json will return a JSON hash:

.. code-block:: json

    {'items':
    [{'match': 'exact',
        'status': 'full access'}],
        'itemURL': 'http://www.archive.org/stream/TheArtOfCommunity',
        'cover': {'large': 'http://covers.openlibrary.org/b/id/6223071-L.jpg',
                'medium': 'http://covers.openlibrary.org/b/id/6223071-M.jpg',
                'small': 'http://covers.openlibrary.org/b/id/6223071-S.jpg'},
        'fromRecord': '/books/OL23747519M',
        'ol-edition-id': 'OL23747519M',
        'ol-work-id': 'OL15328717W'}],
    'records':
    {'/books/OL23747519M':
        {'data': { ... }
        'isbns': ['0596156715',
                    '9780596156718'],
        'publishDates': ['August 2009'],
        'recordURL': 'http://openlibrary.org/books/OL23747519M'}}}

The hash contains two fields:

items
^^^^^

This is a list of any matching or similar online-readable books that Open Library knows about, including links to cover images and read or borrow urls. Thw list may be empty if nothing's available.
Items are sorted first by match ('exact', 'similar') then by availability ('freely available', 'lendable' etc.) then by edition date. The first returned item is usually the best choice to display to the user.

item fields:
* ``match``: This may be ``exact`` or ``similar``. ``exact`` indicates that this scan is of the edition specified by the library ID. ``similar`` indicates that it``s a scan of another edition of the same work.

* ``status``: ``full access``, ``lendable``, ``checked out`` or ``restricted.``

* ``itemURL``: Link to an online-readable scan of the book, or to a borrow page.

* ``cover``: Links to small, medium and large versions of the cover.

* ``fromRecord``: The entry in ``records`` that refers to this item. See below; there's almost always just one record.

* ``publishDate``: Date of publication of this particular returned item; note that it may be different from the looked-up record, if ``match`` is ``similar``.

* ``ol-edition-id``: The Open Library Edition identifier for this item

* ``ol-work-id``: The Open Library Work identifier for this item.

records
^^^^^^^

Bibliographic information about the match. This portion of the result can potentially contain information on more than one match (as, for instance, two books may have the same isbn) - but in almost all cases will just contain one.

Record fields:
* ``isbns``, ``lccns``, ``oclcs``, ``olids``. Each of these fields contains a list of any known library attributes for this record.

* ``publishDates``: A list of publish dates for this edition

* ``recordURL``: URL for the Open Library page for this book.

* ``data``: Results of the Data API for this book.

The multi-request format
~~~~~~~~~~~~~~~~~~~~~~~~~

This format allows multiple requests to be issued in same call. Also, each request can contain multiple keys, to increase the chances of a match.

The URL format is ``http://openlibrary.org/api/volumes/brief/json/<request-list>``;

* ``<request-list>`` is a list of ``<request>`` s, separated by '|'.

* A ``<request>`` is a list of one or more ``<library-ids>``, separated by ';'.

* A ``<library-id>`` is an ``<id-type>`` and an ``<id-value>``, separated by a ':'.

* ``<id-type>`` and ``<id-value>`` are as the single-request format.

The return value is a hash, with each successful ``<request>`` as keys. The hash values are the same as the single-request format. If a request doesn't match, it won't appear as a result in the hash.
If a ``<request>`` starts with 'id:<key>;', then <key> is used instead of the full ``<request>`` string as the key for that hash value.

For example: 
``<http://openlibrary.org/api/volumes/brief/json/id:1;lccn:50006784|olid:OL6179000M;lccn:55011330>``_ will return a hash with two keys:

.. code-block:: json

    {
        '1': ...
        'olid:OL6179000M;lccn:55011330': ...
    }

The key values of the response above are similar to those from the single-request format.

JSONP Requests
~~~~~~~~~~~~~~~

The Read API supports JSONP. Just add ?callback=my_callback to the request, and the result will be wrapped in ``my_callback()``.

Using the API
-----------------

Request IP Address
~~~~~~~~~~~~~~~~~~~
The Read API checks the requesting IP address when looking for borrowable matching books. If the requesting IP address is one by the In Library lending program, many more borrowable books will be returned. If you're with a participating library, it may make sense to use `EZProxy <http://openlibrary.org/dev/docs/ezproxy>`_ to make sure that webpage access to the Read API appears to come from an In Library IP.

