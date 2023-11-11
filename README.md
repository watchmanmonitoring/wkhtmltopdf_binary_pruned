# Installation and usage

Install in your Gemfile instead of `gem "wkhtmltopdf_binary"`, referencing this repository:

```ruby
gem "wkhtmltopdf-binary", github: "watchmanmonitoring/wkhtmltopdf_binary_pruned"
```

In Ubuntu 22 and macOS environments, this is all you need to do. This gem installs a binary stub that tries to determine which wkhtmltopdf binary will work on your system, and point to the packaged binary that most closely matches.

In any other environment, invoking this binary will result in an error, saying the needed permissions are not available.
This is because `wkhtmltopdf-binary` ships with gzipped binaries for many platforms, and then picks the appropriate one
upon first use and unzips it into the same directory. So if your ruby gem binaries are installed here:

    /usr/lib/ruby/versions/2.6/bin/

The various wkhtmltopdf-binaries will be installed here:

    /usr/lib/ruby/versions/2.6/lib/ruby/gems/2.6.0/gems/wkhtmltopdf-binary-0.12.5.1/bin/

Giving write access whatever user is running your program (e.g. web server, background job processor),
e.g. your own personal user in a dev environment, will fix the problem. After the binary is uncompressed, write access can be revoked again if desired.

    chmod -R 777 /usr/lib/ruby/versions/2.6/lib/ruby/gems/2.6.0/gems/wkhtmltopdf-binary-0.12.5.1/bin/

# Gem Development

## Extracting binaries

This where this "gem" differs from https://github.com/zakird/wkhtmltopdf_binary_gem/  

This gem is designed to be small, including the minimal number of binaries to get the job done.  

Why? because the primary gem has grown large over time (source: https://github.com/zakird/wkhtmltopdf_binary_gem/issues/55)  

## Updating this gem/repo

The goal of this repo is to be small. If additional binaries are needed, they can be added as normal.  

However, as the need for old gems passes, the repo should be rebuilt from scratch lest this repo grows as well.

For example, this repo initially included an intel-only binary for macOS. Adding an arm64 binary would be appropriate, as it would be used concurrently by intel and Apple silicon computers. Removing the intel build is a time when the repo should be zero'd and re-create with fresh commits.  

An alternative could be to publish to rubygems.org, however doing so is more lift then periodically maintaining this gem (as `wkhtmltopdf` isn't prone to frequent updates.)
