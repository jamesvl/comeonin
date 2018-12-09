# Comeonin [![Build Status](https://travis-ci.org/riverrun/comeonin.svg?branch=master)](https://travis-ci.org/riverrun/comeonin) [![Hex.pm Version](http://img.shields.io/hexpm/v/comeonin.svg)](https://hex.pm/packages/comeonin) [![Join the chat at https://gitter.im/comeonin/Lobby](https://badges.gitter.im/comeonin/Lobby.svg)](https://gitter.im/comeonin/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Password hashing library for Elixir.

This library is intended to make it very straightforward for developers to check users'
passwords in as secure a manner as possible.

Comeonin supports Argon2, Bcrypt and Pbkdf2 (sha512 and sha256).

## Features

* Comeonin supports the most secure, well-tested, and up-to-date password hashing schemes.
  * Argon2 is the winner of the 2015 Password Hashing Competition.
  * Bcrypt and Pbkdf2 have no known vulnerabilities and have been widely tested for over 15 years.
* It is easy to use.
  * Random salts are generated by default.
  * Each function has sensible, secure defaults.
* It provides excellent documentation.
  * Clear instructions are given on how to use Comeonin.
  * Relevant research is summarized in the documentation and [wiki](https://github.com/riverrun/comeonin/wiki).
    * References are also provided in [this section](https://github.com/riverrun/comeonin/wiki/References) of the wiki.

## Changes in version 4

There were several changes in version 4. These are summarized below:

* Added support for Argon2 (as an **optional** dependency -- argon2_elixir)
* Added higher-level helper functions -- `add_hash` and `check_pass`
* Improved the statistics function in each module -- `report`
* Made all the hashing algorithms **optional** dependencies
* Moved the configuration to the separate dependency libraries
* Removed support for one-time passwords
  * This is now a separate library - [OneTimePassEcto]("https://github.com/riverrun/one_time_pass_ecto")

When upgrading to version 4, you will need to make the following changes:

* Add the algorithm you want to use - Argon2, Bcrypt or Pbkdf2 - to
the `deps` in your mix.exs file (see 2 in `Installation` below).
* Change any entries in the config to now use the dependency
(see 3 in `Installation` below).

For more information about these changes, see
[this page](https://riverrun.github.io/projects/comeonin/2017/09/03/comeonin-v4.html).

## Installation

1. Decide which algorithm to use (see
[Choosing an algorithm](https://github.com/riverrun/comeonin/wiki/Choosing-the-password-hashing-algorithm)
for more information):

* Argon2 - [argon2_elixir](https://github.com/riverrun/argon2_elixir)
* Bcrypt - [bcrypt_elixir](https://github.com/riverrun/bcrypt_elixir)
* Pbkdf2 - [pbkdf2_elixir](https://github.com/riverrun/pbkdf2_elixir)

If you choose Argon2 or Bcrypt, you will need to have a C compiler installed.

Argon2 and Bcrypt version 1.0 also require dirty scheduler support, which
is provided by default in Erlang 20. Bcrypt version 0.12 can be used with
older versions of Erlang.

You do not need to have a C compiler installed to use Pbkdf2.

2. Add `comeonin` and the library (algorithm) you choose to the `deps` section
of your mix.exs file, as in the following example.

```elixir
defp deps do
[
  {:comeonin, "~> 4.1"},
  {:argon2_elixir, "~> 1.3"},
]
end
```

3. Optional: during tests (and tests only), you may want to reduce the number of rounds
so it does not slow down your test suite. If you have a config/test.exs, you should
add (depending on which algorithm you are using):

```elixir
config :argon2_elixir, t_cost: 2, m_cost: 8
config :bcrypt_elixir, log_rounds: 4
config :pbkdf2_elixir, rounds: 1
```

NB: do not use the above values in production.

### Problems / build errors

If you have any problems building Comeonin, see the
[Comeonin wiki](https://github.com/riverrun/comeonin/wiki/Requirements).

## Use

Each module (Comeonin.Argon2, Comeonin.Bcrypt and Comeonin.Pbkdf2) offers the
following functions (the first two are new to version 4):

* `add_hash` - hash a password and return it in a map with the password set to nil
* `check_pass` - check a password by comparing it with the stored hash, which is in a map
* `hashpwsalt` - hash a password, using a randomly generated salt
* `checkpw` - check a password by comparing it with the stored hash
* `dummy_checkpw` - perform a dummy check to make user enumeration more difficult
* `report` - print out a report of the hashing algorithm, to help with configuration

For a lower-level API, you could also use the hashing dependency directly,
without installing Comeonin.

### Deployment

See the [deployment guide](https://github.com/riverrun/comeonin/wiki/Deployment).

### Contributing

There are many ways you can contribute to the development of Comeonin, including:

* reporting issues
* improving documentation
* sharing your experiences with others
* [making a financial contribution](#donations)

## Donations

You can support the ongoing maintenance of this project by
[making donations through Patreon](https://www.patreon.com/riverrun).

Patreon, by default, will bill you on a monthly basis. If you prefer to make a one-off payment,
see [this guide](https://support.patreon.com/hc/en-us/articles/204606215-Can-I-make-a-one-time-payment-).

### Documentation

http://hexdocs.pm/comeonin

### License

BSD. For full details, please read the LICENSE file.
