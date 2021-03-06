# htmldumper
Parsoid HTML dump script for RESTBase APIs like https://rest.wikimedia.org/.

## Installation

`npm install`

## Usage

```
Usage: node ./bin/dump_wiki
Example: node ./bin/dump_wiki --domain en.wikipedia.org \
  --ns 0 --apiURL http://en.wikipedia.org/w/api.php \
  --saveDir /tmp

Options:
  --apiURL       [required]
  --domain       [required]
  --ns           [required]
  --host         [required] [default: "http://rest.wikimedia.org"]
  -d, --saveDir  Directory to store a dump in (named by domain) [default: no saving]
  --db, --dataBase  SQLite database name [default: no saving]
```

### Filesystem output

With `--saveDir` as specified in the example above, a directory structure like
this will be created:

```
/tmp/
  en.wikikpedia.org/
    Aaa/
      123456
    Bbbb/
      456768
```

The directory names for articles are percent-encoded using JavaScript's
`encodeURIComponent()`. On a repeat run with the same `--saveDir` path, only
updated articles are downloaded. Outdated revisions are deleted. These
incremental dumps speed up the process significantly, and reduce the load on
the servers.

### SQLite database output

With `--dataBase` set to `someSQLiteDB.db`, a database will be created /
updated. The schema currently looks like this:

```sql
REATE TABLE data(
    title TEXT,
    revision INTEGER,
    tid TEXT,
    body TEXT,
    page_id INTEGER,
    namespace INTEGER,
    timestamp TEXT,
    comment TEXT,
    user_name TEXT,
    user_id INTEGER,
    PRIMARY KEY(title ASC, revision DESC)
);
```
