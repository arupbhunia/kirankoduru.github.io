---
layout: post
title:  "Best Practices for designing APIs"
date:   2017-10-31 03:01:46
categories: software-design
description : Some ideas you need to remember when you are building or designing APIs
author: Kiran Koduru
image: /img/best-practices-for-designing-apis.png
---
A favorite question for system design during interviews is to design an API. Though there are no right answers to the question, you are tested on your basic understanding of software design. Your interviewer might be probe you on some key concepts. This is a list of things that might be useful when answering that dreaded API design question or a refresher on best practices for building APIs.

I won’t be dwelling into programming languages to use in this post since most of these concepts are language agnostic. There are libraries that already implement most of these concepts easily. Some google fu will lead you in the right direction.

### What kind of API are we discussing today?

APIs also known as Application Programming Interfaces could be anything that allows users to leverage or build upon your software. We will be talking about REST APIs in this post. Something that is accessible via the HTTP protocol. It could be accessible publicly or privately through URLs that you program.

### Know your methods

Some commonly used HTTP verbs when building RESTful services are `GET`, `POST`, `PATCH`, `PUT` and `DELETE`. Each provide a way to communicate with your data. Some people argue heavily over which verb is used for what purpose. The general idea of these HTTP methods are:

- __GET__: Used for retrieval of data.
- __POST__: Create or insert data.
- __PATCH__: Used to update or replace data.
- __PUT__: Used to update or replace too.
- __DELETE__: You guessed it. It deletes data.

### Design your endpoints or resources

A endpoint is the entry point to a web service. It is the only way the external world will be able to interact with your code. Unaware of your tech stack, you guide  your users(with proper documentation) on what methods to use against your endpoints. 

You can multiple endpoints in an API. For instance you might want to use an endpoint for fetching data from users table and another to view analytics. You decide what HTTP verbs and parameters your endpoints might accept.

### Choosing your database and schema

Some APIs work around databases. Some interviewers will want you to build APIs that retrieve and massage the data before it is interacted by your user. Generally for the position of Data Engineers questions about databases are common. 

You can be tested on your schema design skills. If picking a NoSQL database make sure you don't normalize your collections(or table) so much that, you would require multiple joins to retrieve information.

### Basic Authentication, Token, HMAC and Authorization

Security is of utmost importance even when you make information public. Your endpoints are the only doors to the outside world. Secure them. Make it impossible for anyone with the right permissions to access this information. You can use [Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication), [Token Authentication](https://auth0.com/learn/token-based-authentication-made-easy/) or [HMAC](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code) to secure your endpoints. There are other kinds of authentications you can look up as well but this could be a starting point. 

You might also want to look into [Roles based access control](https://en.wikipedia.org/wiki/Role-based_access_control). This authorization method allows administrators to have more privileges over the same data than regular users.

### Data Validation

Do not accept any input from users blindly. Validate the data that you insert. Your first point of contact with user supplied information needs to be verified. If your endpoints accept page numbers, you can validate against a regular expression that checks for page numbers. To validate zip codes, verify if it is a 5 digits sequence. For URLs, you can check if it's a `http` or `https` scheme, ignore `ftp` URLs or relative paths. 

These are just made up validation checks that won't hold much ground in the real world but it certainly sets the first barrier in communicating with your API. Fail fast so your users never get too deep into your APIs code.

### Database Reads, Writes and Deletes

Once you have validated the data, you could allow it to interact with your database. Every operation interacting with the database must be considered from the point of scaling as well. Too many reads and writes might slow things down when you have a lot of users. You can choose to also provide users with bulk operations which will minimize the number of read or writes to your database.

You could cache the results from your database as well. Set a expiration time for invalidating your cached information. 

For deletion you could mark records with a extra field `deleted=True` also known as soft delete or delete the record entirely from the database. 

### XML or JSON Response

You will need to provide your users with HTTP response message that can be easily parsed. Most programming languages have built-in libraries that can parse XML or JSON. 

You can write a wrapper class around your data which can be rendered in both JSON or XML format responses.

### Versioning

For times when you want to add new resources or when you want to discontinue support for some features, versioning your API is good practice. Instead of querying your API like 

<pre>
https://my-tracking-api.com/track/
</pre>

what if users used versioning path in your URLs like 

<pre>
https://my-tracking-api.com/v1/track/
</pre>

Adding the versioning string to your endpoints allows you to support `v2` release and `v1` at the same time if needed. 

And for times when you want to wean off users from `v1` you can provide them with enough notice and slowly end support for older versions. 

### Pagination, Sorting and other URL params

Apart from querying a resource or endpoint, your users might need specific options to play with your API. While it's considered bad practice to display all the results in your API responses, providing users with a `per_page` parameter to select number of results to display in each response and a page number parameter `page_num` parameter to iterate through pages of results is considered perfectly okay. 

Other frequently used parameters that you should think of supporting in your initial releases are sorting, searching and response types to switch between JSON or XML responses.

### Rate Limiting and Caching

Though your API service is protected, you can never trust who might make too many requests for a given resource. You could protect yourself by setting a limit on the number of requests for any given user. Also called Rate Limiting.

Let's say you want to allow `100 requests/min` for user ID `1`. An idea for implementation would be to use a key value store like __redis__ where the key could be user ID and value would be the number of `requests/min`. For each request you would decrement the counter for user ID `1` and every 1 minute you would reset it to `100` again. Once the counter reaches `0` you could temporarily block the user from making more requests.

You can also minimize reads or writes to the database is by generating the same response twice. Also known as caching. You could also purge your cache after a set time internal.

>The benefit of doing this is that we gain speed and reduce server load. The best way to cache your API is to put a gateway cache (or reverse proxy) in front of it. Some frameworks provide their own reverse proxies, but a very powerful, open-source one is Varnish.
>
> Joshua Thijssen ([restcookbook.com](http://restcookbook.com))

### Response Status Codes

Most developers overlook the right HTTP status codes to use when designing APIs. Some programmers love to program against HTTP status codes since `200 OK` response doesn't usually provide much insight. Here's a list of codes which isn't comprehensive in anyway but considered the most frequently used when designing APIs.

|HTTP Code|Short Description|When to use it|
|:--------|:-------|:----|
|200|Success - OK|Used when any request has succeeded. This is sometimes used a catch all for `GET`, `HEAD` and `POST` requests|
|201|Success - Created|When a record is successfully inserted for an endpoint or resource. Mostly used when data is inserted via a `POST` request.|
|304|Not Modified|Return along with cached results for a user requesting the same resource. You can check against `ETag` header for when to purge cached results.|
|400|Bad Request|Raised for malformed syntax from a end user.|
|401|Unauthorized|Resource or endpoint is protected and will require authentication.|
|405|Method Not Allowed|Restrict the methods that a user can use to access a given resource. Not all endpoints require a `POST` method.|
|429|Too many requests|Can be raised after users exceed Rate Limits.|

Apart from the response codes above, you could also read about other HTTP status codes on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

### Documentation

Finally allow your users the wealth of knowledge they have access to via good documentation. Write about what methods are allowed for specific endpoints. How can they authenticate or request access to your API. Describe what parameters they can tweak with for different endpoints. Also, make sure to version your documentation as you version your API.

I hope my post was useful in some way to get you started on building APIs. If you have any comments are suggestion to add to the post please leave them below. If not, you can subscribe to my newsletter and receive other posts like these in your inbox.
