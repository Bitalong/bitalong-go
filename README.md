# Bitalong OpenAPI Client
=====

## Summary

Go client for Bitalong openapi client. Fork from go-gdax.

## Installation
```sh
go get github.com/Bitalong/bitalong-go
```
***!!! As of 0.5 this library uses strings and is not backwards compatible with previous versions, to install previous versions, please use a tool like GoDep***

## Documentation
For full details on functionality, see GoDoc documentation.

### Setup
How to create a client:

```go

import (
  "os"
  bita "github.com/Bitalong/bitalong-go"
)

secret := os.Getenv("BITALONG_SECRET")
key := os.Getenv("BITALONG_KEY")
passphrase := os.Getenv("BITALONG_PASSPHRASE")

// or unsafe hardcode way
secret = "exposedsecret"
key = "exposedkey"
passphrase = "exposedpassphrase"

client := bita.NewClient(secret, key, passphrase)
```

### HTTP Settings
```go
import (
  "net/http"
  "time"
)

client.HttpClient = &http.Client {
  Timeout: 15 * time.Second,
}
```

### Decimals
To manage precision correctly, this library sends all price values as strings. It is recommended to use a decimal library like Spring's Decimal if you are doing any manipulation of prices.

Example:
```go
import (
  "github.com/shopspring/decimal"
)

book, err := bita.getBook("BTC_USDT", 1)
if err != nil {
    println(err.Error())  
}

lastPrice, err := decimal.NewFromString(book.Bids[0].Price)
if err != nil {
    println(err.Error())  
}

order := bita.Order{
  Price: lastPrice.Add(decimal.NewFromFloat(1.00)).String(),
  Size: "2.00",
  Side: "buy",
  ProductId: "BTC-USD",
}

savedOrder, err := client.CreateOrder(&order)
if err != nil {
  println(err.Error())
}

println(savedOrder.Id)
```
