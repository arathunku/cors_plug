CorsPlug
========
[![Build Status](https://travis-ci.org/mschae/cors_plug.svg)](https://travis-ci.org/mschae/cors_plug)

An [Elixir Plug](http://github.com/elixir-lang/plug) to add [CORS](http://www.w3.org/TR/cors/).

## Usage

1. Add this plug to your `mix.exs` dependencies:

```elixir
def deps do
  # ...
  {:cors_plug, "~> 0.1.3"},
  #...
end
```

When used together with the awesomeness that's the [Phoenix Framework](http://www.phoenixframework.org/)
please note that putting the CORSPlug in a pipeline won't work as they are only invoked for
matched routes.

I therefore recommend to put it in `lib/<you_app>/endpoint.ex`:

```elixir
defmodule YourApp.Endpoint do
  use Phoenix.Enpoint, otp_app: :your_app

  # ...
  plug CORSPlug

  plug :router, YourApp.Router
end
```

Alternatively you can add options routes, as suggested by @leighhalliday

```elixir
scope "/api", PhoenixApp do
  pipe_through :api

  resources "/articles", ArticleController
  options   "/articles", ArticleController, :options
  options   "/articles/:id", ArticleController, :options
end
```

## Configuration

This plug will return the following headers:

On preflight (`OPTIONS`) requests:

* Access-Control-Allow-Origin
* Access-Control-Allow-Credentials
* Access-Control-Max-Age
* Access-Control-Allow-Headers
* Access-Control-Allow-Methods

On `GET`, `POST`, ... requests:

* Access-Control-Allow-Origin
* Access-Control-Allow-Credentials

You can configure the value of these headers as follows:

```elixir
plug CORSPlug, [origin: "example.com"]
```

Please find the list of current defaults in [cors_plug.ex](lib/cors_plug.ex#L5:L13).

## License

Copyright 2014 Michael Schaefermeyer

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
