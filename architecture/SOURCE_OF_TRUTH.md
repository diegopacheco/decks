# SOURCE OF TRUTH

It's concept that means what database or system is considered the authoritative source for a particular piece of information. It's comon in distributed systems to have multiple databases or even systems that store the same data. In such cases, it's crucial to designate one of these as the "source of truth" to ensure consistency and reliability of the data across the entire system.

The same concept is used in migrations. While you are migrating from System A to System B, System A is the source of truth until the migration is complete and verified. After that, System B becomes the new source of truth.