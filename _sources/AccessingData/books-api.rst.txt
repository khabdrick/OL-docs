Books API
=========

The Open Library Books API provides a programmatic client-side 
method for querying information of books using Javascript.
Open Library has 4 types of Book APIs.

1. The **Works** API (by Work ID)
2. The **Editions** API (by Edition ID)
3. The **ISBN** API (by ISBN)
4. The **Books** API (generic)

Works & Editions APIs
-----------------------

A Work is a logical collection of similar Editions of the same book. "Fantastic Mr. Fox" could be a Work which contains a Spanish translation edition, or perhaps a 2nd edition which has an additional 
chapter or corrections. Work metadata will include general umbrella information about a book, whereas an Edition will have a publisher, an ISBN, a book-jacket, and other specific information.

Both Work and Edition pages on Open Library (i.e. the pages you navigate to) may also be returned as JSON or YAML (in addition to HTML) by modifying the page URL (example below).

Works API
~~~~~~~~~

Work pages on Open Library begin with the URL prefix "/works".

Endpoint
^^^^^^^^

- **URL**: ``/works/{workId}``
- **Method**: ``GET``

Parameters
^^^^^^^^^^

+-----------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| Path parameters                                     |    Description                                                                                         |
+=====================================================+========================================================================================================+
| ``/works/{workId}``                                 | Provide HTML page of the details about a work, including its editions, availability, ID Numbers, etc.  |
+-----------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| ``/works/{workId}.json`` or ``/works/{workId}.yml`` | Provide work details in JSON or YAML format respectively.                                              |
+-----------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| ``/works/{workId}/editions.json``                   | Get list of work's editions in JSON format.                                                            |
+-----------------------------------------------------+--------------------------------------------------------------------------------------------------------+



Sample Request
^^^^^^^^^^^^^^

.. code-block:: bash

    curl -X GET https://openlibrary.org/works/OL45804W.json

Sample Response
^^^^^^^^^^^^^^^

.. code-block:: json

    {
    "title": "Fantastic Mr Fox",
    "key": "/works/OL45804W",
    "authors": [
        {
        "author": {
            "key": "/authors/OL34184A"
        },
        "type": {
            "key": "/type/author_role"
        }
        },
        {
        "author": {
            "key": "/authors/OL10649522A"
        },
        "type": {
            "key": "/type/author_role"
        }
        }
    ],
    "type": {
        "key": "/type/work"
    },
    "description": "The main character of Fantastic Mr. Fox is an extremely clever anthropomorphized fox named Mr. Fox. He lives with his wife and four little foxes. ...",
    "covers": [
        6498519,
        8904777,
        108274,
        233884,
        ...
    ],
    "subject_places": [
        "English countryside"
    ],
    "subjects": [
        "Animals",
        "Hunger",
        "Open Library Staff Picks",
        ...
    ],
    "subject_people": [
        "Bean",
        "Boggis",
        "Bunce",
        "Mr Fox"
    ],
    "subject_times": [
        "20th Century"
    ],
    "location": "/works/OL45883W",
    "latest_revision": 14,
    "revision": 14,
    "created": {
        "type": "/type/datetime",
        "value": "2009-10-15T11:34:21.437031"
    },
    "last_modified": {
        "type": "/type/datetime",
        "value": "2023-03-24T21:31:47.967277"
    }
    }

Fields in API response
^^^^^^^^^^^^^^^^^^^^^^

* ``title``: The title of the work, which is "Fantastic Mr Fox".
* ``key``: The unique identifier for the work, which is "/works/OL45804W".
* ``authors``: An array of author objects, each containing:
    * ``author``: The unique identifier for the author, such as "/authors/OL34184A" and "/authors/OL10649522A".
    * ``type``: The type of role the author plays, indicated by a unique identifier like "/type/author_role".
* ``type``: The type of information, indicated by a unique identifier like "/type/work".
* ``description``: A detailed description of the work, providing an overview of the main character (Mr. Fox), his actions, and the overall storyline.
* ``covers``: Provide an array of cover IDs.
* ``subject_places``: An array of places or locations associated with the work, in this case containing "English countryside".
* ``subjects``: An array of subject tags related to the work, such as "Animals", "Hunger", "Children's stories", and others.
* ``subject_people``: An array of people or characters associated with the work, including "Bean", "Boggis", "Bunce", and "Mr Fox".
* ``subject_times``: An array of time periods or eras related to the work, in this case containing "20th Century".
* ``location``: Provides the URL path to the work.
* ``latest_revision``: The latest revision number of the work.
* ``revision``: The current revision number of the work.
* ``created``: An object indicating the creation date and time of the work, with the "type" field specifying the type of date and the "value" field providing the actual timestamp.
* ``last_modified``: An object indicating the date and time of the last modification to the work, similar to the "created" field.

Ratings and Bookshelves
^^^^^^^^^^^^^^^^^^^^^^^
Ratings provide information on number of people that have rated the book and the average rating of user satisfaction user satisfaction on a scale of five. You can also get the number of people that rated 
each number on the scale.
Bookshelves indicates the number of books users want to read, are currently reading, and have already read, 
The ratings and bookshelves can be accessed with the following APIs:

* ``https://openlibrary.org/works/{workId}/bookshelves.json``

* ``https://openlibrary.org/works/{workId}/ratings.json``


Editions API
~~~~~~~~~~~~
Edition pages on Open Library begin with the prefix "/books".

Here is an example:
https://openlibrary.org/books/OL7353617M/Fantastic_Mr._Fox

In this example, just like an Work page, if we remove the /Title from the URL (e.g. https://openlibrary.org/works/OL45804W) and then add a suffix of ".json" or ".yml" to the end, the page will return a data representation instead of HTML, e.g.:
https://openlibrary.org/books/OL7353617M.json

ISBN API
~~~~~~~~~~
The ISBN API is a special case and alternative approach to arriving at an Editions page. Instead of "/books", a path of "/isbn" is used, followed by a valid ISBN 10 or 13.

Here is an example:
https://openlibrary.org/isbn/9780140328721

In this example, entering this URL will result in a redirect to the appropriate Editions page: https://openlibrary.org/books/OL7353617M

Just like an Edition or Work page, we may add ".json" to the end of the URL to request the response in json instead of as HTML, e.g.:

https://openlibrary.org/isbn/9780140328721.json

Book API
---------
The Book API is a generic, flexible, configurable endpoint which allows requesting information on one or more books using ISBNs, OCLC Numbers, LCCNs and OLIDs (Open Library IDs). It is inspired by the Google Books `Dynamic links API <https://code.google.com/apis/books/docs/dynamic-links.html>`_ and is compatible with it.
At the core of the API is a URL format that allows developers to construct URLs requesting information on one or more books and send the requests to the Open Library using the ``<script>`` tag.

<script src="https://openlibrary.org/api/books?bibkeys=ISBN:0451526538&callback=mycallback"></script>

Request Format
~~~~~~~~~~~~~~
The API supports the following query parameters.

bibkeys 
^^^^^^^   
List of IDs to request the information. The API supports ISBNs, LCCNs, OCLC numbers, and OLIDs (Open Library IDs).

+--------------------------+----------------------------------------------------------------------------------------------------+
| Query parameters         |    Description                                                                                     |
+==========================+====================================================================================================+
| ``ISBN``                 | Ex. &bibkeys=ISBN:0451526538 (The API supports both ISBN 10 and 13.)                               |
+--------------------------+----------------------------------------------------------------------------------------------------+
| ``OCLC``                 | &bibkeys=OCLC:36792831                                                                             |
+--------------------------+----------------------------------------------------------------------------------------------------+
| ``LCCN``                 | &bibkeys=LCCN:93005405                                                                             |
+--------------------------+----------------------------------------------------------------------------------------------------+
| ``OLID``                 | &bibkeys=OLID:OL123M                                                                               |
+--------------------------+----------------------------------------------------------------------------------------------------+

format
^^^^^^

Optional parameter which specifies the response format. Possible values are JSON and JavaScript. The default format is JavaScript.
The response of the API contains a JSON object for each matched bib_key. The contents of the JSON object are decided by the jscmd parameter.
By default, the API returns the response as Javascript.

Sample Request
""""""""""""""

.. code-block:: bash

    curl 'http://openlibrary.org/api/books?bibkeys=ISBN:0201558025,LCCN:93005405'

Sample Response
""""""""""""""""

.. code-block:: javascript

    var _OLBookInfo = {
        "ISBN:0201558025": {
            ...
        },
        "LCCN:93005405": {
            ...
        }
    };

When ``format=json`` parameter is passed, the API returns the response as JSON instead of JavaScript. This is useful when accessing the API at the server-side.

Sample Request
""""""""""""""
.. code-block:: bash

    curl 'https://openlibrary.org/api/books?bibkeys=ISBN:0201558025,LCCN:93005405&format=json'

Sample Response
""""""""""""""""
.. code-block:: JSON

    {
        "ISBN:0201558025": {
            ...
        },
        "LCCN:93005405": {
            ...
        }
    }

callback
^^^^^^^^

Optional parameter which specifies the name of the JavaScript function to call with the result. This is considered only when the format is JavaScript.
When optional callback parameter is passed, the response is wrapped in a Javascript function call.

Sample Request
""""""""""""""
.. code-block:: bash

    curl 'https://openlibrary.org/api/books?bibkeys=ISBN:0201558025,LCCN:93005405&callback=processBooks'

Sample Response
""""""""""""""""
.. code-block:: bash

    processBooks({
        "ISBN:0201558025": {
            ...
        },
        "LCCN:93005405": {
            ...
        }
    });

jscmd
^^^^^
Optional parameter to decide what information to provide for each matched ``bib_key``. Possible values are viewapi and data. The default value is ``viewapi``.

**jscmd=viewapi**

When ``jscmd`` is not specified or when ``jscmd=viewapi``, the request will look like the following:

.. code-block:: bash

    curl 'https://openlibrary.org/api/books?bibkeys=ISBN:0385472579,LCCN:62019420&format=json'

    {
        "ISBN:0385472579": {
            "bib_key": "ISBN:0385472579",
            "preview": "noview",
            "thumbnail_url": "https://covers.openlibrary.org/b/id/240726-S.jpg",
            "preview_url": "https://openlibrary.org/books/OL1397864M/Zen_speaks",
            "info_url": "https://openlibrary.org/books/OL1397864M/Zen_speaks"
        },
        "LCCN:62019420": {
            "bib_key": "LCCN:62019420",
            "preview": "full",
            "thumbnail_url": "https://covers.openlibrary.org/b/id/6121771-S.jpg",
            "preview_url": "https://archive.org/details/adventurestomsa00twaigoog",
            "info_url": "https://openlibrary.org/books/OL23377687M/adventures_of_Tom_Sawyer"
        }
    }

Fields in API response
"""""""""""""""""""""""

* ``bib_key``: Identifier used to query this book.
* ``info_url``: A URL to the book page in the Open Library.
* ``preview``: Preview state - either "noview" or "full".
* ``preview_url``: A URL to the preview of the book. This links to the archive.org page when a readable version of the book is available, otherwise it links to the book page on openlibrary.org. Please note that the preview_url is always provided even if there is no readable version available. The preview property should be used to test if a book is readable.
* ``thumbnail_url``: A URL to a thumbnail of the cover of the book. This is provided only when thumbnail is available.

**jscmd=data**
When the ``jscmd=data``, data about each matching book is returned. The request and response will look like the following:

.. code-block:: bash

    $ curl 'https://openlibrary.org/api/books?bibkeys=ISBN:9780980200447&jscmd=data&format=json'

    {
        "ISBN:9780980200447": {
            "publishers": [
                {
                    "name": "Litwin Books"
                }
            ],
            "identifiers": {
                "google": [
                    "4LQU1YwhY6kC"
                ],
                "lccn": [
                    "2008054742"
                ],
                "isbn_13": [
                    "9780980200447"
                ],
                "amazon": [
                    "098020044X"
                ],
                "isbn_10": [
                    "1234567890"
                ],
                "oclc": [
                    "297222669"
                ],
                "librarything": [
                    "8071257"
                ],
                "project_gutenberg": [
                    "14916"
                ],
                "goodreads": [
                    "6383507"
                ]
            },
            "classifications": {
                "dewey_decimal_class": [
                    "028/.9"
                ],
                "lc_classifications": [
                    "Z1003 .M58 2009"
                ]
            },
            "links": [
                {
                    "url": "http://johnmiedema.ca",
                    "title": "Author's Website"
                }
            ],
            "weight": "1 grams",
            "title": "Slow reading",
            "url": "https://openlibrary.org/books/OL22853304M/Slow_reading",
            "number_of_pages": 80,
            "cover": {
                "small": "https://covers.openlibrary.org/b/id/5546156-S.jpg",
                "large": "https://covers.openlibrary.org/b/id/5546156-L.jpg",
                "medium": "https://covers.openlibrary.org/b/id/5546156-M.jpg"
            },
            "subjects": [
                {
                    "url": "https://openlibrary.org/subjects/books_and_reading",
                    "name": "Books and reading"
                },
                {
                    "url": "https://openlibrary.org/subjects/reading",
                    "name": "Reading"
                }
            ],
            "publish_date": "2009",
            "authors": [
                {
                    "url": "https://openlibrary.org/authors/OL6548935A/John_Miedema",
                    "name": "John Miedema"
                }
            ],
            "excerpts": [
                {
                    "comment": "test purposes",
                    "text": "test first page"
                }
            ],
            "publish_places": [
                {
                    "name": "Duluth, Minn"
                }
            ]
        }
    }

Fields in API response
"""""""""""""""""""""""

* ``url``: URL of the book

* ``title`` and ``subtitle``: Title and subtitle of the book.

* ``authors``: List of authors. Each entry will be in the following format:

.. code-block:: json

    {
        "name": "...",
        "url": "https://openlibrary.org/authors/..."
    }

* ``identifiers``: All identifiers of the book in the following format:

.. code-block:: json

    {
        "isbn_10": [...],
        "isbn_13": [...],
        "lccn": [...],
        "oclc": [...],
        "goodreads": [...]
    }
    
* ``classifications``: All classifications of the book in the following format.

.. code-block:: json

    {
        "lc_classifications": [...],
        "dewey_decimal_class": [...]
    }

* ``subjects``, ``subject_places``, ``subject_people`` and ``subject_times``: List of subjects, places, people and times of the book. Each entry will be in the following format:

.. code-block:: json

    {
        "url": "https://openlibrary.org/subjects/history",
        "name": "History"
    }

* ``publishers``: List of publishers. Each publisher will be in the following format:

.. code-block:: json

    {
        "name": "..."
    }

* ``publish_places``: List of publish places. Each entry will be in the following format:

.. code-block:: json

    {
        "name": "..."
    }

* ``publish_date``: Published date as a string.

* ``excerpts``: List of excerpts to that book. Each entry will be in the following format:

.. code-block:: json

    {
        "comment": "...",
        "text": "..."
    }

* ``links``: List of links to the book. Each link will be in the following format:

.. code-block:: json

    {
        "url": "https://...",
        "title": "..."
    }

* ``cover``: URLs to small, medium and large covers.

.. code-block:: json

    {
        "small": "https://covers.openlibrary.org/b/id/1-S.jpg",
        "medium": "https://covers.openlibrary.org/b/id/1-M.jpg",
        "large": "https://covers.openlibrary.org/b/id/1-L.jpg",
    }

* ``ebooks``: List of ebooks. Each entry will be in the following format:

.. code-block:: json

    {

        "preview_url": "https://archive.org/details/..."
    }

* ``number_of_pages``: Number of pages in that book.

* ``weight``: Weight of the book.

**jscmd=details**

When ``jscmd=details`` is passed, additional details are provided in addition to the info provided by ``viewapi``. The provided details are same as the data provided by the RESTful API.
It is advised to use jscmd=data instead of this as that is more stable format.

.. code-block:: json

    $ curl 'https://openlibrary.org/api/books?bibkeys=ISBN:9780980200447&jscmd=details&format=json'
    {
        "ISBN:9780980200447": {
            "info_url": "https://openlibrary.org/books/OL22853304M/Slow_reading",
            "bib_key": "ISBN:9780980200447",
            "preview_url": "https://openlibrary.org/books/OL22853304M/Slow_reading",
            "thumbnail_url": "https://covers.openlibrary.org/b/id/5546156-S.jpg",
            "preview": "noview",
            "details": {
                "number_of_pages": 80,
                "table_of_contents": [
                    {
                        "title": "The personal nature of slow reading",
                        "type": {
                            "key": "/type/toc_item"
                        },
                        "level": 0
                    },
                    {
                        "title": "Slow reading in an information ecology",
                        "type": {
                            "key": "/type/toc_item"
                        },
                        "level": 0
                    },
                    {
                        "title": "The slow movement and slow reading",
                        "type": {
                            "key": "/type/toc_item"
                        },
                        "level": 0
                    },
                    {
                        "title": "The psychology of slow reading",
                        "type": {
                            "key": "/type/toc_item"
                        },
                        "level": 0
                    },
                    {
                        "title": "The practice of slow reading.",
                        "type": {
                            "key": "/type/toc_item"
                        },
                        "level": 0
                    }
                ],
                "weight": "1 grams",
                "covers": [
                    5546156
                ],
                "lc_classifications": [
                    "Z1003 .M58 2009"
                ],
                "latest_revision": 14,
                "source_records": [
                    "marc:marc_loc_updates/v37.i01.records.utf8:4714764:907",
                    "marc:marc_loc_updates/v37.i24.records.utf8:7913973:914",
                    "marc:marc_loc_updates/v37.i30.records.utf8:11406606:914"
                ],
                "title": "Slow reading",
                "languages": [
                    {
                        "key": "/languages/eng"
                    }
                ],
                "subjects": [
                    "Books and reading",
                    "Reading"
                ],
                "publish_country": "mnu",
                "by_statement": "by John Miedema.",
                "oclc_numbers": [
                    "297222669"
                ],
                "type": {
                    "key": "/type/edition"
                },
                "physical_dimensions": "1 x 1 x 1 inches",
                "revision": 14,
                "publishers": [
                    "Litwin Books"
                ],
                "description": "\"A study of voluntary slow reading from diverse angles\"--Provided by publisher.",
                "physical_format": "Paperback",
                "last_modified": {
                    "type": "/type/datetime",
                    "value": "2010-08-07T19:35:52.482887"
                },
                "key": "/books/OL22853304M",
                "authors": [
                    {
                        "name": "John Miedema",
                        "key": "/authors/OL6548935A"
                    }
                ],
                "publish_places": [
                    "Duluth, Minn"
                ],
                "pagination": "80p.",
                "classifications": {},
                "created": {
                    "type": "/type/datetime",
                    "value": "2009-01-07T22:16:11.381678"
                },
                "lccn": [
                    "2008054742"
                ],
                "notes": "Includes bibliographical references and index.",
                "identifiers": {
                    "amazon": [
                        "098020044X"
                    ],
                    "google": [
                        "4LQU1YwhY6kC"
                    ],
                    "project_gutenberg": [
                        "14916"
                    ],
                    "goodreads": [
                        "6383507"
                    ],
                    "librarything": [
                        "8071257"
                    ]
                },
                "isbn_13": [
                    "9780980200447"
                ],
                "dewey_decimal_class": [
                    "028/.9"
                ],
                "isbn_10": [
                    "1234567890"
                ],
                "publish_date": "2009",
                "works": [
                    {
                        "key": "/works/OL13694821W"
                    }
                ]
            }
        }
    }

Earlier these details were provided when ``details=true`` parameter is passed. It is equivalent to ``jscmd=details`` and it is retained only for backward-compataibilty.