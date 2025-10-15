# 🎮 GGChest Seller API

![Version](https://img.shields.io/badge/version-1.0.1-blue.svg)

A comprehensive RESTful API for managing game offers, accounts, items, and currencies on the GGChest platform.

API Update 2025-10-15, v1.0.1
	•	Added endpoints for bulk offer deletion (`DELETE /v1/offers`)
	•	Added endpoints for bulk activation and deactivation of offers (`PUT /v1/offers/activate` and `PUT /v1/offers/draft`)
  

## 📋 Table of Contents

- [🚀 Overview](#-overview)
  - [Base URL](#base-url)
  - [API Version](#api-version)
- [✨ Features](#-features)
- [🏁 Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Authentication](#authentication)
- [📡 API Endpoints](#-api-endpoints)
  - [Games](#games)
  - [Attributes](#attributes)
  - [Offers](#offers)
  - [Image Management](#image-management)
- [📋 API Errors](#-api-errors)
- [⚠️ Error Handling](#️-error-handling)
- [🚦 Rate Limiting](#-rate-limiting)
- [📞 Support](#-support)
- [🔄 Changelog](#-changelog)

## 🚀 Overview

The GGChest Seller API enables sellers to manage their offers, accounts, items, and currency offers directly with the GGChest marketplace.

### Base API host
```
https://sellerapi.ggchest.com
```

### API Version
```
Version: 1.0.0
```

## ✨ Features

- 🔐 **Secure Authentication** - API Key authentication with X-API-KEY header
- 🎯 **Offer Management** - Create, update, delete and manage your offers
- 🔄 **Offer activation** - Enable/disable offers
- 💲 **Price Updates** - Real-time price management

## 🏁 Getting Started

### Prerequisites

- Verified GGChest Seller Account
- API Key (obtained from GGChest Support)
- HTTP client for API integration

### Authentication

The GGChest Seller API uses API Key authentication. All requests must include an `X-API-KEY` header.

#### Getting Your API Key

To obtain a personal API key, you need to contact customer support (support@ggchest.com). At the moment, the support team will generate a key for you and send it to the email address you provided when registering your account. In the future, we will allow any seller to generate and manage their API keys independently from their dashboard.

#### Using the API Key

Include the API key in all requests:

```http
X-API-KEY: your_api_key_here
```

**Example Request:**
```bash
curl -X GET "https://sellerapi.ggchest.com/v1/offers/accounts" \
  -H "X-API-KEY: your_api_key_here"
```

## 📡 API Endpoints

All endpoints require authentication via the `X-API-KEY` header.

### Games

#### Get Games list

```http
GET /v1/games
```

Retrieve all games.

**Query Parameters:**
- `filters[offer_type]` (string, optional): Offer type:  `items`, `currency` or `accounts`

**Response:**
```json
[
  {
    "id": "01985a2f-3d47-71e8-9b2c-4f5e8d9a1b3c",
    "name": "Call of Duty: Black Ops 6",
    "icon": {
      "id": "01985a2f-4b8c-72d1-a3e7-6f9d2c8e5a7b",
      "path": "https://storage.googleapis.com/ggchest-public/games/01985a2f-3d47-71e8-9b2c-4f5e8d9a1b3c/icon.png"
    }
  }
]
```

#### Get Currency Offer

```http
GET /v1/games/{id}/currency
```

Get all offers in the Currencies category

**Path Parameters:**
- `id` (string, required): The game identifier

**Example Request:**
```bash
curl -X GET "https://sellerapi.ggchest.com/v1/games/01985a2f-3d47-71e8-9b2c-4f5e8d9a1b3c/currency" \
  -H "X-API-KEY: your_api_key_here"
```

**Response:**
```json
[
  {
    "id": "01985a34-2c8f-7bd3-a5e9-8f3c1e5d7a9b",
    "name": "V-Bucks",
    "nominal": "VB",
    "icon": {
      "id": "01985a34-4e1a-7cd5-b7f2-9d5a3c7e1b4d",
      "path": "https://storage.googleapis.com/ggchest-public/currencies/01985a34-2c8f-7bd3-a5e9-8f3c1e5d7a9b/icon.png"
    }
  },
  {
    "id": "01985a34-6f3b-7ed7-c9a4-2e8f5a1c9d6b",
    "name": "FIFA Points",
    "nominal": "FP",
    "icon": {
      "id": "01985a34-8a5c-7fe9-d1b6-4f2a8c5e3a7d",
      "path": "https://storage.googleapis.com/ggchest-public/currencies/01985a34-6f3b-7ed7-c9a4-2e8f5a1c9d6b/icon.png"
    }
  }
]
```

### Attributes

```http
GET /v1/games/{id}/{offer_type}/attributes
```

Retrieve all offer attributes by game and offer type.

**Response:**
```json
[
  {
    "id": "01985a30-7f2b-73c4-8d6e-9a1b5c7f3e2d",
    "attribute_name": "Platform",
    "options": [
      {
        "id": "01985a30-9e4f-74a7-b5c8-2d6f8a9c1e3b",
        "name": "PlayStation 5"
      },
      {
        "id": "01985a30-b8d2-75e9-c4f7-3e9a7b5d8c2f",
        "name": "Xbox Series X"
      }
    ]
  }
]
```

### Offers

#### Get Offers

```http
GET /v1/offers
```

Retrieve all offers for the authenticated seller with pagination support.

**Query Parameters:**
- `pagination[page]` (integer, optional): Page number (default: 1)
- `pagination[per_page]` (integer, optional): Items per page (default: 10, max: 100)
- `filters[status]` (string, optional): Filter by status - `ACTIVE`, `DRAFT`
- `filters[offer_type]` (string, optional): Filter by offer type - `accounts`, `items`, `currency`
- `filters[game_id]` (string, optional): Filter by game identifier

**Example Request:**
```bash
curl -X GET "https://sellerapi.ggchest.com/v1/offers?pagination[page]=1&pagination[per_page]=10&filters[status]=ACTIVE&filters[offer_type]=currency" \
  -H "X-API-KEY: your_api_key_here"
```

**Response:**
```json
{
  "items": [
    {
      "id": "019856a7-6e02-7320-9edc-a2e94ecc6807",
      "offer_type": "currency",
      "title": "EA FC 25 (FC Coins)",
      "description": "Premium FIFA currency for EA FC 25",
      "price": "1.34",
      "status": "DRAFT",
      "delivery_method": "manual",
      "delivery_time": "1h",
      "qty_total": "10",
      "qty_min": "1",
      "page_slug": "ea-fc-25-fc-coins",
      "game_id": "0192b854-d37a-71d5-9ae5-ad763615171a",
      "game_name": "EA FC 25",
      "attributes": [
        {
          "attribute_id": "01932aae-5546-710f-aa89-52830e51c47a",
          "attribute_name": "Platform",
          "option_id": "01932aae-b1ee-70df-a8f7-57b502b22924",
          "option_name": "Playstation"
        }
      ],
      "expired_at": "2025-08-28 14:47:50",
      "icon": {
        "id": null,
        "path": null
      },
      "extra_properties": {
        "game_currency": {
          "id": "0192b855-6a77-73d0-8c38-509c68ebc45c",
          "name": "FC Coins",
          "nominal": "B",
          "icon": {
            "id": "01932aab-64dc-7239-a4f5-4a471c14aa17",
            "path": "https://storage.googleapis.com/dev-ggchest-public/market/game/currency/0192b855-6a77-73d0-8c38-509c68ebc45c/01932aab-5f5f-71f2-908c-bb4768ebec1e.png"
          }
        }
      }
    }
  ],
  "pagination": {
    "total_count": 40,
    "page_count": 4,
    "page": 1,
    "per_page": 10
  }
}
```

**Response Fields:**
- `items` (array): Array of offer objects
  - `id` (string): Unique offer identifier
  - `offer_type` (string): Type of offer (`currency`, `accounts`, `items`)
  - `title` (string): Offer title
  - `description` (string): Offer description
  - `price` (string): Price in USD (decimal format)
  - `status` (string): Offer status (`ACTIVE`, `DRAFT`)
  - `delivery_method` (string): Delivery method (`manual`, `auto`)
  - `delivery_time` (string): Expected delivery time (`5m`, `20m`, `1h`, `5h`, `12h`, `1d`, `3d`, `7d`, `14d`, `28d` )
  - `qty_total` (string): Total quantity available (string format)
  - `qty_min` (string): Minimum purchase quantity (string format)
  - `page_slug` (string, nullable): URL-friendly slug for the offer page
  - `game_id` (string): Associated game identifier
  - `game_name` (string): Game display name
  - `attributes` (array): Game-specific attributes and options
    - `attribute_id` (string): Attribute identifier
    - `attribute_name` (string): Attribute display name
    - `option_id` (string): Option identifier
    - `option_name` (string): Option display name
  - `expired_at` (string): Offer expiration date (ISO format)
  - `icon` (object): Offer icon information
    - `id` (string): Icon identifier (nullable)
    - `path` (string): Icon file path (nullable)
  - `extra_properties` (object): Additional type-specific properties
    - `game_currency` (object|null): Currency-specific properties for currency offers
      - `id` (string): Currency identifier
      - `name` (string): Currency name
      - `nominal` (string): Currency nominal value
      - `icon` (object): Currency icon information

- `pagination` (object): Pagination information
  - `total_count` (integer): Total number of offers
  - `page_count` (integer): Total number of pages
  - `page` (integer): Current page number
  - `per_page` (integer): Items per page

#### Get Specific Offer

```http
GET /v1/offers/{id}
```

Retrieve detailed information about a specific offer by its ID.

**Path Parameters:**
- `id` (string, required): The offer identifier

**Example Request:**
```bash
curl -X GET "https://sellerapi.ggchest.com/v1/offers/01985a35-c7d2-9fa4-e1b6-8a3f5c2e9d7b" \
  -H "X-API-KEY: your_api_key_here"
```

**Response:**
```json
{
  "id": "01985a35-c7d2-9fa4-e1b6-8a3f5c2e9d7b",
  "title": "Premium Gaming Account - Level 85+",
  "offer_type": "accounts",
  "description": "High-level gaming account with rare items and achievements",
  "price": "129.99",
  "status": "ACTIVE",
  "delivery_time": "1d",
  "delivery_method": "manual",
  "qty_total": 5,
  "qty_min": 1,
  "game_id": "01985a35-e9f3-a1d7-c4b8-6e2a9c5f8d1b",
  "icon": {
    "id": "01985a35-fb5e-a2e9-d5c1-9f4a7c3e1b6d",
    "path": "https://storage.googleapis.com/ggchest-public/offers/icons/01985a35-fb5e-a2e9-d5c1-9f4a7c3e1b6d.png"
  },
  "attributes": [
    {
      "id": "01985a36-1d7f-a4fb-e7d3-2a6c9f5e8c2d",
      "attribute_name": "Platform",
      "option_id": "01985a36-3f1b-a6ed-f9e5-4c8e2a7f1d4b",
      "option_name": "PlayStation 5"
    }
  ],
  "extra_properties": {
    "accounts": [
      {
        "id": "01985a36-5b3d-a8ef-fb17-6e1f4a9c3e5d",
        "description": "Level 85 character with legendary gear",
        "login": "epic*****",
        "password": "****",
        "email_login": "play****@email.com",
        "email_password": "****"
      }
    ],
    "game_currency": null
  }
}
```

#### Delete Offer

```http
DELETE /v1/offers/{id}
```

Permanently delete an offer. This action cannot be undone.

**Path Parameters:**
- `id` (string, required): The offer identifier

**Example Request:**
```bash
curl -X DELETE "https://sellerapi.ggchest.com/v1/offers/01985a36-7e5a-ab2f-fd39-8c4f6a1e7d9b" \
  -H "X-API-KEY: your_api_key_here"
```

**Response (200 - Success):**
```json
{}
```

#### Bulk offer deletion

```http
DELETE /v1/offers
```
**Body Request**

```json
{
    "offers_ids": [
        "01997b76-07ca-7ce2-a5bb-7124bdf09cd6",
        "01997b76-514b-712f-a326-dbf46d00b082"
    ]
}
```

Validation rules:

1.	The list of offer_ids must contain unique UUID strings (otherwise, a 400 response code is expected).
2.	The UUID array must not be empty — at least one UUID must be provided in the array.
3.	The maximum number of UUIDs that can be passed in the array is 100. If exceeded, a 400 response code will be returned.

#### Create New Account Offer

```http
POST /v1/offers/accounts
```

Create a new account offer with game accounts for sale.

**Request Body:**
```json
{
  "game_id": "01985a31-2c8e-76d3-9f5b-4a7c9e2d1f6b",
  "title": "Premium Gaming Account - Level 80+",
  "description": "High-level gaming account with rare items and achievements",
  "price": "66.32",
  "delivery_method": "manual",
  "delivery_time": "1d",
  "qty_total": 0,
  "qty_min": 0,
  "attributes": [
    {
      "id": "01985a31-5e7d-77f8-a2c9-6b8e4f1a3d7c",
      "option_id": "01985a31-7f9b-78e4-c5d8-9a2f6e4b8c1d"
    }
  ],
  "accounts": [
    {
      "login": "epicgamer2024",
      "password": "SecurePass123!",
      "email_login": "player@email.com",
      "email_password": "EmailPass456!",
      "description": "Level 85 character with legendary gear"
    }
  ]
}
```

**Required Fields:**
- `game_id` (string): The ID of the game this offer is for
- `title` (string): Offer title/name
- `description` (string): Detailed description of the offer
- `price` (string): Price in decimal format (e.g., "66.32"). Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_method` (string): Delivery method - either `"manual"` or `"auto"`
- `delivery_time` (string): Expected delivery time (`5m`, `20m`, `1h`, `5h`, `12h`, `1d`, `3d`, `7d`, `14d`, `28d` )
- `qty_total` (integer): Total quantity available
- `qty_min` (integer): Minimum quantity per purchase
- `attributes` (array): Array of attribute objects with `id` and `option_id`
- `accounts` (array): Array of account objects (see below)

**Account Object Structure:**
- `login` (string, required): Account login/username
- `password` (string, required): Account password
- `email_login` (string, optional): Email associated with account
- `email_password` (string, optional): Email password
- `description` (string, optional): Additional account details

**Example Request:**
```bash
curl -X POST "https://sellerapi.ggchest.com/v1/offers/accounts" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "game_id": "0198cbed-af1d-70fb-89f9-d514c56395c3",
    "title": "Epic Adventure Game - Premium Accounts",
    "description": "High-level accounts with rare items and premium currency",
    "price": "49.99",
    "delivery_method": "manual",
    "delivery_time": "1d",
    "qty_total": 10,
    "qty_min": 1,
    "attributes": [
      {
        "id": "01985a31-a4c7-79b2-d8f3-5e9a2c6b4d8e",
        "option_id": "01985a31-c6e9-7ad5-f2b7-8a4c1e6d9f2b"
      },
      {
        "id": "01985a31-e8f2-7bc6-a4d9-3f7b5c8e1a4d",
        "option_id": "01985a31-f9c4-7de8-b6a2-4e8d6f9c2b5e"
      }
    ],
    "accounts": [
      {
        "login": "player123",
        "password": "securePass456",
        "email_login": "player123@example.com",
        "email_password": "emailPass789",
        "description": "Level 85, 50k gold, rare mount included"
      },
      {
        "login": "gamer456",
        "password": "strongPass123",
        "email_login": null,
        "email_password": null,
        "description": "Level 92, premium items, guild leader"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{
  "id": "01985a32-1d5f-7ea9-c3b8-6f2a9c5e8d1b"
}
```

#### Update Account Offer

```http
PUT /v1/offers/accounts/{id}
```

Update an existing account offer with new information.

**Path Parameters:**
- `id` (string, required): The account offer identifier

**Request Body:**
```json
{
  "title": "Updated Premium Gaming Account - Level 90+",
  "description": "Enhanced high-level gaming account with additional rare items and achievements",
  "price": "149.99",
  "delivery_time": "1d",
  "qty_total": 8,
  "qty_min": 1,
  "attributes": [
    {
      "id": "01985a37-2e8f-ac3d-f1b7-5c9e3a6f2d8b",
      "option_id": "01985a37-4f1a-ae5f-a3d9-7e2f5c8a4e6d"
    }
  ],
  "accounts": [
    {
      "id": null,
      "login": "updatedgamer2024",
      "password": "NewSecurePass456!",
      "email_login": "updated@email.com",
      "email_password": "NewEmailPass789!",
      "description": "Level 90 character with mythic gear and rare mounts"
    }
  ]
}
```

**Optional Fields:**
- `title` (string): Updated offer title/name
- `description` (string): Updated detailed description
- `price` (string): Updated price in decimal format. Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_time` (string): Expected delivery time (`5m`, `20m`, `1h`, `5h`, `12h`, `1d`, `3d`, `7d`, `14d`, `28d` )
- `qty_total` (integer): Updated total quantity available
- `qty_min` (integer): Updated minimum quantity per purchase
- `attributes` (array): Updated array of attribute objects with `id` and `option_id`
- `accounts` (array): Updated array of account objects (set `id` to `null` for new accounts)

**Example Request:**
```bash
curl -X PUT "https://sellerapi.ggchest.com/v1/offers/accounts/01985a37-6c4e-af8a-b5d1-9a3f7c5e2a8d" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Ultimate Gaming Collection - Level 95+",
    "description": "Premium account collection with exclusive items, rare achievements, and premium currency",
    "price": "199.99",
    "delivery_time": "1d",
    "qty_total": 3,
    "qty_min": 1,
    "attributes": [
      {
        "id": "01985a37-8d6f-b1ac-c7e3-bc2f8d6a4f1c",
        "option_id": "01985a37-af2e-b3ce-d9f5-de4a1c8f6b3e"
      }
    ],
    "accounts": [
      {
        "id": null,
        "login": "ultimategamer95",
        "password": "UltimatePass123!",
        "email_login": "ultimate@gaming.com",
        "email_password": "UltimateEmail456!",
        "description": "Max level character with all achievements and rare collectibles"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{}
```

---

#### Create New Currency Offer

```http
POST /v1/offers/currency
```

Create a new currency offer for in-game currency sales.

**Request Body:**
```json
{
  "game_id": "01985a32-4b8e-7fd2-9c6a-8e3f1a5d7c9b",
  "game_currency_id": "01985a32-6d1f-8ae5-c4b9-2f6e4a8d1c5f",
  "title": "Premium Game Currency - 10K Coins",
  "description": "High-quality in-game currency with instant delivery",
  "price": "66.32",
  "delivery_time": "1d",
  "qty_total": 0,
  "qty_min": 0,
  "attributes": [
    {
      "id": "01985a32-8f4c-81d7-a5e2-9b6f3c8e5a2d",
      "option_id": "01985a32-a1e6-82b9-c7f4-5d9a1e4c7f6b"
    }
  ]
}
```

**Field Descriptions:**
- `game_id` (string, optional): The ID of the game this currency offer is for
- `game_currency_id` (string, optional): The specific currency type within the game
- `title` (string, optional): Currency offer title/name
- `description` (string, optional): Detailed description of the currency offer
- `price` (string, optional): Price in decimal format (e.g., "66.32"). Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_time` (string, optional): Expected delivery timeframe
- `qty_total` (integer, optional): Total quantity of currency available
- `qty_min` (integer, optional): Minimum quantity per purchase
- `attributes` (array, optional): Array of attribute objects with `id` and `option_id`

**Example Request:**
```bash
curl -X POST "https://sellerapi.ggchest.com/v1/offers/currency" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "game_id": "01985a32-c3f8-83e1-b9d6-7a2e5f8c1d4a",
    "game_currency_id": "01985a32-e5a2-84c5-d1f8-3c7f9e2a6d1c",
    "title": "WoW Classic Gold - Server Stormrage",
    "description": "Premium World of Warcraft Classic gold. Fast delivery, safe transfer methods. Perfect for buying epic mounts, raid consumables, and gear upgrades!",
    "price": "12.99",
    "delivery_time": "1d",
    "qty_total": 50000,
    "qty_min": 1000,
    "attributes": [
      {
        "id": "01985a32-f7c4-85a7-e3b9-5f1c8a4e7b2d",
        "option_id": "01985a32-19e6-86d2-f5c1-8e4a7c1f5e8b"
      },
      {
        "id": "01985a33-3c1e-88f6-b4a8-2d7f5a9c3e6d",
        "option_id": "01985a33-4d2f-89a7-c5b9-3e8a6b1d4f7c"
      },
      {
        "id": "01985a33-5e4a-8ab8-d6c2-4f9b7c2e5a8d",
        "option_id": "01985a33-6f5b-8bc9-e7d3-5a1c8d3f6b9e"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{
  "id": "01985a33-2b8f-87e4-a7d3-1f6c9e3a8d5c"
}
```

#### Update Currency Offer

```http
PUT /v1/offers/currency/{id}
```

Update an existing currency offer with new information.

**Path Parameters:**
- `id` (string, required): The currency offer identifier

**Request Body:**
```json
{
  "title": "Updated Premium Game Currency - 15K Coins",
  "description": "Enhanced in-game currency package with bonus rewards and instant delivery",
  "price": "19.99",
  "delivery_time": "1d",
  "qty_total": 200,
  "qty_min": 5,
  "attributes": [
    {
      "id": "01985a38-3f2e-bd7a-c9d5-6e4a2f8c5b1d",
      "option_id": "01985a38-5d4f-bf9c-ebf7-8a6c4f1e7d3f"
    }
  ]
}
```

**Optional Fields:**
- `title` (string): Updated currency offer title/name
- `description` (string): Updated detailed description of the currency offer
- `price` (string): Updated price in decimal format. Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_time` (string): Updated expected delivery timeframe
- `qty_total` (integer): Updated total quantity of currency available
- `qty_min` (integer): Updated minimum quantity per purchase
- `attributes` (array): Updated array of attribute objects with `id` and `option_id`

**Example Request:**
```bash
curl -X PUT "https://sellerapi.ggchest.com/v1/offers/currency/01985a38-7c6e-c1ae-e3b9-2f5a9c8d4f2a" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Ultimate Currency Pack - 25K Gold + Bonus",
    "description": "Premium currency package with 25,000 gold coins plus exclusive bonus items. Perfect for competitive gameplay and premium purchases!",
    "price": "34.99",
    "delivery_time": "1d",
    "qty_total": 150,
    "qty_min": 10,
    "attributes": [
      {
        "id": "01985a38-9e8a-c3d0-f5db-4c7f1a5e9c8b",
        "option_id": "01985a38-bf1c-c5f2-a7dd-6e9c3a7f2e5d"
      },
      {
        "id": "01985a38-d3f5-c7a4-b9ef-8a2e6c4f8a1c",
        "option_id": "01985a38-f5a7-c9c6-dbf1-aa4f8c6e1d3e"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{}
```

---

### Items

#### Create New Items Offer

```http
POST /v1/offers/items
```

Create a new items offer for in-game items sales.

**Request Body:**
```json
{
  "game_id": "01985a34-9c7e-8af1-d3b8-5f9a2c6e4d1a",
  "title": "Rare Weapon Collection - Legendary Items",
  "description": "Premium collection of legendary weapons and armor pieces. Perfect for end-game content!",
  "price": "89.99",
  "delivery_method": "manual",
  "delivery_time": "1d",
  "qty_total": 25,
  "qty_min": 1,
  "attributes": [
    {
      "id": "01985a34-bf2d-8ce3-e5a9-7c1f4a8e2b6d",
      "option_id": "01985a34-d1f4-8de5-f7c2-9e3a6c1d5f8b"
    }
  ]
}
```

**Required Fields:**
- `game_id` (string): The ID of the game this offer is for
- `title` (string): Offer title/name
- `description` (string): Detailed description of the offer
- `price` (string): Price in decimal format (e.g., "89.99"). Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_method` (string): Delivery method - either `"manual"` or `"auto"`
- `delivery_time` (string): Expected delivery timeframe
- `qty_total` (integer): Total quantity available
- `qty_min` (integer): Minimum quantity per purchase
- `attributes` (array): Array of attribute objects with `id` and `option_id`

**Example Request:**
```bash
curl -X POST "https://sellerapi.ggchest.com/v1/offers/items" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "game_id": "01985a34-f3a6-8ef7-a9d4-3c7e9a2f6c1d",
    "title": "Epic Loot Bundle - Rare Items Pack",
    "description": "Exclusive collection of rare items including legendary weapons, premium armor sets, and special consumables. Perfect for competitive players!",
    "price": "24.99",
    "delivery_method": "manual",
    "delivery_time": "1d",
    "qty_total": 100,
    "qty_min": 1,
    "attributes": [
      {
        "id": "01985a35-1e8b-9af2-c5d7-4f2a7e9c3b6d",
        "option_id": "01985a35-3f9c-9bd4-d7e1-6a4c1f5e8d2a"
      },
      {
        "id": "01985a35-5d1e-9ce6-e9f3-8c6f3a7e1c5d",
        "option_id": "01985a35-7e2f-9df8-fb15-2e8d5c9f4a7b"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{
  "id": "01985a35-9b4f-9ef1-a3c7-6e2a9d5f8c1b"
}
```

#### Update Items Offer

```http
PUT /v1/offers/items/{id}
```

Update an existing items offer with new information.

**Path Parameters:**
- `id` (string, required): The items offer identifier

**Request Body:**
```json
{
  "title": "Updated Epic Loot Collection - Mythic Items",
  "description": "Enhanced collection of mythic-tier weapons and armor. Updated with new legendary items and premium consumables!",
  "price": "119.99",
  "delivery_time": "1d",
  "qty_total": 50,
  "qty_min": 2,
  "attributes": [
    {
      "id": "01985a39-4e3f-be8c-d1a7-5f8c3a7e2b9d",
      "option_id": "01985a39-6f5a-c0ae-e3c9-7a1e5c9f4d2b"
    }
  ]
}
```

**Optional Fields:**
- `title` (string): Updated items offer title/name
- `description` (string): Updated detailed description of the items offer
- `price` (string): Updated price in decimal format. Must match pattern: `^[0-9]{1,16}(\.[0-9]{1,2})?$`
- `delivery_time` (string): Updated expected delivery timeframe
- `qty_total` (integer): Updated total quantity available
- `qty_min` (integer): Updated minimum quantity per purchase
- `attributes` (array): Updated array of attribute objects with `id` and `option_id`

**Example Request:**
```bash
curl -X PUT "https://sellerapi.ggchest.com/v1/offers/items/01985a39-8d7c-c2b0-f5eb-3c6f9a2e8d5a" \
  -H "X-API-KEY: your_api_key_here" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Ultimate Items Package - Mythic Tier Collection",
    "description": "Premium collection of mythic-tier items including legendary weapons, epic armor sets, rare mounts, and exclusive consumables. Perfect for end-game content and competitive play!",
    "price": "149.99",
    "delivery_time": "1d",
    "qty_total": 75,
    "qty_min": 1,
    "attributes": [
      {
        "id": "01985a39-ae1d-c4d2-a7fd-5e9c4a8f3c6e",
        "option_id": "01985a39-cf2e-c6f4-b9ae-7f1d6ca5e8fb"
      },
      {
        "id": "01985a39-e13f-c8a6-dbc0-9a3f8e2a7d1c",
        "option_id": "01985a39-f35a-cab8-fde2-bc5a1f4e9a3d"
      }
    ]
  }'
```

**Response (200 - Success):**
```json
{}
```

---

### Offer Management

#### Activate and Deactivate Offer

```http
PUT /v1/offers/{id}/toggle
```

This API endpoint enables or disables your offer. You don’t need to send any data, just send an empty request. If the offer was active, the request will deactivate it, and vice versa, if the offer was paused, the request will activate it. If you receive a 200 response, it means everything is fine. The response body will be empty.

**Path Parameters:**
- `id` (string, required): The offer identifier

**Request Body:**
```json
{}
```

**Response:**
```json
{}
```

#### Update Offer Price

```http
PUT /v1/offers/{id}/price
```

This API endpoint updates the price of your offer. All prices are specified in US dollars. We only accept 2 decimal places (see example).


**Path Parameters:**
- `id` (string, required): The offer identifier

**Request Body:**
```json
{
  "price": "66.32"
}
```

**Response:**
```json
{}
```

---

### Image Management

#### Attach icon to Offer

```http
POST /v1/offers/{id}/icon
```

Attach an icon to the seller's offer. (Allowed png, jpg, jpeg)

**Path Parameters:**
- `id` (string, required): The offer identifier

**Request Body:** (multipart/form-data)
- `icon` (string, required): The icon file to upload

**Example Request:**
```bash
curl -X POST "https://sellerapi.ggchest.com/v1/offers/0198cce2-6b69-7178-945a-5a45a6139ab4/icon" \
  -H "X-API-KEY: your_api_key_here" \
  -F "icon=@/path/to/icon.png"
```

**Response (200 - Success):**
```json
{
  "id": "01985a33-8b7d-8de2-a9f5-7c3e1f5b8d2a",
  "path": "https://storage.googleapis.com/ggchest-public/offers/icons/01985a33-8b7d-8de2-a9f5-7c3e1f5b8d2a.png"
}
```


#### Delete Offer Icon

```http
DELETE /v1/offers/{offerId}/icon/{iconId}
```

Remove an icon from an offer.

**Path Parameters:**
- `offerId` (string, required): The offer identifier
- `iconId` (string, required): The icon identifier

**Response:**
```json
{ }
```

## 📋 API Errors

The GGChest Seller API uses standard HTTP status codes to indicate the success or failure of requests. All error responses include a message explaining what went wrong.

### Error Response Format

#### 400 - Bad Request

Returned when the request contains invalid data or parameters.

```json
{
    "message": "Invalid input data",
    "errors": [
        {
            "property": "[filters][offer_type]",
            "message": "The value you selected is not a valid choice."
        }
    ]
}
```

#### 403 - Forbidden
Returned when authentication is missing or invalid.

```json
{
  "message": "Full authentication is required to access this resource."
}
```

#### 404 - Not found
Entity not found

```json
{
  "message": "Not found."
}
```

#### 422 - Unprocessable Entity
Returned when validation fails on submitted data.

```json
{
    "message": "Invalid input data",
    "errors": [
        {
            "property": "[filters][offer_type]",
            "message": "The value you selected is not a valid choice."
        }
    ]
}
```

Or simplified format:

```json
{
    "message": "Invalid input data"
}
```

### Common Error Scenarios

- **Missing API Key**: Returns 403 with authentication required message
- **Invalid Parameters**: Returns 400 with specific parameter errors
- **Validation Failures**: Returns 422 with detailed validation errors
- **Resource Not Found**: Returns 404 (not shown in examples above)
- **Rate Limiting**: Returns 429 when limits are exceeded

## ⚠️ Error Handling

### Common Error Codes

| Code | Status | Description |
|------|--------|-------------|
| `AUTHENTICATION_REQUIRED` | 401 | Missing or invalid authentication token |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `VALIDATION_ERROR` | 400 | Request validation failed |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests |
| `INTERNAL_ERROR` | 500 | Server error |

## 🚦 Rate Limiting

- **Rate Limit:** 1000 requests per hour per API key
- **Burst Limit:** 50 requests per minute

### Interactive Documentation

- **OpenAPI JSON:** [openapi/openapi.json](openapi/openapi.json)

## 📞 Support

### Direct Support
- **Email:** it@ggchest.com
- **Response Time:** 24-48 hours

---

## 🔄 Changelog

### v1.0 (2024-01-21)
- Initial Seller API release
- API Key authentication with X-API-KEY header
- Offers management endpoints
  - Accounts
  - Items
  - Currency
- Icon upload and management
- Offer price updates and status toggling
- OpenAPI 3.1.1 specification

---

**Made with ❤️ by the GGChest IT Team**
