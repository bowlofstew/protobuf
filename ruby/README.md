This directory contains the Ruby extension that implements Protocol Buffers
functionality in Ruby.

The Ruby extension makes use of generated Ruby code that defines message and
enum types in a Ruby DSL. You may write definitions in this DSL directly, but
we recommend using protoc's Ruby generation support with .proto files. The
build process in this directory only installs the extension; you need to
install protoc as well to have Ruby code generation functionality.

Installation
------------

To build this Ruby extension, you will need:

* Rake
* Bundler
* Ruby development headers
* a C compiler
* the upb submodule

First, install the required Ruby gems:

    $ sudo gem install bundler rake rake-compiler rspec rubygems-tasks

Optionally, run the tests locally:

    $ rake test

Then build and install the Gem:

    $ rake gem
    $ gem install pkg/protobuf-$VERSION.gem

This gem includes the upb parsing and serialization library as a single-file
amalgamation. It is up-to-date with upb git commit
`535bc2fe2f2b467f59347ffc9449e11e47791257`.

Testing and Releasing
---------------------

Before release, we test with the following Ruby versions: 2.2.0, 2.1.3, 2.0.0,
1.9.3.

This is most convenient with [RVM](https://rvm.io/). To set up, first install
RVM, then:

    $ rvm install ruby-2.2
    $ rvm install ruby-2.1
    $ # ...

Then the release testing procedure is:

    $ cd protobuf/ruby/
    $ rvm use ruby-2.2
    $ gem install rake rake-compiler rubygems-tasks
    $ rake test
    $ rvm use ruby-2.1
    $ gem install ...
    $ rake test
    $ # ...
    $ # if all tests passed:
    $ rake gem
    $ gem push pkg/google-protobuf...gem

Also, be sure that one's `umask` is correct before cloning the repository and
building the Gem. The Gem preserves file permissions, and the umask on some
systems is restrictive (resulting in `0640` rather than `0644` file permissions
by default). This will break the resulting Gem install. If in doubt, `umask
0022` is sufficient.
