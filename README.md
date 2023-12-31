# mvs-dump

This is an attempt at the revival of **MVS dump**.

So far the distribution format was an archive with several files. The problems are as follows:

 * Gigantic text files - difficult to search through.
 * Bad format.
 * Megabytes of useless data.
 * Invalid hash priority for some files (`mvs.txt`).

This approach uses an SQLite database. This time without the uselessness of product notes (useful only for MVS subscribers, none of whom would need the dump in the first place) and release dates. Not directly searchable, but very easy to programatically dig through or display.

Made for the Glory of Hastur, by awuctl.

## Database structure

The database consists of two tables defined as follows:

```sql
CREATE TABLE products (
    id        INT      PRIMARY KEY,
    name      VARCHAR
);
```
```sql
CREATE TABLE files (
    id        INT,
    product   INT,
    name      VARCHAR,
    desc      VARCHAR,
    langc     VARCHAR,
    bootstrap VARCHAR  DEFAULT NULL,
    sha1      VARCHAR  DEFAULT NULL,
    sha2      VARCHAR  DEFAULT NULL,

    FOREIGN KEY(product) REFERENCES products(id),
    PRIMARY KEY(id, product)
);
```
## Use

Normal use requires the user to be a Visual Studio subscriber with access to relevant subscriber benefits. The user is authorized with Microsoft servers using a genuine authentication cookie. You can get the cookie from your MS account credentials by putting them in `secrets/email` and `secrets/password` files and using `get_cookie.py`.

The first step is to generate the skeleton for the database with `mkdb.py`:

```sh
python mkdb.py mvs.db
```

Next, populate the database with MVS data:

```sh
python mvs_dump.py mvs.db <count>
# count - Number of consecutive IDs to query for (set to ~10000)
```

The resulting database can be used with *SQlite3* or browsed in any SQLite-compatible program.


## Disclaimer
Nothing stored in the databases or done by this tool infringes Microsoft copyright.

Files acquired through Visual Studio subscriber benefits are the sole property of Microsoft Corporation and their suppliers. These files **are not** available for download by use of this tool or the databases in any way.

The purpose of this repository and the databases is to provide users with the means of verifying genuine Microsoft software in a transparent manner.
