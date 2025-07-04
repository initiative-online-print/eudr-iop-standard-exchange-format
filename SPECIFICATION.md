# EUDR IOP Standard Exchange Format (EUDR-X), Version 1.0.0

The JSON object represents a single data record containing metadata, operator details, information on the related order
item, and Due Diligence Statements (DDS) with their reference and verification numbers.

The data is submitted as a single JSON object. It contains the following top-level fields:

- [`operator`](#operator) (object, required),
- [`order_item`](#order_item) (object, required),
- [`referenced_eudr_statements`](#referenced_eudr_statements) (array of objects, required) and
- [`document_meta`](#document_meta)  (object, required).

## `operator`

Contains information about the reporting company.

- `company_name` (string, required): Name of the company (free text).
- `address` (object, required):
    - `street` (string, required): street name and house number
    - `postal_code` (string, required): the postal code
    - `city` (string, required): city
    - `country` (string with two characters, required): country code (according
      to [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1))
- `contact_person` (object, required):
    - `name` (string, required): the full name
    - `email` (string, required): email address (must be valid)
    - `phone` (string, required): telephone number incl. country code with optional formatting characters
- `eudr_details` (object, required):
    - `eudr_role` (required) describes whether the operator acts an operator or trader
      and whether the operator is a Small or Medium Enterprise (SME) or a Large Company (LC) according to the EUDR.

      Allowed values:
        - `"SME-Operator"`
        - `"SME-Trader"`
        - `"LC-Operator"`
        - `"LC-Trader"`
    - `eudr_dds_responsibility` (required) defines who is responsible for the **Due Diligence Statement (DDS)** in the
      context of this transaction.

      Allowed values:
        - `"own"` indicates that **the operator itself has created the DDS**. The data submitted is original and based
          on the operator’s internal risk assessment and traceability.
        - `"supplier"` means the DDS referenced are created **by other operators in the supply chain**. The current
          operator references an existing DDS provided by its supplier.
    - `eudr_document_type` (required) defines the document type.

      Allowed values:
        - `"DownstreamReference"` indicates that the operator is not responsible for the initial declaration of a DDS,
          but provides reference number(s) of suppliers/traders.
        - `"FirstPlacerReference"` specifies that the document is the initial declaration of a DDS by the operator.

## `order_item`

identifies an unique order item.

- `order_reference` (string, required): Customer order number without the position number.
- `item_reference` (string, required): Customer item number, which must be unique for the customer in combination with
  the customer order number.
- `production_reference` (string, optional): Internal production reference.

## `referenced_eudr_statements`

Contains at least one item. Each entry contains:

- `reference_number` (string, required): Due Diligence Reference Number (DDR)
- `verification_number` (string, required): the corresponding Due Diligence Verification Number (DDV)

Minimum one entry required. If `eudr_dds_responsibility` is `"own"`, exactly one entry is mandatory.

## `document_meta`

Contains submission metadata.

- `identifier` (string, optional): unique document identifier (e.g., UUID) within all documents created by the operator
- `submission_date` (string, required): the date of submission in ISO 8601 format – `yyyy-MM-ddTHH:mm:ss.sssZ`
