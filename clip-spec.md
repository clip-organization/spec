# CLIP (Context Link Interface Protocol) Specification v1.0-draft

## Table of Contents
1. [Overview](#overview)
2. [Object Types](#object-types)
3. [Core Fields](#core-fields)
4. [Optional Fields](#optional-fields)
5. [Field Specifications](#field-specifications)
6. [Services Array](#services-array)
7. [Persona Object](#persona-object)
8. [Agent/LLM Interaction Flow](#agentllm-interaction-flow)
9. [Extensibility](#extensibility)
10. [Versioning](#versioning)
11. [Security Considerations](#security-considerations)
12. [Future Considerations](#future-considerations)

## Overview

CLIP (Context Link Interface Protocol) is a universal, open JSON standard designed to expose structured, machine- and LLM-friendly context about venues, devices, or services. It enables any agent or AI to access real-time or static information about the physical or digital world in a consistent, predictable format.

### Purpose
- Provide a standardized way for physical and digital entities to describe themselves to AI agents
- Enable context-aware interactions between AI systems and real-world objects/places
- Support both static and dynamic (live) data through extensible service endpoints
- Facilitate discovery through various mechanisms (URLs, QR codes, future glyphs)

### Design Principles
- **Simplicity**: Easy to implement and consume
- **Extensibility**: Support for custom fields and future enhancements
- **LLM-Optimized**: Structured for efficient processing by language models
- **Real-time Capable**: Support for live data through service endpoints
- **Universal**: Applicable to venues, devices, and software applications

## Object Types

CLIP supports three primary object types, each designed for specific use cases:

### 1. Venue
Represents physical locations such as:
- Retail stores, restaurants, gyms
- Museums, theaters, event spaces
- Office buildings, hotels, hospitals
- Public spaces, transportation hubs

### 2. Device
Represents IoT and smart devices such as:
- Smart home appliances (refrigerators, thermostats)
- Industrial equipment and sensors
- Wearable devices
- Connected vehicles

### 3. SoftwareApp
Represents digital services and applications such as:
- Web applications
- Mobile apps
- APIs and microservices
- Digital assistants and bots

## Core Fields

All CLIP objects MUST include these required fields:

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `@context` | string | MUST be `"https://clipprotocol.org/v1"` |
| `type` | string | MUST be one of: `"Venue"`, `"Device"`, `"SoftwareApp"` |
| `id` | string | Globally unique identifier using URI format |
| `name` | string | Human-readable name of the entity |
| `description` | string | Brief description of the entity (max 500 chars) |
| `lastUpdated` | string | ISO 8601 datetime of last update |

## Optional Fields

These fields enhance the CLIP object with additional context:

| Field | Type | Description |
|-------|------|-------------|
| `location` | object | Geographic location information |
| `features` | array | List of capabilities, inventory, or characteristics |
| `actions` | array | Available interactive actions |
| `persona` | object | Agent behavior guidance |
| `services` | array | Live data endpoints and integrations |
| `glyphs` | array | Discovery mechanisms (reserved for future use) |
| `signature` | object | Digital signature for verification |
| `locales` | object | Localized content |

## Field Specifications

### @context (required)
```json
"@context": "https://clipprotocol.org/v1"
```
- Fixed value identifying the CLIP schema version
- Enables future versioning and schema evolution

### type (required)
```json
"type": "Venue" | "Device" | "SoftwareApp"
```
- Determines the primary nature of the entity
- Influences which optional fields are most relevant

### id (required)
```json
"id": "clip:us:ny:gym:fitworks-manhattan"
```
- Globally unique identifier
- Recommended format: `clip:[country]:[region]:[category]:[identifier]`
- MUST be a valid URI

### name (required)
```json
"name": "FitWorks Manhattan"
```
- Human-readable display name
- Maximum 100 characters
- Should be concise and recognizable

### description (required)
```json
"description": "Premium gym with 78 equipment stations and live class schedules."
```
- Brief overview of the entity
- Maximum 500 characters
- Should highlight key features or purpose

### lastUpdated (required)
```json
"lastUpdated": "2025-05-25T16:00:00Z"
```
- ISO 8601 format with timezone
- Indicates data freshness
- Critical for caching and synchronization

### location (optional)
```json
"location": {
  "address": "123 Main St, New York, NY 10001",
  "coordinates": {
    "latitude": 40.7589,
    "longitude": -73.9851
  },
  "timezone": "America/New_York",
  "accessibility": ["wheelchair", "elevator", "braille"]
}
```
- Physical location details
- Coordinates in decimal degrees (WGS84)
- Accessibility features as string array

### features (optional)
```json
"features": [
  {
    "name": "Smith Machine",
    "type": "equipment",
    "count": 2,
    "available": 1,
    "metadata": {
      "brand": "Life Fitness",
      "model": "SM-1000"
    }
  }
]
```
- Extensible array of capabilities or inventory
- Each feature can have custom properties
- Common properties: `name`, `type`, `count`, `available`, `status`

### actions (optional)
```json
"actions": [
  {
    "label": "Book a Class",
    "type": "link",
    "endpoint": "https://fitworks.com/book",
    "description": "Reserve your spot in group fitness classes"
  },
  {
    "label": "Start Chat",
    "type": "chat",
    "endpoint": "https://chat.openai.com/g/fitworks-coach"
  }
]
```
- Interactive capabilities available to agents
- Types: `link`, `chat`, `call`, `api`, `custom`
- Each action must have `label`, `type`, and `endpoint`

## Services Array

The services array enables live data access and advanced integrations:

```json
"services": [
  {
    "type": "http",
    "endpoint": "https://api.fitworks.com/clip/manhattan",
    "updateFrequency": "PT5M",
    "authentication": "bearer"
  },
  {
    "type": "mcp",
    "endpoint": "mcp://fitworks.com/manhattan",
    "capabilities": ["inventory", "booking", "status"]
  },
  {
    "type": "websocket",
    "endpoint": "wss://live.fitworks.com/manhattan",
    "protocol": "clip-live-v1"
  }
]
```

### Service Types

#### HTTP Service
- RESTful API endpoint returning CLIP-formatted JSON
- Supports authentication: `none`, `bearer`, `apikey`
- `updateFrequency` in ISO 8601 duration format

#### MCP Service
- Model Context Protocol integration
- Enables complex queries and interactions
- Lists available capabilities

#### WebSocket Service
- Real-time updates
- Bi-directional communication
- Custom protocol specifications

## Persona Object

The persona object guides agent behavior and interaction style:

```json
"persona": {
  "role": "Fitness Coach",
  "personality": "energetic, supportive, knowledgeable",
  "expertise": ["strength training", "nutrition", "injury prevention"],
  "prompt": "You are a certified fitness coach at FitWorks. Greet users warmly and offer to help design a workout plan based on their goals and current fitness level.",
  "constraints": [
    "Always recommend consulting a doctor before starting new programs",
    "Do not provide medical diagnosis or treatment advice"
  ],
  "preferences": {
    "communicationStyle": "encouraging",
    "responseLength": "concise",
    "useEmoji": true
  }
}
```

### Persona Properties
- `role`: Primary function or title
- `personality`: Behavioral traits
- `expertise`: Areas of specialized knowledge
- `prompt`: Specific instructions for agent behavior
- `constraints`: Limitations and boundaries
- `preferences`: Communication style guidelines

## Agent/LLM Interaction Flow

### 1. Discovery
- Agent discovers CLIP URL through QR code, NFC, or other mechanism
- Fetches the CLIP object from the provided URL

### 2. Parse and Validate
- Validates required fields and schema compliance
- Extracts relevant context based on agent capabilities

### 3. Contextualization
- Incorporates location, features, and current status
- Applies persona guidance if present

### 4. Interaction
- Presents available actions to user
- Accesses live data through services if needed
- Maintains context throughout conversation

### 5. Action Execution
- Executes user-requested actions
- Updates local context based on responses
- Handles authentication if required

### Example Flow
```
User: "What equipment is available?"
Agent: [Fetches CLIP] → [Checks features array] → [Accesses live service] → 
       "Currently, 1 of 2 Smith Machines is available, along with..."
```

## Extensibility

### Custom Fields
Implementers may add custom fields at the root level:
```json
{
  "@context": "https://clipprotocol.org/v1",
  "type": "Venue",
  "customField": "value",
  "x-vendor-extension": {...}
}
```

### Feature Metadata
The features array supports arbitrary metadata:
```json
"features": [
  {
    "name": "Conference Room A",
    "customProperty": "customValue",
    "vendorSpecific": {...}
  }
]
```

### Service Extensions
New service types can be added:
```json
"services": [
  {
    "type": "custom:vendor-protocol",
    "endpoint": "...",
    "customConfig": {...}
  }
]
```

## Versioning

### Schema Version
- Current version: `1.0-draft`
- Version indicated by `@context` URL
- Breaking changes will increment major version

### Content Versioning
- Use `lastUpdated` for content freshness
- Services may implement their own versioning
- ETags recommended for HTTP endpoints

### Migration Strategy
- New versions will be backward compatible when possible
- Deprecation notices in specification updates
- Grace period for version transitions

## Security Considerations

### Authentication
- Services should implement appropriate authentication
- Sensitive data should not be in the base CLIP object
- Use HTTPS for all endpoints

### Validation
- Implement schema validation before processing
- Sanitize all string inputs
- Verify signatures when present

### Privacy
- Minimize personally identifiable information
- Respect user consent for data access
- Implement appropriate data retention policies

## Future Considerations

### Glyphs (Reserved)
The `glyphs` array is reserved for future discovery mechanisms:
```json
"glyphs": [
  {
    "type": "qr",
    "data": "...",
    "format": "clip-qr-v1"
  },
  {
    "type": "hexmap",
    "data": "...",
    "format": "hm-v1"
  }
]
```

### Signature Object (Reserved)
Digital signatures for verification:
```json
"signature": {
  "algorithm": "ES256",
  "keyId": "...",
  "signature": "..."
}
```

### Enhanced Localization
Future support for comprehensive localization:
```json
"locales": {
  "es": {
    "name": "FitWorks Manhattan",
    "description": "Gimnasio premium con 78 estaciones..."
  }
}
```

---

## Appendix: Complete Example

```json
{
  "@context": "https://clipprotocol.org/v1",
  "type": "Venue",
  "id": "clip:us:ny:gym:fitworks-manhattan",
  "name": "FitWorks Manhattan",
  "description": "Premium gym with 78 equipment stations and live class schedules.",
  "lastUpdated": "2025-05-25T16:00:00Z",
  "location": {
    "address": "123 Fitness Ave, New York, NY 10001",
    "coordinates": {
      "latitude": 40.7589,
      "longitude": -73.9851
    },
    "timezone": "America/New_York"
  },
  "features": [
    {
      "name": "Smith Machine",
      "type": "equipment",
      "count": 2,
      "available": 1
    },
    {
      "name": "Group Fitness Studio",
      "type": "facility",
      "capacity": 30,
      "schedule": "See class schedule"
    }
  ],
  "actions": [
    {
      "label": "Book a Class",
      "type": "link",
      "endpoint": "https://fitworks.com/manhattan/book"
    },
    {
      "label": "Chat with Coach",
      "type": "chat",
      "endpoint": "https://chat.openai.com/g/fitworks-coach"
    }
  ],
  "persona": {
    "role": "Fitness Coach",
    "personality": "energetic, supportive",
    "prompt": "You are a certified fitness coach. Help users with workout plans and fitness advice."
  },
  "services": [
    {
      "type": "http",
      "endpoint": "https://api.fitworks.com/clip/manhattan",
      "updateFrequency": "PT5M"
    }
  ]
}
```

---

*This specification is a living document. For the latest version and updates, visit [https://github.com/clip-organization/spec](https://github.com/clip-organization/spec)* 