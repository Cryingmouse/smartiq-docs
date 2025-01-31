# **Restful API Specification**

This document outlines the specification for a Restful API, including URL definitions, header definitions, and HTTP
status codes for responses. Examples are provided to illustrate the usage.

<!-- TOC -->
* [Restful API Specification](#restful-api-specification)
  * [URL Definitions](#url-definitions)
    * [Base URL](#base-url)
    * [Endpoints](#endpoints)
  * [Header Definitions](#header-definitions)
    * [Common Headers](#common-headers)
    * [Example Headers](#example-headers)
  * [HTTP Status Codes](#http-status-codes)
    * [Success Codes](#success-codes)
    * [Error Codes](#error-codes)
  * [Examples](#examples)
    * [Example 1: Retrieve a List of Resources with Locale Support](#example-1-retrieve-a-list-of-resources-with-locale-support)
    * [Example 2: Retrieve a Specific Resource by ID](#example-2-retrieve-a-specific-resource-by-id)
    * [Example 3: Create a New Resource with Locale Support](#example-3-create-a-new-resource-with-locale-support)
    * [Example 4: Update a Specific Resource with Locale Support](#example-4-update-a-specific-resource-with-locale-support)
    * [Example 5: Delete a Specific Resource](#example-5-delete-a-specific-resource)
    * [Example 6: Unauthorized Access with Locale Support](#example-6-unauthorized-access-with-locale-support)
    * [Example 7: Internal Server Error with Locale Support](#example-7-internal-server-error-with-locale-support)
    * [Example 8: Resource Not Found](#example-8-resource-not-found)
    * [Example 9: Unprocessable Entity (Semantic Validation Error)](#example-9-unprocessable-entity-semantic-validation-error)
<!-- TOC -->

## **URL Definitions**

### **Base URL**

All API requests should be made to the base URL:

    https://api.example.com/v1/

### **Endpoints**

- **GET** `/resources` - Retrieve a list of resources.
- **GET** `/resources/{id}` - Retrieve a specific resource by ID.
- **POST** `/resources` - Create a new resource.
- **PUT** `/resources/{id}` - Update a specific resource by ID.
- **DELETE** `/resources/{id}` - Delete a specific resource by ID.

## **Header Definitions**

### **Common Headers**

- **Content-Type**: Specifies the media type of the resource. Default is `application/json`.
- **Accept**: Specifies the media type that the client can accept. Default is `application/json`.
- **Authorization**: Contains the credentials to authenticate a user agent with a server.
- **Accept-Language**: Specifies the preferred language for the response. Used for locale support. Default is `en-US`.

### **Example Headers**

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token>
Accept-Language: en-US
```

## **HTTP Status Codes**

### **Success Codes**

- **200 OK**: The request has succeeded.
- **201 Created**: The request has been fulfilled and resulted in a new resource being created.
- **204 No Content**: The server successfully processed the request, but is not returning any content.

### **Error Codes**

- **400 Bad Request**: The server cannot process the request due to a client error.
- **401 Unauthorized**: The request requires user authentication.
- **403 Forbidden**: The server understood the request, but is refusing to fulfill it.
- **404 Not Found**: The server has not found anything matching the Request-URI.
- **422 Unprocessable Entity**: The request was well-formed but contains semantic errors (e.g., validation errors).
- **500 Internal Server Error**: The server encountered an unexpected condition which prevented it from fulfilling the
  request.

## **Examples**

### **Example 1: Retrieve a List of Resources with Locale Support**

**Request:**

```http
GET /resources HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer <token>
```

**Response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

response body:
```json
{
  "items": [
    {
      "id": 1,
      "name": "Ressource 1"
    },
    {
      "id": 2,
      "name": "Ressource 2"
    },
    {
      "id": 3,
      "name": "Ressource 3"
    },
    {
      "id": 4,
      "name": "Ressource 4"
    },
    {
      "id": 5,
      "name": "Ressource 5"
    },
    {
      "id": 6,
      "name": "Ressource 6"
    },
    {
      "id": 7,
      "name": "Ressource 7"
    },
    {
      "id": 8,
      "name": "Ressource 8"
    },
    {
      "id": 9,
      "name": "Ressource 9"
    },
    {
      "id": 10,
      "name": "Ressource 10"
    }
  ],
  "current_page": 11,
  "page_size": 10,
  "total": 102
}
```

### **Example 2: Retrieve a Specific Resource by ID**

**Request:**

```http
GET /resources/1 HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer <token>
Accept-Language: en-US
```

**Response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

response body:
```json
{
  "id": 1,
  "name": "Resource 1"
}
```    

### **Example 3: Create a New Resource with Locale Support**

**Request:**

```http
POST /resources HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <token>
```

request body：
```json
{
  "name": "Nuevo Recurso"
}
```    

**Response:**

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /resources/3
```

response body:
```json
{
  "id": 3,
  "name": "Nuevo Recurso"
}
```    

### **Example 4: Update a Specific Resource with Locale Support**

**Request:**

```http
PUT /resources/3 HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <token>
Accept-Language: zh-CN
```

request body:
```json
{
  "name": "Aktualisierte Ressource"
}
```    

**Response:**

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

response body:
```json
{
  "id": 3,
  "name": "Aktualisierte Ressource"
}
```    

### **Example 5: Delete a Specific Resource**

**Request:**

```http
DELETE /resources/3 HTTP/1.1
Host: api.example.com
Authorization: Bearer <token>
```

**Response:**

```http
HTTP/1.1 204 No Content
```

### **Example 6: Unauthorized Access with Locale Support**

**Request:**

```http
GET /resources HTTP/1.1
Host: api.example.com
Accept: application/json
Accept-Language: zh-CN
```

**Response:**

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json
```

response body:
```json
{
  "error": "未经授权",
  "message": "未提供身份验证凭据。"
}
```    

### **Example 7: Internal Server Error with Locale Support**

**Request:**

```http
GET /resources HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer <token>
```

**Response:**

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json
```

response body:
```json
{
  "code": "SERVER_001",
  "message": "An unexpected error occurred on the server."
}
```

### **Example 8: Resource Not Found**

**Request:**

```http
GET /resources/999 HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer <token>
Accept-Language: en-US
```

**Response:**

```http
HTTP/1.1 404 Not Found
Content-Type: application/json
```

response body:
```json
{
  "code": "RESOURCE_001",
  "message": "The requested resource was not found."
}
``` 

### **Example 9: Unprocessable Entity (Semantic Validation Error)**

**Request:**

```http
POST /resources HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer <token>
Accept-Language: en-US
```

request body:
```json
{
  "name": "Valid Name",
  "email": "invalid-email"
}
```

**Response:**

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json
```

response body:
```json
{
  "code": "e42200000",
  "message": "unprocessable entity",
  "data": [
    {
      "type": "ip_any_address",
      "loc": [
        "body",
        "primary"
      ],
      "msg": "value is not a valid IPv4 or IPv6 address",
      "input": "asd0"
    },
    {
      "type": "ip_any_address",
      "loc": [
        "body",
        "secondary",
        1
      ],
      "msg": "value is not a valid IPv4 or IPv6 address",
      "input": "10.10.12"
    }
  ]
}
```

