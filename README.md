# EUDR IOP Standard Exchange Format (EUDR-X)

EUDR-X is a **standardized format** developed by the ESG working group of the *Initiative Online Print e.V. (IOP)*
for the structured exchange of data related to the *EU Deforestation Regulation (EUDR)*.

The [specification](./SPECIFICATION.md) was developed to support the exchange of data throughout the supply chain
and to enable companies to report Due Diligence Statement (DDS) reference numbers (DDR) and verification numbers (DDV)
in a standardized and machine-readable way.

## Formats

### JSON (official format, recommended)

The default and recommended EUDR-X format is JSON as described in the [specification](./SPECIFICATION.md).

JSON files must be compliant with the [JSON schema](./json/schema.json).

Example: [example.json](./json/example.json)

### XML (alternative format)

Alternatively, an XML file may be used with an additional root element (`<eudr>`) and equivalent nodes to the JSON
representation.

**If required by legacy systems, the XML format may be used as an alternative to JSON.** It is strongly recommended that
all systems supporting JSON files use the JSON format instead.

XML files must be compliant to the [XML Schema Definition (XSD)](./xml/schema.xsd)
or [Document Type Definition (DTD)](./xml/schema.dtd).
It is recommended to use the XML Schema as it contains additional restrictions for the enum values.

Example: [example.xml](./xml/example.xml)

### CSV (not recommended)

The use of the CSV is **strongly discouraged** as CSV cannot represent nested structures (objects, arrays).

Example: [example.csv](./csv/example.csv)

## Reference implementations

In the future, implementations of the EUDR-X specification will be listed here.

Implementations *MUST* support the JSON format and *SHOULD* support the alternative XML.
