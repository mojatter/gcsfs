# gcsfs

[![PkgGoDev](https://pkg.go.dev/badge/github.com/mojatter/gcsfs)](https://pkg.go.dev/github.com/mojatter/gcsfs)
[![Report Card](https://goreportcard.com/badge/github.com/mojatter/gcsfs)](https://goreportcard.com/report/github.com/mojatter/gcsfs)
[![Tests](https://github.com/mojatter/gcsfs/actions/workflows/tests.yaml/badge.svg)](https://github.com/mojatter/gcsfs/actions/workflows/tests.yaml)

Package gcsfs provides an implementation of [wfs](https://github.com/mojatter/wfs) for GCS (Google Cloud Storage).

Requires Go 1.25 or later.

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

### Explicit client

When you need to control the GCS configuration (credentials, custom
endpoint, etc.), construct the storage client yourself and pass it in:

```go
package main

import (
  "context"
  "log"

  "cloud.google.com/go/storage"
  "github.com/mojatter/gcsfs"
)

func main() {
  ctx := context.Background()
  client, err := storage.NewClient(ctx)
  if err != nil {
    log.Fatal(err)
  }
  fsys := gcsfs.NewWithClient("<your-bucket>", client).
    WithContext(ctx)
  defer fsys.Close()
  // use fsys ...
  _ = fsys
}
```

## Capability layers

gcsfs implements the following [wfs](https://github.com/mojatter/wfs)
capability interfaces:

| Capability | Interface | Notes |
| --- | --- | --- |
| Read | `fs.FS`, `fs.GlobFS`, `fs.ReadDirFS`, `fs.ReadFileFS`, `fs.StatFS`, `fs.SubFS` | |
| Write | `wfs.WriteFileFS` | `MkdirAll` is a no-op (GCS has no directories) |
| Remove | `wfs.RemoveFileFS` | |
| Rename | `wfs.RenameFS` | Implemented via CopierFrom + Delete |
| Sync | `wfs.SyncWriterFile` | No-op (GCS writes atomically on Close) |

## Tests

gcsfs can pass TestFS in `testing/fstest`.

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

Integration tests run against a real GCS bucket and require
[Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials):

```sh
gcloud auth application-default login
```

Then run:

```sh
FSTEST_BUCKET="<your-bucket>" \
FSTEST_EXPECTED="<your-expected>" \
  go test -tags integtest ./...
```
