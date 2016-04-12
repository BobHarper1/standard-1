# Serialization

The Open Contracting Data Standard provides a **structured data model** for capturing in-depth information about all stages of the contracting process.

The current canonical version of this data model is provided by a **[JSON Schema](../../../schema/release)** which describes field names, field definitions and structures for the data. The compliance of data with the Open Contracting Data Standard will be assessed against this schema.

However, there are many use cases where publishers and users will want to work with data serialized in other formats. For this reason, the current version of OCDS supports a number of **secondary serializations** which are based on the canonical schema. There are not currently official components of the standard, but are designed to support implementers in providing accessible data to a range of different users.

## JSON 

JSON stands for Javascript Object Notation, and is a format widely used for the exchange of data on the web. The JSON schema language provides validation tools for working with JSON data.

### Generating JSON

Most programming languages provide tools for output data as JSON. 

The [OCDS Mapper](https://github.com/open-contracting/mapper) tool can convert from flat CSV files to structured OCDS data based on a mapping template. 

[A range of tools](http://json-schema.org/implementations.html) are available for working with [JSON-Schema](http://json-schema.org/), including validation and form generation tools. 

### Consuming JSON

Most programming languages provide tools for reading JSON.

A number of [JSON native databases](http://en.wikipedia.org/wiki/NoSQL) are available for working directly with large collections of JSON documents, and command line tools such as [jq](http://stedolan.github.io/jq/) support advanced query and data retrieval with JSON files.

There are also a range of generic tools which can convert JSON into flat CSV structures, including:

* [JSON -> CSV](http://konklone.io/json/) - online tool for converting small documents.
* [Open Refine](http://openrefine.org/) - desktop tool that can handle large documents, and supports advanced data manipulation.

## CSV 

JSON is based on a tree structure, with data elements nested inside one another. However, many people are more familiar working with tabular data, made up of columns and rows. There is no easy way to represent structured data in a single table. However, we propose two models for publishers to adopt. 

* **Simplified single table** - for cases where there are no one-to-many relationships in the data (e.g. each tender has only one award and contract, and each has only one line-item).
* **Multi-table** - where more advanced structures are required, but where it is desirable to be able to work with data in spreadsheet-style layouts

In each case, fields are identified by the [json pointer](http://tools.ietf.org/html/rfc6901) to their JSON equivalent. For example:

**JSON**

```eval_rst

.. jsoninclude:: docs/en/examples/serialization-flat.json
   :jsonpointer: 
   :expand: releases, tender, items

```

**CSV**

```eval_rst

.. csv-table::
   :header-rows: 1
   :file: standard/docs/en/examples/serialization-flat.csv
   
```

A set of prototype tools for generating flat CSV OCDS templates are [available on GitHub](https://github.com/open-contracting/flattening-ocds), and the OCDS validator includes an option to generate 'flattened' versions of JSON data.

### Simplified single table 

It is possible to represent a full releases in a single flat CSV row by using full JSON pointers for each field as the headings. 

This approach is generally only appropriate for data without one-to-many relationships (for example, only one item per tender, and one award and contract for each tender process).

It is, however, theoretically possible to represent a full releases in a single flat CSV row by using full JSON pointers for each field as the headings. For arrays, this involves adding the array index to the path, such as ```tender/item/0/id``` and ```tender/item/1/id``` as separate columns to represent each of the items. 

For example, to represent a tender release with two items, the CSV file would include:

```eval_rst

.. csv-table::
   :header-rows: 1
   :file: standard/docs/en/examples/serialization-flat-two-items.csv
   
```

The JSON equivalent of this would be:

```eval_rst

.. jsoninclude:: docs/en/examples/serialization-flat-two-items.json
   :jsonpointer: 
   :expand: releases, tender, items

```

Whilst this allows complex data to be expressed in flat CSV, users will need to rebuild the structure in order to analyse the data.

Instead, data with a one-to-many relationship can be represented using a multi-table serialization. 

### Multi-table

The multi-table serialization breaks key sections and building blocks of OCDS into their own tables. This supports complex data structures with one-to-many relationships, whilst allowing analysts to reconstruct the views of data they need to carry out analysis. 

Multiple tables can be packaged together as the tabs of a spreadsheet, or using the Open Knowledge data package specification. 

The OCDS approach to multi-table flat serializations is currently under review. Contact the [helpdesk](../../../support/) if you wish to make use of this. 

### Packaging files with meta-data

Whatever serialisation is used for Open Contracting Data, a single file may contain one or more release and records.

The release and record data package schemas describe the key meta-data that must be supplied for any file providing Open Contracting Data. This includes the publishedDate, publisher, uri for accessing the file, and the licensing details for the file.