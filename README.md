# OpenMenuStandard (OMS)
### Version 1.0 - May 2025

## Abstract

This document defines the OpenMenuStandard (OMS), an open interchange format for restaurant menu items, nutrition information, customizations, and ordering. The standard enables interoperable representation of food ordering information across applications, platforms, and services, while providing a foundation for nutritional transparency and dietary awareness.

## Table of Contents

1. [Introduction](#introduction)
   1. [Purpose](#purpose)
   2. [Scope](#scope)
   3. [Normative References](#normative-references)
   4. [Terminology](#terminology)

2. [Core Specification](#core-specification)
   1. [File Format](#file-format)
   2. [Character Encoding](#character-encoding)
   3. [MIME Types](#mime-types)
   4. [URL Scheme](#url-scheme)

3. [Data Model](#data-model)
   1. [Document Structure](#document-structure)
   2. [Metadata Object](#metadata-object)
   3. [Vendor Object](#vendor-object)
   4. [Item Object](#item-object)
   5. [Nutrition Object](#nutrition-object)
   6. [Customization Object](#customization-object)
   7. [Order Object](#order-object)
   8. [Extensions Object](#extensions-object)

4. [Implementation Guidelines](#implementation-guidelines)
   1. [Restaurant Implementation](#restaurant-implementation)
   2. [Application Implementation](#application-implementation)
   3. [Tap-to-Order Implementation](#tap-to-order-implementation)
   4. [NFC Data Structure](#nfc-data-structure)

5. [Use Cases](#use-cases)
   1. [Individual Ordering](#individual-ordering)
   2. [Group Ordering](#group-ordering)
   3. [Tap-to-Order Scenarios](#tap-to-order-scenarios)
   4. [Dietary Management](#dietary-management)

6. [Security and Privacy Considerations](#security-and-privacy-considerations)
   1. [Personal Data Handling](#personal-data-handling)
   2. [Transmission Security](#transmission-security)
   3. [Storage Guidelines](#storage-guidelines)

7. [Conformance Requirements](#conformance-requirements)
   1. [Minimum Implementation](#minimum-implementation)
   2. [Extended Implementation](#extended-implementation)
   3. [Validation](#validation)

8. [Appendices](#appendices)
   1. [Example Implementations](#example-implementations)
   2. [Reference Validation Tools](#reference-validation-tools)
   3. [Recommended Practices](#recommended-practices)

## 1. Introduction

### 1.1 Purpose

The OpenMenuStandard (OMS) has been developed to address the fragmented landscape of digital food ordering systems. As restaurants and food service providers increasingly transition to digital platforms, there is a growing need for a standardized way to represent menu items, nutritional information, and ordering details. This standard aims to enable interoperability between different platforms and applications while empowering consumers with transparent information about their food choices.

OMS provides a comprehensive framework for representing food items, their nutritional content, customization options, and ordering details in a consistent, machine-readable format. By adopting this standard, restaurants, food service providers, and application developers can create more integrated, user-centric experiences that promote transparency and choice.

### 1.2 Scope

The OpenMenuStandard encompasses:

- Digital representation of menu items and their attributes
- Comprehensive nutritional information and ingredient details
- Standardized description of customization options
- Order representation and transmission
- Integration methods including file sharing, URL schemes, and near-field communication
- Privacy and security considerations for implementations

This standard is designed to be applicable across various food service contexts, including but not limited to restaurants, cafes, fast food establishments, pizza delivery services, coffee shops, food trucks, catering services, and institutional food services.

### 1.3 Normative References

The following referenced documents are indispensable for the application of this document:

- ECMA-404: The JSON Data Interchange Syntax
- RFC 8259: The JavaScript Object Notation (JSON) Data Interchange Format
- RFC 3339: Date and Time on the Internet: Timestamps
- ISO 4217: Currency codes
- ISO 3166-1: Country codes
- ISO 639-1: Language codes

### 1.4 Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

For the purposes of this standard, the following terms and definitions apply:

**Item**: A discrete food or beverage product available for purchase.

**Customization**: A modification that can be applied to an item, affecting its preparation, ingredients, or presentation.

**Nutrition**: Quantitative information about the nutritional content of an item.

**Vendor**: An entity that provides food items for sale or distribution.

**Order**: A collection of items with specified customizations requested by a customer.

**NFC**: Near Field Communication, a set of protocols enabling electronic devices to establish communication by bringing them within proximity.

## 2. Core Specification

### 2.1 File Format

OpenMenuStandard files SHALL be represented in JSON (JavaScript Object Notation) format as defined in ECMA-404 and RFC 8259. The JSON structure MUST conform to the schemas defined in this specification.

A conforming OMS document SHALL be a single JSON object containing the required top-level properties defined in the Document Structure section.

### 2.2 Character Encoding

All OpenMenuStandard files MUST use UTF-8 encoding as defined in RFC 3629. Implementations MUST properly handle all valid Unicode characters, including those for international menu items, ingredients, and descriptions.

### 2.3 MIME Types

When transmitted over HTTP or other protocols that use MIME types, OpenMenuStandard documents SHOULD be identified with the following MIME type:

`application/vnd.openmenu+json`

For file storage, the recommended file extension is `.omenu`.

### 2.4 URL Scheme

The OpenMenuStandard defines a URL scheme for enabling direct integration between applications and services. The URL scheme SHALL be structured as follows:

`omenu://<action>/<parameters>`

Where:
- `<action>` is one of: "view", "order", "customize", or "share"
- `<parameters>` are URL-encoded key-value pairs specific to each action

Example:
`omenu://order?vendor=subway-usa&location=store-1234&item=italian-bmt`

## 3. Data Model

### 3.1 Document Structure

An OpenMenuStandard document MUST include the following top-level properties:

```json
{
  "oms_version": "1.0",
  "metadata": { /* Metadata object */ },
  "vendor": { /* Vendor object */ },
  "items": [ /* Array of Item objects */ ]
}
```

The document MAY also include these OPTIONAL top-level properties:

```json
{
  "order": { /* Order object */ },
  "extensions": { /* Extensions object */ }
}
```

### 3.2 Metadata Object

The Metadata object contains information about the OMS document itself. It MUST include the following properties:

```json
{
  "created": "2025-05-14T10:30:00Z",
  "source": "application-identifier",
  "locale": "en-US"
}
```

Where:
- `created` is an RFC 3339 timestamp indicating when the document was created
- `source` identifies the application or system that generated the document
- `locale` is an RFC 5646 language tag indicating the primary language of text content

### 3.3 Vendor Object

The Vendor object contains information about the food service provider. It MUST include the following properties:

```json
{
  "id": "unique-vendor-id",
  "name": "Vendor Name",
  "type": "restaurant"
}
```

The Vendor object MAY include these OPTIONAL properties:

```json
{
  "location_id": "specific-location-id",
  "location_name": "Specific Location Name",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "region": "State/Province",
    "postal_code": "12345",
    "country": "USA"
  },
  "contact": {
    "phone": "+1-555-123-4567",
    "website": "https://example.com"
  },
  "hours": [
    {
      "day": "monday",
      "ranges": [{"open": "08:00", "close": "22:00"}]
    },
    // Other days
  ],
  "cuisine": ["italian", "sandwiches"],
  "services": ["dine-in", "takeout", "delivery"]
}
```

### 3.4 Item Object

The Item object represents a food or beverage product. It MUST include the following properties:

```json
{
  "id": "item-unique-id",
  "name": "Menu Item Name",
  "category": "sandwich"
}
```

The Item object MAY include these OPTIONAL properties:

```json
{
  "vendor_id": "vendor-specific-id",
  "description": "Detailed description of the item",
  "subcategory": "breakfast",
  "image_url": "https://example.com/images/item.jpg",
  "base_price": 7.99,
  "currency": "USD",
  "nutrition": { /* Nutrition object */ },
  "customizations": [ /* Array of Customization objects */ ],
  "selected_customizations": [ /* Array of selected customization objects */ ],
  "quantity": 1,
  "item_note": "Please toast it extra long",
  "calculated": { /* Calculated values based on customizations */ },
  "components": [ /* For combo meals, an array of component Item objects */ ],
  "availability": {
    "start_date": "2025-05-01",
    "end_date": "2025-06-30",
    "times_of_day": ["breakfast", "lunch"],
    "days_of_week": ["monday", "tuesday", "wednesday", "thursday", "friday"]
  },
  "popularity": {
    "rank": 5,
    "tags": ["best-seller", "staff-pick"]
  }
}
```

### 3.5 Nutrition Object

The Nutrition object provides detailed nutritional information about an item. The structure allows for both simple representations (for basic applications) and comprehensive details (for nutrition-focused applications).

```json
{
  "serving_size": {
    "value": 240,
    "unit": "g"
  },
  "calories": 410,
  "protein": {
    "value": 22,
    "unit": "g"
  },
  "fat": {
    "value": 16,
    "unit": "g",
    "details": {
      "saturated": {
        "value": 6,
        "unit": "g"
      },
      "trans": {
        "value": 0,
        "unit": "g"
      },
      "polyunsaturated": {
        "value": 2,
        "unit": "g"
      },
      "monounsaturated": {
        "value": 8,
        "unit": "g"
      }
    }
  },
  "carbohydrates": {
    "value": 47,
    "unit": "g",
    "details": {
      "fiber": {
        "value": 3,
        "unit": "g"
      },
      "sugars": {
        "value": 6,
        "unit": "g",
        "details": {
          "added_sugars": {
            "value": 2,
            "unit": "g"
          }
        }
      },
      "starch": {
        "value": 38,
        "unit": "g"
      }
    }
  },
  "sodium": {
    "value": 980,
    "unit": "mg"
  },
  "cholesterol": {
    "value": 35,
    "unit": "mg"
  },
  "vitamins": [
    {
      "name": "Vitamin A",
      "value": 120,
      "unit": "mcg",
      "daily_value_percent": 13
    },
    {
      "name": "Vitamin C",
      "value": 15,
      "unit": "mg",
      "daily_value_percent": 17
    }
  ],
  "minerals": [
    {
      "name": "Calcium",
      "value": 200,
      "unit": "mg",
      "daily_value_percent": 15
    },
    {
      "name": "Iron",
      "value": 2.5,
      "unit": "mg",
      "daily_value_percent": 14
    },
    {
      "name": "Potassium",
      "value": 450,
      "unit": "mg",
      "daily_value_percent": 10
    }
  ],
  "allergens": ["wheat", "dairy", "soy"],
  "dietary_flags": ["contains_gluten", "contains_dairy"],
  "health_claims": ["good_source_of_protein", "contains_whole_grains"],
  "ingredients": [
    {
      "name": "Bread",
      "ingredients": ["flour", "water", "yeast", "salt"]
    },
    {
      "name": "Salami",
      "ingredients": ["pork", "salt", "spices"]
    }
  ],
  "nutrition_standards": {
    "us_fda": {
      "serving_size_description": "1 sandwich (240g)",
      "daily_value_year": 2020
    },
    "eu_regulation": {
      "reference_intake_description": "Per 100g"
    }
  }
}
```

### 3.6 Customization Object

The Customization object defines ways in which an item can be modified. The standard supports several types of customizations to accommodate various food service models.

```json
{
  "id": "cust-1",
  "name": "Bread Type",
  "type": "single_select",
  "required": true,
  "default": "italian-herbs",
  "options": [
    {
      "id": "italian-herbs",
      "name": "Italian Herbs & Cheese",
      "price_adjustment": 0.00,
      "nutrition_adjustments": {
        "calories": 0,
        "fat": {"value": 0, "unit": "g"},
        "carbohydrates": {"value": 0, "unit": "g"}
      },
      "allergens": ["wheat", "dairy"],
      "dietary_flags": ["contains_gluten"]
    },
    {
      "id": "wheat",
      "name": "Wheat Bread",
      "price_adjustment": 0.00,
      "nutrition_adjustments": {
        "calories": -20,
        "fat": {"value": -1, "unit": "g"},
        "carbohydrates": {"value": 2, "unit": "g"}
      },
      "allergens": ["wheat"],
      "dietary_flags": ["contains_gluten", "high_fiber"]
    },
    {
      "id": "gluten-free",
      "name": "Gluten-Free Bread",
      "price_adjustment": 1.50,
      "nutrition_adjustments": {
        "calories": -10,
        "carbohydrates": {"value": -2, "unit": "g"}
      },
      "allergens": [],
      "dietary_flags": ["gluten_free"]
    }
  ]
}
```

The standard supports the following customization types:

- `single_select`: Choose one option from a list
- `multi_select`: Choose multiple options from a list
- `quantity`: Specify a numeric quantity of an ingredient or add-on
- `boolean`: Simple yes/no option
- `text`: Free-form text instructions
- `range`: Select a value within a numeric range (e.g., spice level)

For `multi_select` customizations, additional properties may be specified:

```json
{
  "min_selections": 0,
  "max_selections": 10
}
```

For `quantity` customizations, additional properties may be specified:

```json
{
  "min": 0,
  "max": 3,
  "step": 1,
  "unit_price_adjustment": 0.50,
  "unit_nutrition_adjustments": {
    "calories": 50,
    "protein": {"value": 3, "unit": "g"},
    "fat": {"value": 4, "unit": "g"}
  }
}
```

### 3.7 Order Object

The Order object represents a collection of items being ordered. It MAY include the following properties:

```json
{
  "id": "unique-order-id",
  "status": "draft",
  "created": "2025-05-14T10:30:00Z",
  "pickup_time": "2025-05-14T12:00:00Z",
  "delivery_time": "2025-05-14T12:15:00Z",
  "type": "pickup",
  "customer_notes": "Please include extra napkins",
  "payment": {
    "status": "unpaid",
    "method": "credit-card",
    "subtotal": 15.97,
    "tax": 1.28,
    "tip": 3.00,
    "total": 20.25,
    "currency": "USD"
  },
  "customer": {
    "id": "customer-id",
    "name": "Customer Name",
    "phone": "+1-555-987-6543",
    "email": "customer@example.com"
  },
  "delivery": {
    "address": {
      "street": "456 Oak Ave",
      "unit": "Apt 3B",
      "city": "Anytown",
      "region": "State",
      "postal_code": "12345",
      "country": "USA"
    },
    "instructions": "Ring buzzer 3B. Leave at door if no answer."
  }
}
```

### 3.8 Extensions Object

The Extensions object allows for vendor-specific or future extensions to the standard without requiring changes to the core specification. Extensions MUST be namespaced to avoid conflicts.

```json
{
  "extensions": {
    "com.example.restaurant": {
      "loyalty": {
        "points_earned": 25,
        "member_discount": 2.50
      }
    },
    "org.dietary.app": {
      "meal_plan": {
        "compatibility": "high",
        "recommended_sides": ["side-salad", "fruit-cup"]
      }
    }
  }
}
```

## 4. Implementation Guidelines

### 4.1 Restaurant Implementation

Restaurants and food service providers implementing the OpenMenuStandard should approach adoption through a structured process that ensures comprehensive integration with existing systems.

Menu digitization is the foundational step in implementing OMS. This process involves converting existing menu items into the standardized format, including detailed nutritional information and customization options. The level of detail should be proportional to the complexity of the menu offerings and the sophistication of the implementation.

The nutritional information provided should adhere to local regulatory requirements while leveraging the comprehensive structure defined in the standard. In regions with stringent nutritional reporting guidelines, such as the United States FDA requirements or European Union regulations, the nutrition object should include all mandated values.

Customization options should reflect the actual preparation capabilities of the restaurant. Each customization should be categorized according to the appropriate type (single_select, multi_select, quantity, etc.) and include accurate price adjustments and nutritional impacts.

Integration points with existing systems are critical for successful implementation. The standard supports integration with:

- Point-of-Sale (POS) systems through API connections or exported files
- Online ordering platforms via direct implementation of the data model
- Mobile applications through the OMS URL scheme and file format
- Kiosks and self-order terminals with NFC capabilities

For tap-to-order implementation, restaurants should develop a strategy that aligns with their service model. This may include:

- Programming NFC tags with OMS URL schemes, placing them on tables, menus, or promotional materials
- Configuring payment terminals to accept OMS data in addition to payment information
- Implementing backend services that can process orders received in OMS format

Restaurants should consider phased implementation, beginning with basic menu digitization and progressing to full tap-to-order capabilities as systems and customer adoption allow.

### 4.2 Application Implementation

Application developers implementing the OpenMenuStandard should focus on creating a seamless user experience while adhering to the technical requirements of the standard.

File handling is essential for proper implementation. Applications should:

- Register to handle the `.omenu` file extension on applicable platforms
- Implement the `omenu://` URL scheme for deep linking
- Support both importing and exporting of OMS data
- Validate incoming OMS data against the schema defined in this specification

Feature implementation should focus on enhancing the user experience through the capabilities enabled by the standard:

- Nutrition calculation based on selected customizations, showing real-time updates as users modify their orders
- Order history storage in a standardized format that can be shared across applications
- Order sharing capabilities that leverage the standard to enable social and group ordering experiences
- Group order consolidation, allowing multiple individual orders to be combined into a single submission

Integration methods should support the various ways users may interact with restaurants and other food service providers:

- Direct API connections to restaurant ordering systems
- QR code generation and scanning for sharing and ordering
- NFC read/write capabilities for tap-to-order experiences
- Deep linking with restaurant-specific applications

Attention to security and privacy is paramount, particularly when handling user preferences, dietary restrictions, and order history. Applications should implement appropriate safeguards and clearly communicate privacy policies to users.

### 4.3 Tap-to-Order Implementation

Tap-to-order functionality represents an advanced implementation of the OpenMenuStandard that leverages Near Field Communication (NFC) technology to streamline the ordering process. This capability allows users to initiate or complete orders by tapping their NFC-enabled device on a compatible tag or terminal.

The core functionality of tap-to-order relies on encoding OMS-compatible data in NFC tags or transmitting it between NFC-enabled devices. This data can take several forms:

- A complete OMS document containing all necessary order information
- An OMS URL scheme that references a specific item or order
- A hybrid approach that includes basic information in the NFC tag with a link to retrieve full details

For in-restaurant ordering scenarios, the implementation typically involves placing NFC tags on tables, menu items, or dedicated ordering stations. When a user taps their device on the tag, the device reads the encoded information and launches an appropriate application that can interpret the OMS data. The user can then customize their order on their personal device before submitting it directly to the restaurant's ordering system.

Rapid reordering can be implemented by allowing users to save favorite orders as NFC tags or by associating their preferences with their payment methods. When the user taps to reorder, the system can retrieve their preferred items and customizations, allowing for one-tap ordering with minimal interaction.

Smart vending and self-service kiosks can leverage tap-to-order by allowing users to browse options on their personal device after tapping, then configuring customizations before the machine prepares the item. This approach enhances the traditional vending experience by offering more customization options while maintaining the convenience of self-service.

### 4.4 NFC Data Structure

The NFC Data Structure defines how OpenMenuStandard information is encoded in NFC tags and transmitted between NFC-enabled devices. Two primary formats are supported:

1. URL Format:
   The URL format encodes essential information in an `omenu://` URL that can be launched by the device's operating system. This format is compact and compatible with most NFC tag capacities.

   ```
   omenu://order?v=vendor_id&l=location_id&i=item_id&c=preset_customization_id
   ```

   Where:
   - `v` is the vendor identifier
   - `l` is the specific location identifier
   - `i` is the item identifier
   - `c` is an optional preset customization identifier

2. JSON Format:
   For more complex scenarios where more data needs to be transmitted, a compact JSON representation can be used. This format is particularly useful for offline ordering or when the tag needs to contain complete nutritional information.

   ```json
   {"v":"vendor_id","l":"location_id","i":"item_id","o":{"c":[{"id":"bread","s":"wheat"},{"id":"cheese","s":"provolone"}]}}
   ```

   Where:
   - `v` is the vendor identifier
   - `l` is the specific location identifier
   - `i` is the item identifier
   - `o` is an object containing order details
   - `c` is an array of customization selections

The physical implementation of NFC tags should take into account the environmental factors of the restaurant setting. Tags should be:

- Water and heat resistant to withstand food service environments
- Securely attached to prevent tampering or removal
- Clearly marked to indicate their purpose to users
- Positioned for convenient access by customers

For table tags, the recommended approach is to embed NFC tags in table markers, menu cards, or dedicated ordering devices. Each tag should be programmed with location-specific information that identifies the table and directs to the restaurant's ordering system.

## 5. Use Cases

### 5.1 Individual Ordering

The OpenMenuStandard enhances individual ordering experiences by providing consistent access to detailed information and personalization options across different food service providers.

Dietary management is significantly improved through the standardized nutrition and allergen information. Users with specific dietary requirements can filter menu options based on their restrictions, whether those are allergen-related, religious, ethical, or preference-based. Applications can analyze the nutritional content of items and provide guidance based on the user's dietary goals or restrictions.

The standard supports personalized recommendations by allowing applications to learn from a user's preferences and order history. By analyzing patterns in customization choices and item selection, applications can suggest items that align with the user's taste preferences and dietary needs.

Order history preservation in a standardized format allows users to reference past orders regardless of which application was used to place them. This consistency enables users to reorder favorite items with the same customizations without needing to remember specific details or navigate different interfaces.

### 5.2 Group Ordering

Group ordering scenarios benefit significantly from a standardized format that allows multiple individuals to contribute to a unified order. The OpenMenuStandard facilitates several common group ordering patterns.

Family meals often involve collecting preferences from multiple family members who may have different dietary needs or preferences. The standard allows each family member to create their customization using a compatible application, then share it with the ordering person via text messaging, email, or direct application sharing. The receiving application can consolidate these selections into a single order, maintaining all customizations and calculating the total cost.

Office lunch coordination is streamlined by allowing a designated coordinator to create a group order to which individuals can contribute their selections. Each person can browse the menu, select and customize items according to their preferences, and submit their choices to the group order. The coordinator can review the combined order, make adjustments if necessary, and submit it to the restaurant with all individual customizations intact.

Party planning for larger events becomes more manageable when organizers can collect standardized orders from attendees in advance. The structured format allows for efficient aggregation of requests, clear communication with catering services, and accurate fulfillment of individual preferences even when scaling to dozens or hundreds of participants.

### 5.3 Tap-to-Order Scenarios

Tap-to-order functionality enabled by the OpenMenuStandard creates new interaction models for food ordering that emphasize convenience and personalization.

In-restaurant ordering is enhanced by allowing customers to tap NFC tags at their table or on menu items to initiate the ordering process. After tapping, customers can view detailed information about the item, including nutritional content and available customizations, on their personal device. They can then customize their order according to their preferences and submit it directly to the kitchen, reducing wait times and potential errors in order taking.

Rapid reordering becomes possible when customers can save their preferred orders as NFC tags or associate them with their payment methods. By simply tapping their device or card at a compatible terminal, customers can recall their favorite orders with all customizations intact, streamlining the ordering process for repeat visits.

Smart vending represents an innovative application of the standard, where traditional vending machines are enhanced with NFC capabilities. Customers can tap their device on the machine to view detailed product information and nutritional content before making a selection. For prepared items, they can specify customizations on their device, which the machine then follows during preparation.

### 5.4 Dietary Management

The comprehensive nutritional and ingredient information provided by the OpenMenuStandard enables sophisticated dietary management across different food service providers.

Allergen tracking is enhanced by the structured representation of allergens both at the item level and within customization options. Users with food allergies can quickly identify safe options and make informed customization choices to avoid allergens. Applications can provide alerts when selected items contain allergens relevant to the user's profile.

Nutritional goal tracking is supported by the detailed nutritional information included in the standard. Users following specific dietary plans can monitor their intake of calories, macronutrients, and micronutrients across different meals and restaurants. Applications can aggregate this information over time to provide insights into dietary patterns and progress toward nutritional goals.

Religious and ethical dietary requirements are accommodated through ingredient transparency and dietary flags. Users following kosher, halal, vegetarian, vegan, or other ethical dietary practices can identify compliant options based on the detailed ingredient information provided in the standard.

Medical dietary restrictions, such as those related to diabetes, kidney disease, hypertension, or other health conditions, can be managed more effectively with access to detailed nutritional information. Applications can highlight items that align with medical guidelines or suggest modifications to make items more suitable for specific health conditions.

## 6. Security and Privacy Considerations

### 6.1 Personal Data Handling

The OpenMenuStandard includes provisions for handling personal data in ways that respect user privacy and comply with relevant regulations. The standard itself does not mandate the collection of personal information, but it provides structures that may contain such data, particularly in the Order and Customer objects.

Implementations should adhere to the following principles regarding personal data:

- Collection of personal data should be limited to what is necessary for the intended function
- Users should be informed about what personal data is collected and how it will be used
- Personal data should be stored securely and retained only as long as necessary
- Users should have the ability to access, correct, and delete their personal data
- Transmission of personal data should be secured using appropriate encryption methods

In regions with specific privacy regulations, such as the European Union's General Data Protection Regulation (GDPR) or the California Consumer Privacy Act (CCPA), implementations must ensure compliance with all applicable requirements.

### 6.2 Transmission Security

The transmission of OpenMenuStandard data between systems should be secured to protect both personal information and order details. Implementations should adopt the following practices:

- Use HTTPS for all web-based transmission of OMS data
- Implement TLS 1.2 or higher for all API connections
- Employ secure elements for NFC transactions where applicable
- Use end-to-end encryption for direct device-to-device transfers
- Validate the authenticity of received data before processing

For NFC implementations, additional security considerations apply:

- NFC tags in public spaces should contain only non-sensitive information
- Sensitive data should only be transmitted during active, authenticated sessions
- Users should be able to verify the destination of tap-to-order actions before submission
- Physical security measures should protect NFC tags from tampering or replacement

### 6.3 Storage Guidelines

The storage of OpenMenuStandard data, particularly order history and user preferences, requires appropriate security measures to protect user privacy and prevent unauthorized access.

Local storage on user devices should:
- Encrypt sensitive data at rest
- Implement secure access controls
- Provide clear mechanisms for users to delete stored data
- Separate personal identifiable information from order details where feasible

Server-side storage should:
- Comply with relevant data protection regulations
- Implement robust access controls and authentication
- Regularly audit security measures and update as needed
- Maintain secure backups and disaster recovery procedures
- Anonymize or pseudonymize personal data when used for analytics or improvement purposes

## 7. Conformance Requirements

### 7.1 Minimum Implementation

To claim conformance with the OpenMenuStandard, an implementation must satisfy the following minimum requirements:

1. Document Structure:
   - Support the required top-level properties (oms_version, metadata, vendor, items)
   - Properly handle the JSON format and UTF-8 encoding

2. Menu Representation:
   - Accurately represent menu items with their basic properties
   - Support at least one customization type (single_select recommended)
   - Include basic nutritional information (at minimum: calories, serving size)

3. File Format:
   - Generate and process files with the `.omenu` extension
   - Support the correct MIME type: `application/vnd.openmenu+json`

4. URL Scheme:
   - Recognize and handle the `omenu://` URL scheme for at least the "view" action

### 7.2 Extended Implementation

An extended implementation should support these additional features:

1. Complete Nutrition:
   - Support the full nutrition object structure
   - Include detailed macronutrient and micronutrient information
   - Represent allergens and dietary flags

2. Advanced Customizations:
   - Support multiple customization types
   - Include price and nutrition adjustments for customizations
   - Handle interdependent customizations

3. Order Management:
   - Support the complete order object structure
   - Handle multi-item orders with individual customizations
   - Process order status changes

4. Tap-to-Order:
   - Implement NFC tag reading and writing
   - Support both URL and JSON formats for NFC data
   - Handle secure transaction processing

### 7.3 Validation

Valid OpenMenuStandard documents must conform to the JSON schema defined in this specification. Implementations should validate documents before processing to ensure compatibility and prevent errors.

The OpenMenuStandard community maintains reference validation tools that can be used to verify conformance:

- JSON Schema files for validation
- Command-line validation tools
- Web-based validation services
- Integration libraries for common programming languages

Implementations should regularly test their OMS document generation and processing against these validation tools to ensure ongoing compliance with the standard.

## 8. Appendices

### 8.1 Example Implementations

#### Fast Food Restaurant Example

```json
{
  "oms_version": "1.0",
  "metadata": {
    "created": "2025-05-14T10:30:00Z",
    "source": "fastfood-pos-system",
    "locale": "en-US"
  },
  "vendor": {
    "id": "burger-place",
    "name": "Burger Place",
    "type": "fast-food",
    "location_id": "store-1234",
    "location_name": "Main Street"
  },
  "items": [
    {
      "id": "value-meal-1",
      "name": "Quarter Pounder Meal",
      "category": "combo_meal",
      "base_price": 8.99,
      "currency": "USD",
      "components": [
        {
          "id": "quarter-pounder",
          "name": "Quarter Pounder",
          "category": "burger",
          "customizations": [
            {
              "id": "remove-items",
              "name": "Remove Ingredients",
              "type": "multi_select",
              "required": false,
              "default": [],
              "options": [
                {"id": "no-onions", "name": "No Onions"},
                {"id": "no-pickles", "name": "No Pickles"},
                {"id": "no-ketchup", "name": "No Ketchup"}
              ]
            }
          ]
        },
        {
          "id": "side-item",
          "name": "Side Item",
          "type": "single_select",
          "required": true,
          "default": "fries-medium",
          "options": [
            {"id": "fries-medium", "name": "Medium Fries"},
            {"id": "fries-large", "name": "Large Fries", "price_adjustment": 0.50},
            {"id": "salad-side", "name": "Side Salad", "price_adjustment": 1.00}
          ]
        },
        {
          "id": "drink",
          "name": "Drink",
          "type": "single_select",
          "required": true,
          "default": "cola-medium",
          "options": [
            {"id": "cola-medium", "name": "Medium Cola"},
            {"id": "diet-cola-medium", "name": "Medium Diet Cola"},
            {"id": "lemonade-medium", "name": "Medium Lemonade"}
          ]
        }
      ],
      "nutrition": {
        "serving_size": {"value": 1, "unit": "meal"},
        "calories": 1050,
        "protein": {"value": 25, "unit": "g"},
        "fat": {"value": 45, "unit": "g"},
        "carbohydrates": {"value": 120, "unit": "g"}
      }
    }
  ]
}
```

#### Coffee Shop Example

```json
{
  "oms_version": "1.0",
  "metadata": {
    "created": "2025-05-14T10:30:00Z",
    "source": "coffee-shop-app",
    "locale": "en-US"
  },
  "vendor": {
    "id": "java-junction",
    "name": "Java Junction",
    "type": "coffee-shop",
    "location_id": "downtown-3"
  },
  "items": [
    {
      "id": "latte",
      "name": "Latte",
      "category": "coffee",
      "base_price": 4.50,
      "currency": "USD",
      "customizations": [
        {
          "id": "size",
          "name": "Size",
          "type": "single_select",
          "required": true,
          "default": "medium",
          "options": [
            {"id": "small", "name": "Small (12oz)", "price_adjustment": -0.50},
            {"id": "medium", "name": "Medium (16oz)", "price_adjustment": 0.00},
            {"id": "large", "name": "Large (20oz)", "price_adjustment": 0.50}
          ]
        },
        {
          "id": "milk",
          "name": "Milk Type",
          "type": "single_select",
          "required": true,
          "default": "whole",
          "options": [
            {"id": "whole", "name": "Whole Milk"},
            {"id": "skim", "name": "Skim Milk"},
            {"id": "almond", "name": "Almond Milk", "price_adjustment": 0.75},
            {"id": "oat", "name": "Oat Milk", "price_adjustment": 0.75},
            {"id": "soy", "name": "Soy Milk", "price_adjustment": 0.75}
          ]
        },
        {
          "id": "shots",
          "name": "Espresso Shots",
          "type": "quantity",
          "required": true,
          "default": 2,
          "min": 1,
          "max": 6,
          "unit_price_adjustment": 0.75
        },
        {
          "id": "syrups",
          "name": "Flavor Syrups",
          "type": "multi_select",
          "required": false,
          "default": [],
          "options": [
            {"id": "vanilla", "name": "Vanilla", "price_adjustment": 0.50},
            {"id": "caramel", "name": "Caramel", "price_adjustment": 0.50},
            {"id": "hazelnut", "name": "Hazelnut", "price_adjustment": 0.50},
            {"id": "mocha", "name": "Mocha", "price_adjustment": 0.50}
          ]
        }
      ],
      "nutrition": {
        "serving_size": {"value": 16, "unit": "oz"},
        "calories": 180,
        "fat": {"value": 9, "unit": "g"},
        "carbohydrates": {"value": 15, "unit": "g"},
        "protein": {"value": 10, "unit": "g"}
      }
    }
  ]
}
```

### 8.2 Reference Validation Tools

The OpenMenuStandard community maintains a set of validation tools to assist implementers in ensuring compliance with the specification:

1. JSON Schema: A formal schema definition that can be used with standard JSON validation tools to verify the structure of OMS documents.

2. Command-line Validator: A utility that can validate OMS files and provide detailed error reporting for non-compliant documents.

3. Web Validation Service: An online tool where implementers can upload or paste OMS documents for validation and receive immediate feedback.

4. Integration Libraries: Validation components for common programming languages, including JavaScript, Python, Java, and Swift.

These tools are available through the OpenMenuStandard website and GitHub repository.

### 8.3 Recommended Practices

Beyond the formal requirements of the specification, the following practices are recommended for optimal implementation:

1. Versioning: Include version information in both the document (oms_version) and in API endpoints to support future evolution of the standard.

2. Graceful Degradation: Implement fallback behaviors for when a receiving system does not support all features of a document.

3. Progressive Enhancement: Design systems to take advantage of advanced features when available while maintaining core functionality for basic implementations.

4. Caching Strategy: Develop appropriate caching mechanisms for menu data to balance freshness with performance.

5. Internationalization: Support multiple languages and localization formats, particularly for nutritional information that may follow different regional standards.

6. Accessibility: Ensure that implementations are accessible to users with disabilities, including screen reader compatibility and keyboard navigation.

7. Error Handling: Implement clear error messages and recovery mechanisms for when OMS documents cannot be processed correctly.

8. Analytics: Consider how anonymous, aggregated data might be used to improve menu offerings and ordering experience while respecting user privacy.

## License

The OpenMenuStandard is licensed under MIT
