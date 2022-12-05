# Dynamic filters

Intelie Live supports that the Pipes filters react to dynamic parameters of a query. This is useful to support interfaces that allow the user to change the filters without the need of changing the query expression.

The dynamic values are passed in the `lookupValues` argument of the Query object. This argument is a  `map<string, list<string>>`, where the key is the name of a Lookup Table and the value is a list of the desired keys in that Lookup Table. Intelie Live will apply to the query the values corresponding to each of those keys.

### Lookup Tables

Lookup Tables are structures that associate multiple values with keys. They can be created  programmatically through the API or through the web interface. A Lookup Table created programmatically will not be editable through the web interface.

![Web interface to manage lookup tables](<../.gitbook/assets/image (112).png>)

### The @@lookup macros

The `@@lookup` macro allows the lookup values to be used on the query. For every Lookup Table created, a `@@lookup.name_of_the_table` macro will be available.

![Example of the @@lookup macros](<../.gitbook/assets/image (58).png>)

When this macro is used on a query, it will be dynamically replaced by the values corresponding to the Lookup Table keys in the `lookupValues` query argument.

### The lookup functions

Some functions are also created to facilitate the use of the Lookup Tables keys and values on any Pipes query.

![Example of auxiliar Pipes functions](<../.gitbook/assets/image (36).png>)
