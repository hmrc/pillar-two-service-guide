# Post and Parcels (Moving Parcels to Northern Ireland) Service Guide

## Overview

This repository contains the source files that are used to generate the Pillar 2 Service Guide for how to use the SDES File Upload API
on the HMRC Developer Hub.

## Editing Service Guide pages

Template files are located in `./source/documentation`. All pages are written in [Markdown](https://en.wikipedia.org/wiki/Markdown).

To add new pages simply copy and paste one of the existing pages, it will automatically appear in the menu.

`example-page.html.md.erb` provides examples and best practices styles.

## Previewing

#### Option 1 - Using Docker (recommended)

Requirements:
* [Docker](https://www.docker.com/)

To live preview:
```
./batect preview
```
The local URL and port where the files can be previewed will be output, this is normally http://localhost:4567.

NB The first time this is run it builds the Docker image and installs dependencies so may take 5 mins.
Subsequent runs will be much quicker.

#### Option 2 - Local install (Not recommended)

Requirements:
* [Ruby Version Manager][rbenv]
* [Node Version Manager][nodenv]

After installing ruby, change the gem sources to use HMRC's artefact repository:
```
gem sources -r https://rubygems.org/
gem sources -a https://artefacts.tax.service.gov.uk/artifactory/api/gems/gems/
````

To live preview:
```
bundle install
bundle exec middleman serve
```
The local URL and port where the files can be previewed will be output, this is normally http://localhost:4567.

## Running the Scala Application

Requirements:
* Scala/sbt

### Build the HTML files
```
./batect build
```
### Run the Scala Application
```
sbt run
```

The local URL and port where the files can be previewed will be output, this is normally http://localhost:9000.

## Building

Create a [build job](https://github.com/hmrc/build-jobs) like:
```
new SbtMicroserviceJobBuilder(TEAM, 'service-guide-skeleton')
        .withTests("test")
        .withNodeJs(version = '16.11.0')
        .build(this as DslFactory)
```

NB the version of Ruby is automatically picked up from `.ruby-version`. But the Node version isn't! Make sure that the 
version you specify on the build job is the same as what is in `.node-version`.

## FAQ

### I already have a Service Guide how do I update to this version

The easiest and safest option would be:

1. Clone this git repository into a new directory.
2. Follow the _Getting started_ section above, copying the settings from your original project.
3. Copy the `./source` directory from your original project.
4. Copy all files from the new directory to overwrite your original project. (except the `.git` directory)
5. Check the build job has the correct Node version specified.
6. Commit and push changes from your original project.

### How do I update the Ruby Gems
To update the Ruby Gems to the latest versions, run
```
./batect update
```
This will update the `Gemfile.lock`

In some cases, the [latest release](https://github.com/alphagov/tech-docs-gem/releases) of the gem might not be fetched.
If this happens, edit the Gemfile to specify the latest version explicitly, for example
```
gem 'govuk_tech_docs', '4.4.0'
```
and run the update command again. Once updated, remove the explicit version and run the update once more.

### How do I change the Ruby version
Edit `.ruby-version` with the required version of Ruby.


### How do I change the Node version
Edit `.node-version` with the required version of Node.

[tdt]: https://github.com/alphagov/tech-docs-template
[rbenv]: https://github.com/rbenv/rbenv
[nodenv]: https://github.com/nodenv/nodenv

## License
This code is open source software licensed under the [Apache 2.0 License]("http://www.apache.org/licenses/LICENSE-2.0.html").
