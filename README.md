# prettier-remote

`prettier-remote` contains the infrastructure for a server that will run `prettier` formatting on code over an HTTP connection. The purpose of this is to be able to support formatting from within the browser even though some plugins require server-side resources (e.g., the ruby plugin needs a ruby runtime).

It is set up as a series of docker containers, each running a different runtime that is necessary for each plugin, in addition to one "api" runtime which handles passing off requests to the correct runtime. All in all, you can think of this service as a singular HTTP endpoint that will handle formatting based on the POST request body.

The main "api" server has just one POST endpoint: the root (i.e., "/"). It accepts a JSON body that contains three keys:

* `code` - the code to be formatted
* `lang` - the language of the source code
* `opts` - an optional object that may contain any valid prettier options, as well as any options that are valid for the language's plugin

The successful response will have an HTTP status of 201 and will have a JSON body containing the same three keys, with the `code` value being formatted appropriately. A failed response will have an HTTP status of 422 and will have a JSON body containing an `error` key.

## Getting started

To run the service locally, you'll need to have both [`docker`](https://docs.docker.com/install/) and [`docker-compose`](https://docs.docker.com/compose/install/) installed. You can then begin running the service via:

```bash
docker-compose up
```

This will build the images appropriately and begin running the service on `http://localhost:8080`. You can then test it out locally using `curl` to hit the service, as in:

```bash
curl -H "Content-Type: application/json" -d '{"code": "foo", "lang": "ruby"}' http://localhost:8080
```

You'll receive back the formatted code within the JSON response.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/kddeisz/prettier-remote.

## License

The package is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
