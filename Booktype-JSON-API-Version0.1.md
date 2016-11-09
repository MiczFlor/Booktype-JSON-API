# Communicating with Booktype

The Booktype JSON API is based on Version 1.0 of JSON API, a specification for how a client should request that resources be fetched or modified, and how a server should respond to those requests. 

For more information see: http://jsonapi.org/format/1.0/

This page presents Booktype JSON API version 0.1. None of the normative text on this page will change. Subsequent versions of will remain compatible with this one, as the Booktype JSON API uses a never remove, only add strategy.

## Client Server does the talking

### Book related

#### Does book with ID xyz exist?

##### Request:

```
GET /book/xyz HTTP/1.1
Accept: application/vnd.api+json
```

##### Answer Success:
```
{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "attributes": {
      "title": "Book title"
    }
  }
}
```
##### Answer Error:
```
404 NOT FOUND
```
#### Create new book with ID xyz

The Booktype server MAY accept a client-generated ID along with a request to create a resource. An ID MUST be specified with an id key, the value of which MUST be a universally unique identifier. If no ID is given by the client, Booktype creates its own ID.

The attribute "title" MUST be provided. All other attributes are optional.
```
POST /book HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "attributes": {
      "title": "Book title",
      "subtitle": "StringValue",
      "author": "StringValue",
      "publisher": "StringValue",
      "short_description": "StringValue",
      "issued": "StringValue",
      "dateCopyrighted": "StringValue",
      "publisher_city": "StringValue",
      "ebook_isbn": "StringValue",
      "rightsHolder": "StringValue",
      "text_by": "StringValue",
      "edited_by": "StringValue",
      "translation_by": "StringValue",
      "introduction_by": "StringValue",
      "cover_design": "StringValue",
      "cover_photography": "StringValue",
      "photography": "StringValue",
      "illustration_by": "StringValue",
      "research": "StringValue",
      "lectorate": "StringValue",
      "proofreading": "StringValue",
      "rights_clearing": "StringValue",
      "typeface": "StringValue",
      "printed_on": "StringValue",
      "bookbinder": "StringValue",
      "printer": "StringValue",
      "printer_country": "StringValue",
      "acknowledgement": "StringValue",
      "paper_certification": "StringValue",
      "edition": "StringValue",
      "url_publisher": "StringValue",
      "url_author": "StringValue",
      "bibliographic_information": "StringValue",
      "dedication": "StringValue"
    }
  }
}
```
#### Update metadata for book xyz
```
PATCH /book/xyz HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "attributes": {
      "title": "StringValue",
      "subtitle": "StringValue",
      "author": "StringValue",
      "publisher": "StringValue",
      "short_description": "StringValue",
      "issued": "StringValue",
      "dateCopyrighted": "StringValue",
      "publisher_city": "StringValue",
      "ebook_isbn": "StringValue",
      "rightsHolder": "StringValue",
      "text_by": "StringValue",
      "edited_by": "StringValue",
      "translation_by": "StringValue",
      "introduction_by": "StringValue",
      "cover_design": "StringValue",
      "cover_photography": "StringValue",
      "photography": "StringValue",
      "illustration_by": "StringValue",
      "research": "StringValue",
      "lectorate": "StringValue",
      "proofreading": "StringValue",
      "rights_clearing": "StringValue",
      "typeface": "StringValue",
      "printed_on": "StringValue",
      "bookbinder": "StringValue",
      "printer": "StringValue",
      "printer_country": "StringValue",
      "acknowledgement": "StringValue",
      "paper_certification": "StringValue",
      "edition": "StringValue",
      "url_publisher": "StringValue",
      "url_author": "StringValue",
      "bibliographic_information": "StringValue",
      "dedication": "StringValue"
    }
  }
}
```
### User related

#### Does user with ID abc exist?

##### Request:
```
GET /user/abc HTTP/1.1
Accept: application/vnd.api+json
```
##### Answer Success:
```
{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "user",
    "id": "abc",
    "attributes": {
      "fullname": "Full Name",
      "email": "Email"
    }
  }
}
```
##### Answer Error:
```
404 NOT FOUND
```
#### Create new user with ID abc
```
POST /user HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "user",
    "id": "abc",
    "attributes": {
      "fullname": "Full Name",
      "email": "Email"
    }
  }
}
```
#### Update user information for user abc with this information
```
PATCH /user HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "user",
    "id": "abc",
    "attributes": {
      "name": "Full Name",
      "email": "Email"
    }
  }
}
```
#### Grant full access for user abc to book xyz

The access is granted by updating the role of a user in relation to a book.
Available roles depend on the Booktype configuration.

The request triggers Booktype to do the following:

* Add user abc to the collaborators of book xyz
* Assign the role in user.attributes.role for user abc in book xyz
```
POST /user HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "user",
    "id": "abc",
    "attributes": {
      "role": "Editor"
    },
    "relationships": {
      "data": { 
        "type": "book", 
        "id": "xyz"
      }
    }
  }
}
```
??? or the other way around ???
```
POST /user HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "relationships": {
      "data": { 
        "type": "user", 
        "id": "abc",
        "attributes": {
          "role": "Editor"
        }
      }
    }
  }
}
```
## Booktype talking

#### User abc edited book xyz and created a PDF which is here: url
```
POST /book HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "attributes": {
      "format": "PDF"
    },
    "links": {
      "self": "http://example.com/articles/1"
    }
    "relationships": {
      "data": { 
        "type": "user", 
        "id": "abc"
      }
    }
  }
}
```
#### User abc edited book xyz and created an EPUB which is here: url
```
POST /book HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "jsonapi": {
    "version": "1.0"
  }
  "data": {
    "type": "book",
    "id": "xyz",
    "attributes": {
      "format": "EPUB"
    },
    "links": {
      "self": "http://example.com/articles/1"
    }
    "relationships": {
      "data": { 
        "type": "user", 
        "id": "abc"
      }
    }
  }
}
```
