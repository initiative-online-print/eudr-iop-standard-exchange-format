# EUDR IOP Standard Exchange Format (EUDR-X), Version 2.0.1

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
    - `taric_document_code` (required) as defined by the [European Commission](https://www.clecat.org/media/deforestation-reg-2023-1115---taric-data.pdf)

      Allowed values:
        - `"C716"` to declare that DDS is available.
        - `"C717"` to declare that the operator does not have to fulfill due diligence and forwards the data of the upstream supplier (only for SME operators).
        - `"Y129"` to declare that the product is not subject to EUDR although belonging to HS code in scope
          ("ex" products in [Annex I](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32023R1115&qid=1687867231461#d1e32-243-1) of the EUDR).
        - `"Y132"` to declare that in scope products have been produced before 30 December 2025.
        - `"Y133"` to declare that the product has been produced entirely from 100 % recycled material.
        - `"Y141"` to declare that the operator is a micro/small company subject to the transitional period until 30 June 2026.
        - `"Y142"` to declare that it is a non-commercial activity.
    - `hs_code` (optional) to classify the type of goods according to the Harmonized System (HS) regulated by the World Customs Organization (WCO).

      Values: 2, 4 or 6 digits as defined in the [nomenclature](https://ec.europa.eu/taxation_customs/dds2/taric/taric_consultation.jsp?Lang=en&Expand=true#afterForm).

      Example: [4901 for books](https://ec.europa.eu/taxation_customs/dds2/taric/taric_consultation.jsp?Lang=en&Taric=4901&Expand=true)

## `order_item`

identifies an unique order item.

- `order_reference` (string, required): Customer order number without the position number.
- `item_reference` (string, required): Customer item number, which must be unique for the customer in combination with
  the customer order number.
- `production_reference` (string, optional): Internal production reference.

## `referenced_eudr_statements`

references Due Diligence Statements (DDS).

Each entry contains:
- `reference_number` (string, required): Due Diligence Reference Number (DDR)
- `verification_number` (string, required): the corresponding Due Diligence Verification Number (DDV)
- `serial_shipping_container_reference` (string, optional): a unique identifier for a shipping container,
   such as a [Serial Shipping Container Code (SSCC)](https://www.gs1.org/standards/id-keys/sscc) or a tracking code
   which allows the receiver of goods to associate the DDS with certain shipments.

If `taric_document_code` is `"C716"`, exactly one entry for the own DDS is mandatory.
If `taric_document_code` is `"C717"`, at least one entry is mandatory.
In combination with other TARIC document codes, there may be no entries at all.

## `document_meta`

Contains submission metadata.

- `identifier` (string, optional): unique document identifier (e.g., UUID) within all documents created by the operator
- `submission_date` (string, required): the date of submission in ISO 8601 format – `yyyy-MM-ddTHH:mm:ss.sssZ`
- `version` (string, required): the version of the EUDR-X standard the document complies to
