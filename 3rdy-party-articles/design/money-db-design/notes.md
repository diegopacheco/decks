# How to store Money in relational DB?

Options

- string
- float
- int
- int / 100
- 2 columns: money and cents
- Decimal

PostgreSQL Numeric Types
https://www.postgresql.org/docs/current/datatype-numeric.html


DECIMAL (or NUMERIC)

This is generally considered the best option for storing monetary values.Pros:

- Precise decimal representation
- Avoids rounding errors
- Supports fractional cents
Cons:
- Slightly more storage space required compared to integers
- Slower performance for calculations compared to integers
Usage:DECIMAL(19,4)is often recommended, allowing for 15 digits before the decimal point and 4 after 12.

INTEGER (storing cents)

Storing the amount in cents as an integer is another popular approach.Pros:

- Simple to implement
- Faster calculations
- No rounding errors
Cons:
- Doesn't support fractional cents
- Requires conversion for display purposes
Usage: UseBIGINTto store large monetary values3.

FLOAT or DOUBLE

These are strongly discouraged for storing monetary values.Cons:

- Imprecise representation leading to rounding errors
- Can cause unexpected results in financial calculations14

VARCHAR (as string)

While possible, this is generally not recommended for direct monetary storage.Pros:

- Can store any precision
- Easy to read when viewing raw data
Cons:
- Requires parsing for calculations
- More storage space
- Potential for invalid data entry

Two-column approach (dollars and cents)

This is less common but can be useful in specific scenarios.Pros:

- Avoids decimal point issues
- Easy to understand
Cons:
- Requires more complex queries for calculations
- Takes up more storage space

Best Practices and Considerations

1. Always store the currency alongside the monetary value2.
2. Use a DECIMAL or INTEGER type, never FLOAT or DOUBLE45.
3. Consider using a money handling library in your application code5.
4. Be aware of different currency denominations (e.g., JPY doesn't use cents)5.
5. For multi-currency support, store the currency code in a separate column25.
6. Consider using a composite type in PostgreSQL to store value and currency together6.
7. Think about your specific use case and requirements before choosing a storage method6.
Remember, the choice depends on your specific needs, but avoiding floating-point types and ensuring precise calculations are key principles in handling money in databases.