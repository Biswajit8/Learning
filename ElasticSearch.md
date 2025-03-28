**Mapping**

> GET /index-name/_mapping

This command retrieves the mapping for the specified index.

In ElasticSearch, a mapping is like a schema definition for your data. It defines how documents and their fields are stored and indexed. Specifically, it specifies:
- Data Types: The data type of each field (e.g., text, keyword, integer, date, boolean).
- Indexing Options: How each field should be indexed and analyzed. For example, for a text field, you can specify the analyzer to use for tokenization and normalization.
- Other Metadata: Additional metadata about the fields, such as whether they are stored, whether they should be included in the _all field, and so on.

You can use this command to inspect the structure of your data and understand how fields are being indexed.

NOTE: Index is like a DB table in SQL.

---
