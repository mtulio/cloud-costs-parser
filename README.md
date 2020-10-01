# cloud-costs-parser (State:Proposal)

The Cloud Costs processor (Import/Export to common DB)

## Description

Cloud Costs parser import CSV file costs data from Cloud Provider (initially AWS and Azure) and export to a common database - preferably NoSQL.

## Command (SPEC suggested)

### Fields `fields`

Extract the fields from the source file and show the output (without parsing).

### Parser `parser`

Command to start the parser.

#### Subcommand

- `--vendor`

The Cloud provider to create metadata field `vendor`

- `--in`

The value of CSV file path. The source driver could be discovered the source (s3, gzip, csv, blob, etc)

Example of AWS CUR v2: `--in s3://my-bucker-reported-by-cur/path/to/monthlyReport/20201001-20201101`

Will try to read all CSV files inside the path. See `--latest` option to save extra processing.

- `--latest`

Read latest data. Latest by default will be D-1 (last day). See `--latest-days N`

This option could save extra operation on the destination storage (less read/comparation/upserts)

- `--latest-days N`

Overrides `--latest` to define how many days to parse the data. By default it will get N days from now until the end of file.

If the monthly processor range is older than current month, one option could be created consider N days from last day of month. (v2)

This option could save extra operation on the destination storage (less read/comparation/upserts)

- `--override-char --override-from CHAR_ORIGIN --override-to CHAR_DEST`

Enable the override character on field name, usefull when destination storage does not support special characters on field name, or you just want to have a better view without nesting first level of dictionaries on NoSQL databases (it will save complexity on queries);

Example on AWS CUR v2: `--override-char --override-from / --override-to _`

The field `identity/LineItemId` on CSV will be saved as `identity_LineItemId`

- `--key-fields`

Fields sepparated by commad that will be used to UID (Key) of cost item.

For example on AWS CUR the following fields could be used to identify the cost item on whole file: `identity_LineItemId,identity_TimeStart`

- `--out`

The value of stdout storage, could be MySQL, PGSQL, ArangoDB, S3.


Example:

`--out arangodb://server:5432/database/collection?username=x,password=y`

`--out psql://server:5432/database/table?username=x,password=y`

`--out aztables://stgAccount/TableName?storageKey=x`
