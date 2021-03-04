# pq - A pure Go openGauss driver for Go's database/sql package

fork from [github/lib/pq](https://github/lib/pq)

## Install

	go get github.com/enmotech/pq

## What's the difference of libpq for openGauss
When using original libpq go driver to access openGauss, the following error will be reported.
```
 pq: Invalid username/password,login denied.
```
The reason is that openGauss default user connection password authentication method is sha256, which is a unique encryption method. Although openGauss configuration can be modified by the following methods to support native libpq connection.

1. Set the openGauss initialization parameter password_encryption_type.
```
alter system set password_encryption_type=0;
```
3. Set pg_hba.conf to allow md5 password verification: host all test 0.0.0.0/0 md5
4. Create a new user in database, then connect by this user.

We still prefer to use a more secure encryption method like sha256, so the modified libpq can be directly compatible with sha256.

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

