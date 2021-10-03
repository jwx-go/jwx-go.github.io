# The JWX Toolkit

Documents found here contain pointers to resources about [jwx](https://github.com/lestrrat-go/jwx), a set of libraries that handles the various JW(x) technologies including JWT, JWS, JWE, JWK, and JWA.

Please note that these documents do not intend to explain how JWT et al work. You will have to look elsewhere to understand how these technologies work.

# SYNOPSIS

```go
import (
  "encoding/json"
  "fmt"
  "os"

  "github.com/lestrrat-go/jwx/jwk"
  "github.com/lestrrat-go/jwx/jwt"
)

func main() {
	if err := _main(); err != nil {
		fmt.Fprintf(os.Stderr, "%s\n", err)
		os.Exit(1)
  }
}

func _main() error {
	keyset, err := jwk.ReadFile("/path/to/keyset.json")
	if err != nil {
		return fmt.Errorf(`failed to read JWKS from file: %w`, err)
	}

  payload, err := os.ReadFile("/path/to/payload.txt")
  if err != nil {
    return fmt.Errorf(`failed to read payload from file: %w`, err)
  }

  token, err := jwt.Parse(
    payload,
    jwt.InferAlgorithm(true), # Allow guessing algorithm
    jwt.WithKeySet(keyset),   # Use keyset to verify JWS
    jwt.Validate(true),       # Validate JWT token itself
  )
  if err != nil {
    return fmt.Errorf(`failed to parse/verify/validate token: %w`, err)
  }

  enc := json.NewEncoder(os.Stdout)
  enc.SetIndent("", "  ")
  if err := enc.Encode(token); err != nil {
    return fmt.Errorf(`failed to encode token: %w`, err)
  }
}
```

# Documentation available with the source code

* [How to JWx That](https://github.com/lestrrat-go/jwx/docs)
* [Runnable Examples](https://github.com/lestrrat-go/jwx/examples)

