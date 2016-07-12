# go-fastbqstreamer

A fast BigQuery streaming client for Go (Golang).

I was able to get >500% increase in throughput with this client compared to everything else I tried, and that limit was due to BigQuery's API crapping out rather than my small worker.

This is because other BigQuery clients I looked at take in map[string]interface{} for row data, whereas I already had marshal'd JSON in bytes and couldn't afford
to unmarshal that to pass into the client lib's API and then have the client lib marshal that again.

So this lib is great to put on a worker that consuming from Kafka and inserting to BigQuery. That's what I'm using it for.

## Example

``` go
service, err := New(&Options{
	ProjectID:     os.Getenv("PROJECT_ID"),
	Email:         os.Getenv("EMAIL"),
	PEM:           []byte(os.Getenv("PEM")),
})
log.Check(err)

service.Input() <- row{
    DatasetID: "yout-dataset,"
    TableID: "your-table",
    Data: yourJSON,
}
```

[GoDocs](https://godoc.org/github.com/travisjeffery/go-fastbqstreamer). 

## Author

Travis Jeffery

- [Twitter](http://twitter.com/travisjeffery)
- [Medium](http://medium.com/@travisjeffery)
- [Homepage](http://travisjeffery.com)

## License

MIT
