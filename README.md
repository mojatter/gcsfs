# github.com/mojatter/gcsfs

[![PkgGoDev](https://pkg.go.dev/badge/github.com/mojatter/gcsfs)](https://pkg.go.dev/github.com/mojatter/gcsfs)
[![Report Card](https://goreportcard.com/badge/github.com/mojatter/gcsfs)](https://goreportcard.com/report/github.com/mojatter/gcsfs)

Package gcsfs provides an implementation of [wfs](https://github.com/mojatter/wfs) for GCS (Google Cloud Storage).

## Examples

### ReadDir

```go
package main

import (
  "fmt"
  "io/fs"
  "log"

  "github.com/mojatter/gcsfs"
)

func main() {
  fsys := gcsfs.New("<your-bucket>")
  entries, err := fs.ReadDir(fsys, ".")
  if err != nil {
    log.Fatal(err)
  }
  for _, entry := range entries {
    fmt.Println(entry.Name())
  }
}
```

### WriteFile

```go
package main

import (
  "io/fs"
  "log"

  "github.com/mojatter/wfs"
  "github.com/mojatter/gcsfs"
)

func main() {
  fsys := gcsfs.New("<your-bucket>")
  _, err := wfs.WriteFile(fsys, "test.txt", []byte(`Hello`), fs.ModePerm)
  if err != nil {
    log.Fatal(err)
  }
}
```

## Tests

GCSFS can pass TestFS in "testing/fstest".

```go
import (
  "testing/fstest"

  "github.com/mojatter/gcsfs"
)

// ...

fsys := gcsfs.New("<your-bucket>")
if err := fstest.TestFS(fsys, "<your-expected>"); err != nil {
  t.Errorf("Error testing/fstest: %+v", err)
}
```

## Integration tests

```sh
FSTEST_BUCKET="<your-bucket>" \
FSTEST_EXPECTED="<your-expected>" \
  go test -tags integtest ./...
```
