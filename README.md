# pq - A pure Go openGauss driver for Go's database/sql package

fork from [github/lib/pq](https://github/lib/pq)

## Install

	go get github.com/enmotech/pq

## Features

* Adapt openGauss SHA256 password authentication
* SSL
* Handles bad connections for `database/sql`
* Scan `time.Time` correctly (i.e. `timestamp[tz]`, `time[tz]`, `date`)
* Scan binary blobs correctly (i.e. `bytea`)
* Package for `hstore` support
* COPY FROM support
* pq.ParseURL for converting urls to connection strings for sql.Open.
* Many libpq compatible environment variables
* Unix socket support
* Notifications: `LISTEN`/`NOTIFY`
* pgpass support
* GSS (Kerberos) auth


## Example
```
import (
	"database/sql"

	_ "github.com/enmotech/pq"
)

func main() {
	connStr := "user=pqgotest dbname=pqgotest sslmode=verify-full"
	db, err := sql.Open("opengauss", connStr)
	if err != nil {
		log.Fatal(err)
	}

	age := 21
	rows, err := db.Query("SELECT name FROM users WHERE age = $1", age)
	…
}
```

## Tests

`go test` is used for testing.  See [TESTS.md](TESTS.md) for more details.

