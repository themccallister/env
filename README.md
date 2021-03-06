# env [![Build Status](https://travis-ci.org/jasonmccallister/env.svg?branch=master)](https://travis-ci.org/jasonmccallister/env) [![GoDoc](https://godoc.org/github.com/jasonmccallister/env?status.svg)](https://godoc.org/github.com/jasonmccallister/env)
A Go package to make application environments a little more sane by allowing defaults to be passed when asking for environment variables.

## TL;DR

Instead of this:

    value, empty := os.LookupEnv("ENV_VAR")
    if empty == false {
        // set the value six other ways...
    }

You can write this instead:

    e := env.Set{}
    value := e.GetOr("ENV_VAR", "default_var")
    // do something with the value!

## Introduction

Golang has a lot of awesome ways to get environment variables from the operating systems. However, one of the languages that I used often (Laravel) has some excellent helpers for grabbing or setting default environment variables. This package is a small helper to reduce the amount of code I write in other applications. Overtime this package will add more options, such as `AppMode`, to set some common practices that I see in the applications I build.

Pull requests are welcome and if you would like to have a discussion please feel free to open an issue!

## Installation

Install env using the command `go get jasonmccallister/env`.

## Common Helpers

There are a few helper methods on Env that are used often, these are `AppMode` and `AppKey`.

`AppMode` is a string that defines the applications mode, such as production, development or staging.

`AppKey` is a random string that can be used for encryption within the app.

## Documentation

You can create a new env instance in your app like so:

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{}
        // more boilerplate
    }

### Getting the App "Mode"

By default, `env` sets an `AppMode` which allows you to quickly define the application mode (e.g production, staging or development). You can access the `AppMode` like so:

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{}
        if e.AppMode == "development" {
            // do something only for development
        }
    }

### Setting a Default Mode for the App

By default, `AppMode` will look for an OS environment variable `APP_MODE`. If `APP_MODE` is not set, it will return a string "development".

However, if you wish to change the default `AppMode`, when creating the env, you can set a field on the Env struct `DefaultMode`. If this is set, calling `AppMode` will return the `DefaultMode` instead of "development".

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{DefaultMode: "production"}
        // the rest of your application
    }

### Getting the App Key

Similar to `AppMode` By default Env sets an `AppKey` which allows you to quickly access the applications key for encryption purposes:

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{}
        k := e.AppKey()
        // do something awesome with the key
    }

You can also set a default, using the following code:

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{DefaultKey: "D035ABB6AC62A811D462D9BD572396E5C52E383699737A9D4B022E3C3B2618CF"}
        // the rest of your application
    }

### Getting an Environment Variable Specifying a Default (Fallback)

Env has a helper to get an environment variable or fall back to a default you specify. This helper is really just a wrapper around the `env.LookupEnv` to remove some extra code in your application.

Lets say you need to get the applications URL but want to set a default at the same time, you would write something like this:

    package main

    import "github.com/jasonmccallister/env"

    func main() {
        e := env.Set{DefaultMode: "production"}
        url := e.GetOr("APP_URL", "https://mccallister.io")
        // the rest of your application
    }
