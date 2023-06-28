Accessing Books Data 
======================

Open Library offers a suite of APIs to help developers get up and running with our data. This includes `RESTful APIs <https://openlibrary.org/dev/docs/restful_api>`_, which make Open Library data availabile in JSON, 
YAML and RDF/XML formats. There's also an earlier, now deprecated `JSON API <https://openlibrary.org/dev/docs/json_api>`_ which is preserved for backward compatibility.

:doc:`Books API <books-api>` - Retrieve a specific work or edition by identifier

`Authors API <https://openlibrary.org/dev/docs/api/authors>`_- Retrieve an author and their works by author identifier

`Subjects API <https://openlibrary.org/dev/docs/api/subjects>`_ - Fetch books by subject name.

`Search API <https://openlibrary.org/dev/docs/api/search>`_- Search results for books, authors, and more.

`Search inside API <https://openlibrary.org/dev/docs/api/search_inside>`_- Search for matching text within millions of books.

`Partner API <https://openlibrary.org/dev/docs/api/read>`_- Formerly the "Read" API, fetch one or more books by library identifiers (ISBNs, OCLC, LCCNs).

`Covers API <https://openlibrary.org/dev/docs/api/covers>`_- Fetch book covers by ISBN or Open Library identifier.

`Recent Changes API <https://openlibrary.org/dev/docs/api/recentchanges>`_- Programatic access to changes across Open Library.

Bulk Access
-----------

Please do not use our APIs for bulk download of Open Library data because this affects our ability to serve patrons. We make our data `publicly available <https://openlibrary.org/developers/dumps>`_ each month for partners. If you want a dump of complete data, 
please read about your `Bulk Download <https://openlibrary.org/data#downloads>`_ options, or email us at openlibrary@archive.org.

More APIs
---------

Did you know, nearly every page on Open Library is or has an API. You can return structured bibliographic data for any page by adding a .rdf/.json/.yml extension to the end of any Open Library identifier. For instance: https://openlibrary.org/works/OL15626917W.json or https://openlibrary.org/authors/OL33421A.json. Many pages, such as the Books, Authors, and Lists, will include links to their RDF and JSON formats.

Questions
---------

We encourage developers to ask questions by opening issues on `Github <https://github.com/internetarchive/openlibrary/issues>`_ and on our `gitter <https://gitter.im/theopenlibrary/Lobby>`_ chat channel.


.. toctree::
   :hidden:

   books-api