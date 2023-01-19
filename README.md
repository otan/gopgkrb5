# gopgkrb5
Adds the dependencies necessary for Go PG drivers to access krb5.

It is a separate repository for [reducing dependencies](https://github.com/lib/pq/issues/971)
and is heavily inherited from the [lib/pq library](https://github.com/lib/pq).

## Usage with `jackc/pgconn`  and friends

Add the library:

```
go get github.com/otan/gopgkrb5
```

and initialise the package and import the package into `pgconn`:

```go
import (
 "github.com/jackc/pgconn"
 "github.com/otan/gopgkrb5"
)

func init() {
  pgconn.RegisterGSSProvider(func() (pgconn.GSS, error) { return gopgkrb5.NewGSS() })
}
```

### Unix only features

Optional settings are available:

```go
import (
 "github.com/jcmturner/gokrb5/v8/client"
 "github.com/jackc/pgconn"
 "github.com/otan/gopgkrb5"
)

func init() {
  pgconn.RegisterGSSProvider(func() (pgconn.GSS, error) { return gopgkrb5.NewGSS(client.NewSettings(...)) })
}
```

You can also use keytab to log in:
```go
func init() {
  pgconn.RegisterGSSProvider(func() (pgconn.GSS, error) {
	  return gopgkrb5.NewGSSWithKeytab("username", "DOMAIN.LOCAL", "/test.keytab")
  })
}
```

or password:
```go
func init() {
  pgconn.RegisterGSSProvider(func() (pgconn.GSS, error) {
	  return gopgkrb5.NewGSSWithPassword("username", "DOMAIN.LOCAL", "password")
  })
}
```

All dependencies of [`jackc/pgconn`](https://github.com/jackc/pgconn), e.g. [`jackc/pgx`](https://github.com/jackc/pgx) will now be able to authenticate with
GSSAPI/krb5.
