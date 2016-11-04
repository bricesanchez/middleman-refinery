# Middleman Refinery


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'middleman-refinery'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install middleman-refinery
i
## Configuration

To configure the extension, add the following configuration block to Middleman's config.rb:


Parameter     |i Description
----------    |------------
api_url       | the single endpoint of your content repository
api_token     | Refinery API OAuth2 based access token (optional)
api_path      | Refinery API path (optional)
release       | The content release (optional, defaults to `master`)
link_resolver | A link resolver. Expects a proc with one param, which is an object of type [Refinery::Fragments::DocumentLink](http://www.rubydoc.info/github/refinery/ruby-kit/master/Refinery/Fragments/DocumentLink) (optional)

For instance:

```ruby
activate :refinery do |f|
  f.api_url = 'https://testrepositorymiddleman.refinery.io/api'
  f.api_token = '123'
  f.api_path = '/api/v1'
  f.release = 'master'
  f.link_resolver = ->(link) { binding.pry; "#{link.type.pluralize}/#{link.slug}"}
  f.custom_queries = { test: [Refinery::Predicates::at('document.type', 'product')] }
end
```

## Usage
Run `bundle exec middleman refinery` in your terminal. This will fetch entries for the configured
spaces and content types and put the resulting data in the [local data folder](https://middlemanapp.com/advanced/local-data/) as yaml files.

For each document mask you have, you will get a file named `refinery_{document-name}` under `/data`.
So for instance if you have article and product document masks, you will get 2 files: `refinery_articles`, `refinery_products`.
Each file will contain all your documents of the specified document mask in an array.


In your templates, you get for free helper methods named after your document mask that return the Refinery equivelant object.
For instance for articles, if you run `<%= articles %>` you get back a `Refinery::Document`.

If at any time need access to `Refinery::Ref` you can do it using the `reference` helper.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release` to create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

1. Fork it ( https://github.com/[my-github-username]/middleman-refinery/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
