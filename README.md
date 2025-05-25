# CLIP (Context Link Interface Protocol)

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![JSON Schema](https://img.shields.io/badge/JSON%20Schema-Draft%202020--12-green.svg)](clip.schema.json)
[![Status](https://img.shields.io/badge/Status-v1.0--draft-orange.svg)](clip-spec.md)

The official specification, schema, and examples for the CLIP (Context Link Interface Protocol) standard. CLIP enables venues, devices, and services to expose structured, machine- and LLM-friendly context to AI agents and applications.

## 🎯 What is CLIP?

CLIP is a universal JSON standard that allows physical and digital entities to describe themselves to AI systems. Think of it as a "context card" that any location, device, or service can present to make itself understandable to AI agents.

### Key Benefits
- 🤖 **AI-Native**: Designed specifically for LLMs and intelligent agents
- 🔄 **Real-time Ready**: Supports both static and live data through service endpoints
- 🌍 **Universal**: Works for venues, IoT devices, and software applications
- 🔌 **Extensible**: Add custom fields and integrate with existing systems
- 🚀 **Simple**: Easy to implement with just 6 required fields

## 🎯 Who is this for?

- **🏢 Business Owners**: Make your venue discoverable and interactive for AI assistants
- **🔧 Device Manufacturers**: Enable smart devices to communicate their state and capabilities
- **👩‍💻 Developers**: Build AI-powered apps that understand real-world context
- **🤖 AI/Agent Developers**: Access structured context about the physical world

## 📋 How does a Clip work?

1. **Discovery**: An AI agent discovers a CLIP URL (via QR code, NFC, or other methods)
2. **Fetch**: The agent retrieves the JSON object from the URL
3. **Parse**: The agent validates and extracts relevant context
4. **Interact**: The agent uses available actions and real-time services
5. **Personalize**: The agent applies persona guidance for appropriate behavior

## 🚀 Quick Start

### Basic CLIP Example

```json
{
  "@context": "https://clipprotocol.org/v1",
  "type": "Venue",
  "id": "clip:us:ny:coffee:joes-manhattan",
  "name": "Joe's Coffee",
  "description": "Cozy neighborhood coffee shop serving artisan coffee and fresh pastries since 2010.",
  "lastUpdated": "2025-05-25T08:00:00Z"
}
```

That's it! With just these 6 required fields, you have a valid CLIP.

### Full Example with Optional Fields

See our comprehensive examples:
- 🏋️ [Gym](examples/gym.json) - Fitness center with equipment tracking
- 🏠 [Smart Fridge](examples/fridge.json) - IoT device with inventory management  
- 🏛️ [Museum](examples/museum.json) - Cultural venue with exhibits and tours

## 📑 Documentation

- 📖 **[Full Specification](clip-spec.md)** - Complete technical documentation
- 📐 **[JSON Schema](clip.schema.json)** - Validation schema (JSON Schema Draft 2020-12)
- 📁 **[Examples](examples/)** - Real-world implementation examples

## ✅ Validating Your CLIP

### Using Python
```python
import json
import jsonschema

# Load the schema
with open('clip.schema.json') as f:
    schema = json.load(f)

# Load your CLIP
with open('your-clip.json') as f:
    clip = json.load(f)

# Validate
jsonschema.validate(clip, schema)
```

### Using Node.js
```javascript
const Ajv = require('ajv');
const ajv = new Ajv();

const schema = require('./clip.schema.json');
const clip = require('./your-clip.json');

const validate = ajv.compile(schema);
const valid = validate(clip);

if (!valid) console.log(validate.errors);
```

### Online Validators
- [JSONSchema.net](https://www.jsonschema.net/)
- [JSON Schema Validator](https://www.jsonschemavalidator.net/)

## 🔧 Implementation Guide

### 1. Choose Your Object Type
- **Venue**: Physical locations (stores, restaurants, offices)
- **Device**: IoT and smart devices (appliances, sensors, vehicles)
- **SoftwareApp**: Digital services (web apps, APIs, bots)

### 2. Start with Required Fields
```json
{
  "@context": "https://clipprotocol.org/v1",
  "type": "YOUR_TYPE",
  "id": "clip:unique:identifier",
  "name": "Human Readable Name",
  "description": "Brief description (max 500 chars)",
  "lastUpdated": "2025-05-25T12:00:00Z"
}
```

### 3. Add Relevant Optional Fields
- `location`: For physical venues
- `features`: List capabilities or inventory
- `actions`: Define interactive options
- `persona`: Guide AI behavior
- `services`: Enable live data access

### 4. Serve Your CLIP
- Host at a stable URL (HTTPS recommended)
- Set `Content-Type: application/json`
- Enable CORS if needed for web access
- Consider caching strategies

## 🌐 Object Types

### Venue
Physical locations with features like equipment, facilities, or exhibits.
```json
{
  "type": "Venue",
  "features": [
    {"name": "Conference Room A", "capacity": 20, "available": true}
  ]
}
```

### Device  
Smart devices with state, inventory, or sensor data.
```json
{
  "type": "Device",
  "features": [
    {"name": "Temperature", "value": 72, "unit": "fahrenheit"}
  ]
}
```

### SoftwareApp
Digital services with capabilities and integrations.
```json
{
  "type": "SoftwareApp",
  "services": [
    {"type": "mcp", "endpoint": "mcp://app.com/service"}
  ]
}
```

## 📊 Versioning

- **Current Version**: v1.0-draft
- **Schema Version**: Indicated by `@context` field
- **Breaking Changes**: Will increment major version
- **Non-breaking Updates**: Minor version increments

The specification is in draft status and subject to change based on community feedback.

## 🤝 Contributing

We welcome contributions! Please:

1. **File Issues**: Report bugs or suggest features
2. **Submit PRs**: Improvements to spec, schema, or examples
3. **Share Feedback**: Real-world implementation experiences
4. **Add Examples**: New use cases and object types

### Contribution Process
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Validate all examples against the schema
5. Submit a pull request

## 🔒 Security Considerations

- Use HTTPS for all CLIP URLs
- Don't include sensitive data in base CLIP objects
- Implement appropriate authentication for services
- Validate and sanitize all inputs
- See [specification](clip-spec.md#security-considerations) for details

## 📱 Discovery Methods

Currently, CLIP URLs can be discovered via:
- QR codes
- NFC tags  
- URLs in apps/websites
- Direct sharing

Future versions may support additional discovery mechanisms (glyphs).

## 🛠️ Tools & Resources

### Official
- [CLIP Specification](https://github.com/clip-organization/spec)
- [Schema Validator](clip.schema.json)

### Community
- Implementation libraries (coming soon)
- CLIP generators (coming soon)
- Discovery tools (coming soon)

## ❓ FAQ

**Q: How is CLIP different from other standards?**  
A: CLIP is designed specifically for AI/LLM consumption, with features like persona guidance and real-time service integration.

**Q: Can I add custom fields?**  
A: Yes! CLIP is extensible. Add any fields you need at the root level or within features.

**Q: What about authentication?**  
A: Base CLIP objects are public. Use authentication on service endpoints for sensitive data.

**Q: How often should I update `lastUpdated`?**  
A: Whenever the content meaningfully changes. This helps with caching and freshness.

## 📜 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- **Website**: [clipprotocol.org](https://clipprotocol.org) (coming soon)
- **GitHub**: [github.com/clip-organization/spec](https://github.com/clip-organization/spec)
- **Discussion**: [GitHub Discussions](https://github.com/clip-organization/spec/discussions)

---

*CLIP is an open standard. We believe in a future where every place, device, and service can communicate its context to AI systems, creating more intelligent and helpful interactions for everyone.*
