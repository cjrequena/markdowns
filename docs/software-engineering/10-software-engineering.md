---
layout: default
title: api-design-reference-guideline
parent: software-engineering
nav_order: 10
---
# API Design Reference Guideline
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

Software architecture centers around decoupled microservices that provide functionality via RESTful APIs with a JSON payload. Engineering teams own, deploy and operate these microservices in AWS.

Our APIs most purely express what our systems do, and are therefore highly valuable business assets. Designing high-quality, long-lasting APIs has become critical.

With this in mind, we’ve adopted "API First" as one of our key engineering principles. Microservices development begins with API definition outside the code and ideally involves ample peer-review feedback to achieve high-quality APIs.

API First encompasses a set of quality-related standards and fosters a peer review culture including a lightweight review procedure. We encourage our teams to follow them to ensure that our APIs:

- Are easy to understand and learn.
- Are general and abstracted from specific implementation and use cases.
- Are robust and easy to use.
- Have a common look and feel.
- Follow a consistent RESTful style and syntax.
- Are consistent with other teams’ APIs and our global architecture

## Conventions Used in These Guideline

The requirement level keywords **"MUST"**, **"MUST NOT"**, **"REQUIRED", "SHALL", "SHALL NOT"**, **"SHOULD", "SHOULD NOT"**, **"RECOMMENDED"**, **"MAY"**, and **"OPTIONAL"** used in this document are to be interpreted as described in [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119 "RFC 2119: Key words for use in RFCs to Indicate Requirement Levels") and in [RFC 8174](https://datatracker.ietf.org/doc/html/rfc8174 "RFC 8174: Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words")

```
1. MUST   This word, or the terms "REQUIRED" or "SHALL", mean that the
   definition is an absolute requirement of the specification.

2. MUST NOT   This phrase, or the phrase "SHALL NOT", mean that the
   definition is an absolute prohibition of the specification.

3. SHOULD   This word, or the adjective "RECOMMENDED", mean that there
   may exist valid reasons in particular circumstances to ignore a
   particular item, but the full implications must be understood and
   carefully weighed before choosing a different course.

4. SHOULD NOT   This phrase, or the phrase "NOT RECOMMENDED" mean that
   there may exist valid reasons in particular circumstances when the
   particular behavior is acceptable or even useful, but the full
   implications should be understood and the case carefully weighed
   before implementing any behavior described with this label.

5. MAY   This word, or the adjective "OPTIONAL", mean that an item is
   truly optional.  One vendor may choose to include the item because a
   particular marketplace requires it or because the vendor feels that
   it enhances the product while another vendor may omit the same item.
   An implementation which does not include a particular option MUST be
   prepared to interoperate with another implementation which does
   include the option, though perhaps with reduced functionality. In the
   same vein an implementation which does include a particular option
   MUST be prepared to interoperate with another implementation which
   does not include the option (except, of course, for the feature the
   option provides.)
```

## General

### ==Must: Follow API first principle==

### ==Must: Provide API reference definition using OpenAPI==

We use the OpenAPI specification (aka Swagger spec) as standard for our REST API definitions. You may choose YAML or JSON as a format of your OpenAPI API definition file; however, YAML is generally preferred due to its improved readability.

We also call the OpenAPI API definition the "API Reference definition" (or "API definition"); it provides all information needed by an experienced API client developer to use this API.

The OpenAPI API specification file should be the subject of version control together with source code management. Services also have to support an endpoint to access the API Reference definition for their external API(s).

### ==Should: Provide API user manual==

In addition to the API Specification, it is good practice to provide an API user manual to improve client developer experience, especially of engineers that are less experienced in using this API.

A helpful API user manual typically describes the following API aspects:

-  API scope, purpose, and use cases.
- Concrete examples of API usage.
- Edge cases, error situation details, and repair hints.
- Architecture context and major dependencies - including figures and sequence flows

The user manual must be published online, e.g. via our documentation hosting platform service, GHE pages, or specific team web servers. Please do not forget to include a link to the API user manual into the API specification using the #/externalDocs/url property.

### ==Must: Write APIs in English==

## Security

### ==Must: Secure endpoints with OAuth 2.0==

Every API endpoint needs to be secured using OAuth 2.0 using the auth service. Please refer to the official OpenAPI spec on how to specify security definitions in your API specification or take a look at the following example.

```yaml
components:
  securitySchemes:
    oauth2:
      type: oauth2
      flows:
        clientCredentials:
          tokenUrl: https://api.tui-dx.com/oauth2/token
          scopes:
            booking-issue-service.booking-issue.retrieve: Booking issue retrieve right needed.
            booking-issue-service.booking-issue.create: Booking issue create right needed.
```

<br>

The example defines OAuth2 with client credentials flow as security standard used for authentication when accessing endpoints.&#x20;

Additionally, there are two API access rights (permissions) defined via the scopes section for later endpoint authorization usage (see next section).

It makes little sense specifying the flow to retrieve OAuth tokens in the securitySchemes section, as API endpoints should not care, how OAuth tokens were created.&#x20;

Unfortunately the flow field is mandatory and cannot be omitted. API endpoints should always set flow:&#x20;

clientCredentials and ignore this information.

### ==Must: Define and assign access rights (Scopes)==

APIs must define permissions to protect their resources. Thus, at least one permission must be assigned to each endpoint.&#x20;

Permissions are defined as shown in the previous section.

Every API needs to define access right or permissions, called scopes here, and every endpoint needs to have at least one scope assigned.&#x20;

Scopes are defined by name and description per API specification, as shown in the previous section.

APIs should stick to standard scopes by default -- for the majority of use cases, restricting access to specific APIs (with read vs. write differentiation) is sufficient for controlling access for client types like merchant or retailer business partners, customers or operational staff.&#x20;

We want to avoid too many, fine grained scopes increasing governance complexity without real value add.&#x20;

In some situations, where the API serves different types of resources for different owners, resource specific scopes may make sense.

Some examples for standard and resource-specific scopes:

| **Application ID**    | **Resource ID** | **Access Type** | **Example**                                  |
| --------------------- | --------------- | --------------- | -------------------------------------------- |
| booking-issue-service | booking-issue   | retrieve        | booking-issue-service.booking-issue.retrieve |
| booking-issue-service | booking-issue   | create          | booking-issue-service.booking-issue.crearte  |

After scopes names are defined and the scope is declared in the security definition at the top of an API specification, it should be assigned to each API operation by specifying a security requirement like this:

```yaml
paths:
  /booking-issue-service/{id}:
    get:
      summary: Retrieves a booking issue
      security:
        - oauth2:
          - booking-issue-service.booking-issue.retrieve
```

In very rare cases a whole API or some selected endpoints may not require specific access control. However, to make this explicit you should assign the uid pseudo access right scope in this case. It is the user id and always available as OAuth2 default scope.

```yaml

paths:
  /public-information:
    get:
      summary: Provides public information about ...
               Accessible by any user; no access rights needed.
      security:
        - oauth2:
          - uid
```

## Compatibility

### ==Must: Do not break backward compatibility==

Change APIs, but keep all consumers running. Consumers usually have independent release lifecycles, focus on stability, and avoid changes that do not provide additional value. APIs are service contracts that cannot be broken via unilateral decisions.

There are two techniques to change APIs without breaking them:

1. Follow rules for compatible extensions.
2. Introduce new API versions and still support older versions.

We strongly encourage using compatible API extensions and discourage versioning. With Postel’s Law in mind, here are some rules for providers and consumers that allow us to make compatible changes without versioning:

### ==Must: Do not use URI versioning==

With URI versioning a (major) version number is included in the path, e.g. /v1/customers. The consumer has to wait until the provider has been released and deployed. If the consumer also supports hypermedia links — even in their APIs — to drive workflows (HATEOAS), this quickly becomes complex. So does coordinating version upgrades — especially with hyperlinked service dependencies — when using URL versioning. To avoid this tighter coupling and complexer release management we do not use URI versioning, and go instead with media type versioning and content negotiation (see above).

### ==Should: Avoid versioning if backward compatibility can be maintained.==

When changing your RESTful APIs, do so in a compatible way and avoid generating additional API versions. Multiple versions can significantly complicate understanding, testing, maintaining, evolving, operating and releasing our systems (supplementary reading).

If changing an API can’t be done in a compatible way, then proceed in one of these three ways:

1. &#x20;Create a new resource (variant) in addition to the old resource variant.
2. &#x20;Create a new service endpoint — i.e. a new application with a new API (with a new domain name).
3. &#x20;Create a new API version supported in parallel with the old API by the same microservice

As we discourage versioning by all means because of the manifold disadvantages, we suggest to only use the first two approaches.

### ==Should: Provide Version Information in OpenAPI Documentation==

Only the documentation, not the API itself, needs version information.

Example:

```json
"swagger": "v2",
"info": {
  "title": "Parcel service API",
  "description": "API for <...>",
  "version": "v2",
    <...>
}
```

Example:

## Versioning

Versioning is one of the most important considerations when designing your Web API.

### ==Must: Never release an API without a version. Make the version mandatory.==

### ==Must: Specify the version with a ‘v’ (e.g. v1).==

### ==Must: Use a simple ordinal number.==

Don’t use the dot notation like v1.2 because it implies a granularity of versioning that doesn’t work well with APIs--it’s an interface not an implementation. Stick with v1, v2, and so on.

### ==Must:  Use the Accept & the Accept-Version headers==

There is a well-known HTTP header called Accept which is sent on a request from a client to a server. For instance

`Accept: application/json`

This notation is saying that I, the client, would like the response to be in json, please.

For reactive communication you can use:

`Accept: application/stream+json  or  Accept: text/event-stream`

For defining the version a custom header will be used. The accept headers is using this header to make up your own resource types, for example:

`Accept-Version: vnd.myapi.v2`

Using headers is more correct for many reasons: it leverages existing HTTP standards, it’s intellectually consistent with Fielding’s vision, it solves some hard real-world problems related to inter-dependent APIs, and more.

<br>

**Example of versioning strategy**

```http
http://company.com/api/customer/123

===>
GET /customer/123 HTTP/1.1
Accept: application/xml
Accept-Version: vnd.myapi.v3
  
<===
HTTP/1.1 200 OK
Content-Type: application/xml
<customer>
  <name>Neil Armstrong</name>
</customer>
```

## JSON Guidelines

JSON here refers to RFC 7159 (which updates RFC 4627), the “application/json” media type and custom JSON media types defined for APIs.

### ==Must: Use Consistent Property Names==

### ==Must: Property names must be snake\_case (and never camelCase).==

No established industry standard exists, but many popular Internet companies prefer snake\_case: e.g. GitHub, Stack Exchange, Twitter. Others, like Google and Amazon, use both - but not only camelCase. It’s essential to establish a consistent look and feel such that JSON looks as if it came from the same hand.

### ==Must: Property names must be an ASCII subset==

Property names are restricted to ASCII encoded strings. The first character must be a letter, an underscore or a dollar sign, and subsequent characters can be a letter, an underscore, a dollar sign, or a number.

### ==Must: Use consistent property values==

### ==Must: Boolean property values must not be null==

Schema basedJSON properties that are by design booleans must not be presented as nulls.&#x20;

A boolean is essentially a closed enumeration of two values, true and false.&#x20;

If the content has a meaningful null value, strongly prefer to replace the boolean with enumeration of named values or statuses - for example accepted\_terms\_and\_conditions with true or false can be replaced with terms\_and\_conditions with values yes, no and unknown.

### ==Must: Date property values must conform to RFC 3339==

Use the date and time formats defined by RFC 3339:

- For "date" use strings matching date-fullyear "-" date-month "-" date-day, for example: 2015-05-28
- For "date-time" use strings matching full-date "T" full-time , for example 2015-05- 28T14:07:17Z

Note that the OpenAPI format "date-time" corresponds to "date-time" in the RFC and 2015- 05-28 for a date.&#x20;

Note that the OpenAPI format "date" corresponds to "full-date" in the RFC.

Both are specific profiles, a subset of the international standard ISO 8601.

A zone offset may be used (both, in request and responses) -- this is simply defined by the standards. However, we encourage restricting dates to UTC and without offsets. For example 2015-05-28T14:07:17Z rather than 2015-05-28T14:07:17+00:00.

From experience we have learned that zone offsets are not easy to understand and often not correctly handled.&#x20;

Note also that zone offsets are different from local times that might be including daylight saving time.

Localization of dates should be done by the services that provide user interfaces, if required.&#x20;

When it comes to storage, all dates should be consistently stored in UTC without a zone offset.

Localization should be done locally by the services that provide userinterfaces, if required.

Sometimes it can seem data is naturally represented using numerical timestamps, but this can introduce interpretation issues with precision - for example whether to represent a timestamp as 1460062925, 1460062925000 or 1460062925.000.&#x20;

Date strings, though more verbose and requiring more effort to parse, avoid this ambiguity.

See: <https://www.utctime.net/>

### ==Must: Use standards for language==

To ensure **consistency, interoperability, and clarity**, always use standardized codes for language, country, and currency.

**1. Country Codes – ISO 3166-1 alpha-2**

- Use the **two-letter country codes** from the **ISO 3166-1 alpha-2** standard.
- **Example:**
    - `GB` (United Kingdom)
    - `UK` (Incorrect, as "UK" is not the official ISO code)

**2. Language Codes – ISO 639-1 and BCP-47**

- Use **ISO 639-1**, the **two-letter language codes** (e.g., `en` for English, `fr` for French).
- For **language variants and regional differences**, use **BCP-47**, which builds on ISO 639-1.
- **Examples:**
    - `en` → English
    - `fr` → French
    - `es-ES` → Spanish (Spain)
    - `es-MX` → Spanish (Mexico)

**3. Currency Codes – ISO 4217**

- Use **ISO 4217**, the **three-letter currency codes**.
- **Examples:**
    - `USD` (United States Dollar)
    - `EUR` (Euro)
    - `JPY` (Japanese Yen)

**Summary of Best Practices**

| **Type**     | **Standard**       | **Example**            |
| ------------ | ------------------ | ---------------------- |
| **Country**  | ISO 3166-1 alpha-2 | `GB` (United Kingdom)  |
| **Language** | ISO 639-1 / BCP-47 | `en`, `es-ES`, `fr-CA` |
| **Currency** | ISO 4217           | `USD`, `EUR`, `JPY`    |

### ==Should: Reserved JavaScript keywords should be avoided==

Most API content is consumed by non-JavaScript clients today, but for security and sanity reasons, JavaScript (strictly, ECMAScript) keywords are worth avoiding. A list of keywords can be found in the ECMAScript Language Specification.

### ==Should: Array names should be pluralized==

To indicate that they contain multiple values, **prefer pluralizing array names**. This implies that **object names should, in turn, be singular**.

### ==Must: Null values must have their fields removed==

### ==Should: Empty array values should not be null==

Empty array values can unambiguously be represented as the the emptylist, \[] .

### ==Should: Enumerations should be represented as Strings==

Strings are a reasonable target for values that are by design enumerations.

### ==Could: Time durations and intervals could conform to ISO 8601==

Schema-based JSON properties that represent **durations and intervals** should be **formatted as strings**, following the **ISO 8601** standard. *(Appendix A of RFC 3339 provides a grammar for durations.)*

## Naming Conventions

In REST, primary data representation is called Resource. A resource can be a singleton or a collection. For example, “customers” is a collection resource and “customer” is a singleton resource (in a banking domain).&#x20;

We can identify “customers” collection resource using the URI “/customers”.&#x20;

We can identify a single “customer” resource using the URI “/customers/{customerId}”.

REST APIs use Uniform Resource Identifiers (URIs) to address resources. REST API designers should create URIs that convey a REST API’s resource model to its potential client developers. When resources are named well, an API is intuitive and easy to use. If done poorly, that same API can feel difficult to use and understand.

The best practices in naming resources are listed in the subsecctions below.

### ==Must: Use nouns in order to name a resource==

RESTful URI should refer to a resource that is a thing (noun) instead of referring to an action (verb) because nouns have properties which verbs do not have – similar to resources have attributes.

1. &#x20;Keep the base URL simple and intuitive the base URL is the most important design affordance of your API.
2. &#x20;Keep verbs out of the base URL

**There should be 2 base URL per resource:**

1. The first URL is for a collection. You can apply filters to retrieve a partial set of the collection.
2. The second URL is for a specific element in the collection. Since the URI contains a single resource, the identifier must uniquely identify a single resource.

**Use HTTP verbs to operate on the collection and elements**

**HTTP verbs are POST,  GET,  PUT,  DELETE,  PATCH**

| **RESOURCE** | **GET**                        | **POST**             | **PUT**               | **PATCH**                              | **DELETE**    |
|--------------|--------------------------------|----------------------|------------------------|----------------------------------------|---------------|
| `/hotels`    | Get a collection list          | Create new           | Update                 | Bad Request (400)                      | Delete all    |
| `/hotel/1`   | Get a single resource by ID    | Bad Request (400), new | If exists, then update | If exists, update the fields given     | Delete by ID  |
|              |                                |                      | Else 404               | Else 404                               |               |


### ==Must: Use lowercase separate words with hyphens for path segments==

Example:

`/purchase-orders/{purchase-order-id}`

This applies to concrete path segments and not the names of path parameters.

**`Must`**`: Use snake_case (never camelCase) for Query Parameters`

Examples:

`customer_number, order_id, billing_address`

### ==Must: Use Hyphenated HTTP Headers==

This is for consistency in your documentation (most other headers follow this convention).&#x20;

Avoid camelCase (without hyphens). Exceptions are common abbreviations like “ID.”

Examples:

```http
Accept-Encoding
Apply-To-Redirect-Ref
Disposition-Notification-Options
Original-Message-ID
Accept
Accept-Version
```

**See also: HTTP Headers are case-insensitive (RFC 7230).**

### ==Must: Pluralize resource names==

Usually, a collection of resource instances is provided (at least API should be ready here). The special case of a resource singleton is a collection with cardinality 1.

Given that the first thing most people probably do with a RESTful API is a GET, we think it reads more easily and is more intuitive to use plural nouns. But above all, avoid a mixed model in which you use singular for some resources, plural for others. Being consistent allows developers to predict and guess the method calls as they learn to work with your API.

| **RESOURCES** |
| ------------- |
| /hotels       |
| /rooms        |
| /bookings     |
| /transfers    |

### ==Must: Avoid Trailing Slashes==

The trailing slash must not have specific semantics. Resource paths must deliver the same results whether they have the trailing slash or not.

### ==Should: Not use /api as base path==

In most cases, all resources provided by a service are part of the public API, and therefore should be made available under the root "/" base path.

**Why?**

1. **Redundancy**
    - If all resources in a service are part of the API, adding `/api` to the base path is unnecessary.
    - Example: Instead of `/api/users`, simply use `/users`.
2. **Cleaner and More Intuitive URLs**
    - RESTful APIs should have **clear, predictable** paths without extra nesting.
    - A root (`/`) base path keeps endpoints straightforward.
3. **Better Maintainability**
    - Avoids hardcoded, redundant prefixes that may require changes later.
    - Makes URL structures easier to manage and scale.
4. **Consistency with Industry Standards**
    - Many well-designed APIs (e.g., GitHub, Stripe) expose resources directly under `/`, not `/api`.

**Examples**

**Recommended (Without /api)**

```http
GET /users       → Fetch all users  
GET /users/123   → Fetch user with ID 123  
POST /orders     → Create a new order  
```

**Not Recommended (With /api)**

```http
GET /api/users        → Redundant `/api`  
GET /api/users/123    → Extra nesting, unnecessary  
POST /api/orders      → No added value from `/api`  
```

**When Might /api Be Useful?**

- If the service exposes **both API and non-API resources** (e.g., serving a frontend and an API on the same domain).
- If multiple versions of an API are running (`/api/v1/`, `/api/v2/`).  -- **We do not use versioning at URI level.**
- If explicitly needed to separate internal and external APIs.

## Resources

### ==Must: Avoid actions — Think about resources==

REST is all about your resources, so consider the domain entities that take part in web service interaction, and aim to model your API around these using the standard HTTP methods as operation indicators. For instance, if an application has to lock articles explicitly so that only one user may edit them, create an article lock with PUT or POST instead of using a lock action.

```
Request:
PUT /recipes/100
```

The added benefit is that you already have a service for browsing and filtering article locks.

### ==Must: Keep URLs Verb-Free==

The API describes resources, so the only place where actions should appear is in the HTTP methods. In URLs, use only nouns.

### ==Must: Use Domain-Specific Resource Names==

API resources represent elements of the application’s domain model. Using domain-specific nomenclature for resource names helps developers to understand the functionality and basic semantics of your resources.&#x20;

It also reduces the need for further documentation outside the API definition. For example, "purchase-order-items" is superior to "purchase-items" in that it clearly indicates which business object it represents. Along these lines, “items” is too general.

To provide some guidance the following  characteristics that a name should embody in order to align with practical API design principles:

- **Expressiveness**, which means an API's name should clearly convey the type of function, resource, message, field, property or value that it represents;
- **Simplicity**, which dictates that names should be expressive, but only to the extent that each part of the name adds a justifiable value
- **Predictability**, meaning API names should be easy to learn and follow a recognizable naming pattern.
- **Secure,** do not expose internal implementation of the end system (do no expose fields like XREF in the api if it is used to store the internal invoice number)

<br>

### ==Must: Identify resources and Sub-Resources via Path Segments==

```http
Basic URL structure:
/{resources}/[resource_id]/{sub-resources}/[sub_resource_id]

Examples:
/orders/1681e6b88ec1/items
/orders/1681e6b88ec1/items/1
```

### ==Must: Must simplify associations==

Resources almost always have relationships to other resources. What's a simple way to express these relationships in a Web API?

For example given the following operations GET /owners/5678/dogs and POST /owners/5678/dogs the relationships can be complex. Owners have relationships with veterinarians, who have relationships with dogs, who have relationships with food, and so on.

### ==Should: Define useful resources==

As a rule of thumb resources should be defined to cover 90% of all its client's use cases. A useful resource should contain as much information as necessary, but as little as possible. A great way to support the last 10% is to allow clients to specify their needs for more/less information by supporting filtering and embedding.

### ==Could: Consider Using (Non-) Nested URLs==

If a sub-resource is only accessible via its parent resource and may not exists without parent resource, consider using a nested URL structure, for instance:

`/orders/1681e6b88ec1/items/1`

However, if the resource can be accessed directly via its unique id, then the API should expose it as a top-level resource. For example, customer is a collection for sales orders; however, sales orders have globally unique id and some services may choose to access the orders directly, for instance:

```
/customers/1681e6b88ec1
/orders/5273gh3k525a
```

### ==Should: Limit number of resources==

To keep maintenance and service evolution manageable, we should follow "functional segmentation" and "separation of concern" design principles and do not mix different business functionalities in same API definition. In this sense the number of resources exposed via API should be limited - our experience is that a typical range of resources for a well-designed API is between 4 and 8. There may be exceptions with more complex business domains that require more resources, but you should first check if you can split them into separate subdomains with distinct APIs.

To ensure **maintainability, scalability, and service evolution**, an API should follow **functional segmentation** and the **separation of concerns** design principles. This means **avoiding the mixing of different business functionalities** within the same API definition.

**Guidelines for API Resource Limitation**

- The number of **resources exposed via an API should be limited** to keep it manageable.
- Based on experience, a **well-structured API typically has between 4 and 8 resources**.
- More complex business domains **may require additional resources**, but before expanding, consider:
    - **Splitting functionalities into subdomains** and designing separate APIs for them.
    - **Ensuring each API serves a clear, distinct purpose** without overlapping responsibilities.

**Why Limit API Resources?**

- **Better Maintainability** – Fewer resources make the API easier to understand and evolve.
- **Improved Scalability** – Avoids monolithic API structures that become difficult to extend.
- **Clearer API Design** – Ensures a logical, well-organized resource structure.
- **Encourages Microservices Approach** – Allows separation of concerns across distinct APIs.

**Example**

**Bad Design (Too Many Mixed Resources in One API)**

```
/users  
/orders  
/products  
/payments  
/notifications  
/reports  
/settings  
/logs  
/shipping  
/reviews  

```

**Problem:** This API **mixes multiple business functionalities** (user management, payments, shipping, etc.), making it harder to scale.

**Good Design (Segmentation into Multiple APIs)**

Instead of a single overloaded API, split it by **domain-specific APIs**:

- **User API:** `/users`, `/profiles`, `/authentication`
- **Order API:** `/orders`, `/payments`, `/shipping`
- **Product API:** `/products`, `/reviews`, `/categories`
- **Notification API:** `/notifications`, `/messages`

**Benefit:** Each API remains **focused, modular, and easier to maintain**.

### ==Should: Limit number of Sub-Resource Levels==

APIs should be designed with **clarity, maintainability, and usability** in mind. This includes **limiting sub-resource (nested) levels** to prevent excessive complexity and long URLs.

- **Main resources** have **root URL paths** (e.g., `/users`).
- **Sub-resources** (or **nested resources**) are **dependent entities** that exist within the scope of a main resource (e.g., `/users/{userId}/orders`).
- Use sub-resources **only if their lifecycle is tightly or loosely coupled** with the parent resource (e.g., an order belongs to a user).
- **Limit sub-resource nesting to a maximum of 3 levels** to maintain API simplicity and usability.

**Why Limit Sub-Resource Levels?**

- **Improves Readability** – Shorter URLs are easier to understand and use.
- **Reduces Complexity** – Deep nesting makes APIs harder to maintain.
- **Prevents URL Length Issues** – Some web browsers have a **2000-character limit** for URLs.
- **Enhances Performance** – Deeply nested resources often require complex database queries.

**Example: Recommended vs. Overly Nested URLs**

**Good (Max 3 Levels)**

```
GET /users/{userId}/orders  
GET /users/{userId}/orders/{orderId}/items  
GET /users/{userId}/addresses/{addressId}  

```

**Bad (Excessive Nesting)**

```
GET /users/{userId}/orders/{orderId}/items/{itemId}/reviews/{reviewId}/comments/{commentId}  

```

## Responses that do not involve resources

API calls that send a response that’s not a resource per se are not uncommon depending on the domain. We’ve seen it in financial services, Telco, and the automotive domain to some extent.

Actions like the following are your clue that you might not be dealing with a “resource” response.

- Calculate
- Translate
- Convert

For example, you want to make a simple algorithmic calculation like how much tax someone should pay, or do a natural language translation (one language in request; another in response), or convert one currency to another. None involve resources returned from a database.

### ==Must: Use verbs not nouns for operation that do not involve resources.==

For example, an API to convert 100 euros to Chinese Yen: /convert?from=EUR\&to=CNY\&amount=100. Make it clear in your API documentation that these “non-resource” scenarios are different.&#x20;

Simply separate out a section of documentation that makes it clear that you use verbs in cases like this – where some action is taken to generate or calculate the response, rather than returning a resource directly.

> If it is a verb that recovers persits data without apply transformations and it doesn't recover additional data is a GET, otherwise, is a POST.

For API operations that **do not correspond to a resource**, use **verbs instead of nouns** to clearly indicate an action is being performed.

**Key Principles**

- **Use verbs for operations that trigger actions or calculations**, rather than retrieving or modifying a stored resource.
- **Make it clear in the API documentation** that these are "non-resource" endpoints by organizing them in a dedicated section.
- **Follow HTTP method best practices**:
    - **Use** **`GET`** if the action retrieves or persists data without transformations and does not return additional computed data.
    - **Use** **`POST`** if the action processes input, applies transformations, or returns computed results.
    - Use **`POST`** if the request triggers complex processing or requires a request body

**Examples**

**Good (Using a Verb for an Action-Based API)**

```http
GET /convert?from=EUR&to=CNY&amount=100  
POST /generate-report  
POST /send-notification  
---
POST /hotel-rooms/search-availability 
Body:
{
  "hotelId": "1234",
  "checkin": "2025-03-10",
  "checkout": "2025-03-15",
  "guests": 2,
  "rooms": 1
}
```

**Bad (Using a Noun for an Action-Based API)**

```
GET /conversion?from=EUR&to=CNY&amount=100  
POST /report  
POST /notification
```

### ==Must: Expecify and document in the API documentation or SWAGGER that that operation do not involve a resource.==

## HTTP

### ==Must: Use HTTP Methods Correctly==

Be compliant with the standardized HTTP method semantics summarized as follows:

#### GET

GET requests are used to **read** either a single or a collection resource.

- GET requests for individual resources will usually generate a 404 if the resource does not exist
- GET requests for collection resources may return either 
  - **200** (if the collection is empty)&#x20;
- **404** (if the collection is missing)
- GET requests must NOT have a request body payload (see GET With Body)

**Note**: GET requests on collection resources should provide sufficient filter and Pagination mechanisms.

#### PUT

PUT requests are used to **update** (in rare cases to create) entire resources – single or collection resources.&#x20;

The semantic is best described as *"please put the enclosed representation at the resource mentioned by the URL, replacing any existing resource.".*

- PUT requests are usually applied to single resources, and not to collection resources, as this would imply replacing the entire collection
- PUT requests are usually robust against non-existence of resources by implicitly creating before updating
- On successful PUT requests, the server will replace the entire resource addressed by the URL with the representation passed in the payload (subsequent reads will deliver the same payload)
- Successful PUT requests will usually generate 
  - **200 or 204** (if the resource was updated – with or without actual content returned)
- **201** (if the resource was created)

**Important**: It is best practice to prefer POST over PUT for creation of (at least top-level) resources. This leaves the resource ID under control of the service and allows to concentrate on the update semantic using PUT as follows.

**Note**: In the rare cases where PUT is although used for resource creation, the resource IDs are maintained by the client and passed as a URL path segment.&#x20;

Putting the same resource twice is required to be idempotent and to result in the same single resource instance **(see Must: Fulfill Common Method Properties)**.

#### POST

POST requests are idiomatically used to **create** single resources on a collection resource endpoint, but other semantics on single resources endpoint are equally possible. The semantic for collection endpoints is best described as *"please add the enclosed representation to the collection resource identified by the URL"*.

- On a successful POST request, the server will create one or multiple new resources and provide their URI/URLs in the response
- Successful POST requests will usually generate 
  - **200** (if resources have been updated).
- **201** (if resources have been created).
- **202** (if the request was accepted but has not been finished yet)
- **204** exceptionally  with Location header (if the actual resource is not returned).

The semantic for single resource endpoints is best described as *"please execute the given well specified request on the resource identified by the URL".*

**Generally**: POST should be used for scenarios that cannot be covered by the other methods sufficiently. In such cases, make sure to document the fact that POST is used as a workaround

**Note**: Resource IDs with respect to POST requests are created and maintained by server and returned with response payload.

#### PATCH

PATCH requests are used to **update** parts of single resources, i.e. where only a specific subset of resource fields should be replaced.&#x20;

The semantic is best described as *"please change the resource identified by the URL according to my change request"*.&#x20;

The semantic of the change request is not defined in the HTTP standard and must be described in the API specification by using suitable media types.

- PATCH requests are usually applied to single resources as patching entire collection is challenging.
- PATCH requests are usually not robust against non-existence of resource instances.
- On successful PATCH requests, the server will update parts of the resource addressed by the URL as defined by the change request in the payload.
- Successful PATCH requests will usually generate 
  - **200 or 204** (if resources have been updated with or without updated content returned).

**Note**: Since implementing PATCH correctly is a bit tricky, we strongly suggest to choose one and only one of the following patterns per endpoint, unless forced by a backwards compatible change. In preference order:

- Use [PUT](https://datatracker.ietf.org/doc/html/rfc9110#name-put "RFC 9110: HTTP Semantics") with complete objects to update a resource as long as feasible (i.e. do not use PATCH at all).
- Use [PATCH](https://datatracker.ietf.org/doc/html/rfc5789 "RFC 5789: PATCH Method for HTTP") with partial objects to only update parts of a resource, whenever possible. (This is basically JSON Merge Patch, a specialized media type application/merge-patch+json that is a partial resource representation.)
- Use [PATCH](https://datatracker.ietf.org/doc/html/rfc5789 "RFC 5789: PATCH Method for HTTP") with JSON Patch, a specialized media type application/json-patch+json that includes instructions on how to change the resource.
- Use [POST](https://datatracker.ietf.org/doc/html/rfc9110#section-9.3.3 "RFC 9110: HTTP Semantics") (with a proper description of what is happening) instead of PATCH, if the request does not modify the resource in a way defined by the semantics of the media type.

In practice JSON Merge Patch quickly turns out to be too limited, especially when trying to update single objects in large collections (as part of the resource). In this cases [JSON Patch](https://datatracker.ietf.org/doc/html/rfc6902 "RFC 6902: JavaScript Object Notation (JSON) Patch") can shown its full power while still showing readable patch requests (see also [JSON patch vs. merge](http://erosb.github.io/post/json-patch-vs-merge-patch)).

#### DELETE

DELETE requests are used to delete resources. The semantic is best described as *"please delete the resource identified by the URL".*

DELETE requests are usually applied to single resources, not on collection resources, as this would imply deleting the entire collection

successful DELETE requests will usually generate 

- 200 (if the deleted resource is returned)&#x20;
- 204 (if no content is returned)
-  404 (if the resource cannot be found)&#x20;
- 410 (if the resource was already deleted before)

**Important**: After deleting a resource with DELETE, a GET request on the resource is expected to either return 404 (not found) or 410 (gone) depending on how the resource is represented after deletion. Under no circumstances the resource must be accessible after this operation on its endpoint.

#### HEAD

- HEAD requests are used to retrieve the header information of single resources and resource collections.
- HEAD has exactly the same semantics as GET, but returns headers only, no body.
- **Hint**: HEAD is particular useful to efficiently lookup whether large resources or collection resources have been updated in conjunction with the ETag-header.

#### OPTIONS

- OPTIONS requests are used to inspect the available operations (HTTP methods) of a given endpoint.
- OPTIONS responses usually either return a comma separated list of methods in the Allow header or as a structured list of link templates

**Note**: OPTIONS is rarely implemented, though it could be used to self-describe the full functionality of a resource.

### ==Must: Use HTTP status codes==

| **STATUS CODE** | **DESCRIPTION**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **200**         | **OK** Standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request, the response will contain an entity describing or containing the result of the action.                                                                                                                                                                                       |
| 201             | **Created** The request has been fulfilled, resulting in the creation of a new resource.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 202             | **Accepted** The request has been accepted for processing, but the processing has not been completed. The request might or might not be eventually acted upon, and may be disallowed when processing occurs                                                                                                                                                                                                                                                                                                       |
| 204             | **No Content** The server successfully processed the request and is not returning any content                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 304             | **Not Modified** Indicates that the resource has not been modified since the version specified by the request headers If-Modified-Since or If-None-Match. In such case, there is no need to retransmit the resource since the client still has a previously-downloaded copy.                                                                                                                                                                                                                                      |
| 400             | **Bad Request** The server cannot or will not process the request due to an apparent client error (e.g., malformed request syntax, too large size, invalid request message framing, or deceptive request routing)                                                                                                                                                                                                                                                                                                 |
| 401             | **Unauthorized** Similar to 403 Forbidden, but specifically for use when authentication is required and has failed or has not yet been provided. The response must include a WWW-Authenticate header field containing a challenge applicable to the requested resource.                                                                                                                                                                                                                                           |
| 404             | **Not Found** The requested resource could not be found but may be available in the future. Subsequent requests by the client are permissible.                                                                                                                                                                                                                                                                                                                                                                    |
| 406             | **Not Acceptable** The requested resource is capable of generating content but the request is not acceptable according to the Accept or Accept-Version headers sent in the request.                                                                                                                                                                                                                                                                                                                               |
| 409             | **Conflict** Indicates that the request could not be processed because of conflict in the request, such as an edit conflict between multiple simultaneous updates.                                                                                                                                                                                                                                                                                                                                                |
| 410             | **Gone** Indicates that the resource requested is no longer available and will not be available again. This should be used when a resource has been intentionally removed and the resource should be purged. Upon receiving a 410 status code, the client should not request the resource in the future. Clients such as search engines should remove the resource from their indices. Most use cases do not require clients and search engines to purge the resource, and a "404 Not Found" may be used instead. |
| 429             | **Too Many Requests** The user has sent too many requests in a given amount of time.                                                                                                                                                                                                                                                                                                                                                                                                                              |
| 500             | **Internal Server Error** A generic error message, given when an unexpected condition was encountered andno more specific message is suitable.                                                                                                                                                                                                                                                                                                                                                                    |
| 501             | **Not Implemented** The server either does not recognize the request method, or it lacks the ability to fulfil the request. Usually this implies future availability (e.g., a new feature of a web-service API).                                                                                                                                                                                                                                                                                                  |

## Sorting

### ==Might: Use sorting in the HTTP Get Request==

In case you want to provide a query parameter to sort the http response, the current microservice archetype by means of Jade library supports the sorting feature. To do this, you only have to include the "sort" attribute as a query parameter in the request.

`example: /dogs?sort=+name, +race | /dogs?sort=-name, -race`

## Pagination (Not needed for reactive endpoints)

### ==Must: Support Pagination for synchronous services==

Access to lists of data items must support pagination for best client side batch processing and iteration experience. This holds true for all lists that are (potentially) larger than just a few hundred entries.

### ==Must: Use Offset and limit operators==

The offset and limit operators are used for pagination. Access to lists of data items must support pagination for best client-side batch processing and iteration experience. This holds true for all lists that are (potentially) larger than just a few hundred entries.

**Offset/Limit-based pagination:** numeric offset identifies thefirst page entry

- Use limit and offset. It is more common, well understood in leading databases, and easy for developers. `/dogs?limit=25&offset=50`
- Include metadata with each response that is paginated that indicated to the developer the total number of records available.

**Note**: It is not a must if the api calls to an external system that it has not limit and offset.

### ==Must: Define a maximum value for the limit query parameter.==

In the case the limit value execeeds the maximum value established, the service will respond with an HTTP Error 400

### ==Must: Not allowed to retrieve the whole resource collection without setting a limit==

It is not allowed for security reasons to enable any kind of backdoors that allows to retrieve the wholse set of a collection. That is, you cannot set  to zero the limit value.

### ==Must: Include Pagination Http Response Header==

For all the HTTP GET request, the HTTP response must include the next HTTP Headers:

1. number-of-elements
2. total-counts
3. total-pages

Example below:

```http
GET /products HTTP/1.1 HTTP/1.1 200 OK

cache-control: no store, private, max-age=0
content-length: 144
content-type: application/vnd.foo-api.v1+json;charset=UTF-8
date: Mon, 11 Nov 2019 11:53:18 GMT
etag: "06960d24ee58a147eb02cb296c43f7568"
number-of-elements: 2
total-count: 2
total-pages: 1
trace-id: 73306c019b9271a0
```

## Partial response

Partial response allows you to give developers just the information they need.

By default, the server sends back the full representation of a resource after processing requests. For better performance, you can ask the server to send only the fields you really need and get a partial response instead.

To request a partial response, use the fields request parameter to specify the fields you want returned . You can use this parameter with any request that returns a response body.

### ==Must: Use Fields operator==

In the case you want to allow partial response, you have use the fields operator. Fields allows you to choose what fields to return from an entity. You can choose multiple values by combining comma separated operators.

Field Selectors are the simplest form of query parameters, consisting only of a comma-separated list of field names on the response object.

`fields={field 1},{field 2},...{field n}`

Fields are returned in the order in which they are specified in the query.

In case you do not use fields operator, you have to disable the code intercepetor by means of an annotation from the Jaspe Library: @DisableJaspe

See: <https://git.tui-ds.com/architecture/jaspe-v2/blob/master/README.md> (REST API Partial Response Library for Spring Boot Applications)

**Understanding the fields parameter**

The fields parameter filters the API response so that the response only includes a specific set of fields. The fields parameter lets you remove nested properties from an API response and thereby reduce your bandwidth usage.

The following rules explain the supported syntax for the fields parameter value, which is loosely based on XPath syntax:

- Use a comma-separated list (fields=a,b) to select multiple fields.
- Use an asterisk (fields=\*) as a wildcard to identify all fields.
- Use parentheses (fields=a(b,c)) to specify a group of nested properties that will be included in the API response.
- Use a dot-separated (fields=a.b,a.c)) to specify a group of nested properties that will be included in the API response.

In practice, these rules often allow several different fields parameter values to retrieve the same API response. For example, if you want to retrieve the playlist item ID, title, and position for every item in a playlist, you could use any of the following values:

```http
fields=items(id,snippet(title,position))
fields=items.id,items.snippet(title,position))
fields=items.id,items.snippet.title,items.snippet.position
```

**Note**: As with all query parameter values, the fields parameter value must be URL encoded. For better readability, the examples in this document omit the encoding.

## Filtering

### ==Might: Support filtering==

In case the API requieres filtering, there are two differents ways of achiving this:

1. Dynamic Filtering
2. Static Filtering

### ==Might: Support Dynamic Filtering==

Use RSQL for dynamic filtering. RSQL is a query language for parametrized filtering of entries in RESTful APIs. It’s based on FIQL (Feed Item Query Language), that was originally specified by Mark Nottingham as a language for querying Atom feeds. However the simplicity of RSQL and its capability to express complex queries in a compact and HTTP URI-friendly way makes it a good candidate for becoming a generic query language for searching REST endpoints.

RSQL introduces simple and composite operators which can be used to build basic and complex queries.&#x20;

**The following table lists basic operators:**

| Basic Operator | Description         |
| -------------- | ------------------- |
| \==            | Equal To            |
| !=             | Not Equal To        |
| =gt=           | Greater Than        |
| =ge=           | Greater Or Equal To |
| =lt=           | Less Than           |
| =le=           | Less Or Equal To    |
| =in=           | In                  |
| =out=          | Not in              |
| =isnull=       | Is null value       |
| =isempty=      | Is empty value      |

**The following table lists two joining operators:**

| Composite Operator | Description |
| ------------------ | ----------- |
| ;                  | Logical AND |
| ,                  | Logical OR  |

We use <https://github.com/jirutka/rsql-parser> as RSQL implementation.

### ==Must: Use filters operator for dynamic filtering==

To build the query with RSQL you have to use filters operator as is shown below:

`filters=age=gt=10;age=lt=20;(str=Fero,str=Hero)`

### ==Must: Indicate which are the paremeters you allow to filter by filters operator==

In the case of using RSQL, the API specification must indicate which are the parameters by it can be filtered. In the same way, the code must only enable filtering by the fields mentioned in the specification.

### ==Should: Use query parameters for mandatory fields==

In case a resource operation requires filtering by a mandatory parameter, it is recommended to use the query parameter, however, if you want to use RSQL which is also allowed.

## Handling errors

Error handling it is a very important piece of the puzzle for any software developer, and especially for API designers.

### ==Must: The error message payload and the header must include the following attributes==

- The status code in the response header.
- The developer error message in the payload.
- The error code in the payload.

```http
HTTP Status Code: 404
{
  "code":  "Prefix-ERROR-errorCode",
  "message":  "A short developer message information"
}
```

## Data Formats

## Common Data Objects

## Common Headers

### ==Must: Use Content Headers Correctly==

Content or entity headers are headers with a Content- prefix. They describe the content of the body of the message and they can be used in both, HTTP requests and responses.&#x20;

Commonly used content headers include but are not limited to:

- **Content-Disposition** can indicate that the representation is supposed to be saved as a file, and the proposed file name.
- **Content-Encoding** indicates compression or encryption algorithms applied to the content.
- **Content-Length** indicates the length of the content (in bytes).
- **Content-Language** indicates that the body is meant for people literate in some human language(s).
- **Content-Location** indicates where the body can be found otherwise (see below for more details).
- **Content-Range** is used in responses to range requests to indicate which part of the requested resource representation is delivered with the body.
- **Content-Type** indicates the media type of the body content.

### ==Must: Use Content-Location Correctly==

This header is used in the response of either a successful read (GET, HEAD) or successful write operation (PUT, POST or PATCH).

In the case of the GET requests it points to a location where an alternate representation of the entity in the response body can be found. In this case one has to set the Content-Type header as well.&#x20;

**For example:**

```http
GET /products/123/images HTTP/1.1 HTTP/1.1 200 OK
Content-Type: image/png
Content-Location: /products/123/images?format=raw
```

In the case of mutating HTTP methods, the Content-Location header can be used when there is a response body, and then it indicates that the included response body can be found at the location indicated in the header.

If the header value is the same as the location of the created resource (as indicated by the Location header after POST) or the modified resource (as indicated by the request URI after PUT / PATCH), then the returned body is indeed the current representation of the entity making a subsequent GET operation from the client side not necessary.

If your API returns the new representation after a PUT, PATCH, or POST you should include the Content-Location header to make it explicit, that the returned resource is an up-to-date version.

[More details in rfc7231](https://datatracker.ietf.org/doc/html/rfc9110 "RFC 9110: HTTP Semantics")

### ==Must: Use Sleuth Trace-Id Header==

All the HTTP Requests and Responses must include the Trace-Id Headers.&#x20;

**For Example:**

```http
GET /products HTTP/1.1 HTTP/1.1 200 OK
Content-Type: application/json
Trace-Id : 73306c019b9271a0
```

## Custom Headers.

### ==SHOULD NOT prefix their parameter names with "X-" or similar constructs.==

### ==SHOULD NOT prohibit parameters with an "X-" prefix or similar constructs from being registered.==

### ==MUST NOT stipulate that a parameter with an "X-" prefix or similar constructs needs to be understood as unstandardized.==

### ==MUST NOT stipulate that a parameter without an "X-" prefix or similar constructs needs to be understood as standardized==

See: <https://tools.ietf.org/html/rfc6648>

---
