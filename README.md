## jekyll_pages_api_search Plugin

This Ruby gem adds a [lunr.js](http://lunrjs.com) search index to a
[Jekyll](http://jekyllrb.com/)-based web site.

The search index is generated and compressed automatically via `jekyll build`
or `jekyll serve`. The supporting JavaScript code is optimized, compressed,
and loads asynchronously. These features ensure that the preparation of the
search index does not introduce rendering latency in the browser.

### Installation

Add this line to your Jekyll project's `Gemfile`:

```ruby
group :jekyll_plugins do
  gem 'jekyll_pages_api_search'
end
```

Then add the following to the project's `_config.yml` file:

```yaml
# Configuration for jekyll_pages_api_search plugin gem.
jekyll_pages_api_search:
  # Uncomment this to speed up site generation while developing.
  #skip_index: true

  # Each member of `index_fields` should correspond to a field generated by
  # the jekyll_pages_api. It can hold an optional `boost` member as a signal
  # to Lunr.js to weight the field more highly (default is 1).
  index_fields:
    title:
      boost: 10
    tags:
      boost: 10
    url:
      boost: 5
    body:
```

Now running `jekyll build` or `jekyll serve` will produce `search-index.json`
and `search-index.json.gz` files in the `_site` directory (or other output
directory, as configured).

If you're running [Nginx](http://nginx.org), you may want to use the
[`gzip_static on`
directive](http://nginx.org/en/docs/http/ngx_http_gzip_static_module.html)
to take advantage of the gzipped versions of the search index and supporting
JavaScript.

### Usage

To add the index to your pages, insert the following tags in your `_layouts`
and `_includes` files as you see fit:

- `{% jekyll_pages_api_search_interface %}`: inserts the HTML for the search
  box and search results
- `{% jekyll_pages_api_search_load %}`: inserts the `<script>` tags to load
  the search code asynchronously; the search code will then load
  `search-index.json` asynchronously

You can also add `@import "jekyll_pages_api_search";` to one of your [Sass
assets](http://jekyllrb.com/docs/assets/) to use the default interface style.

### Customization

If you prefer to craft your own versions of these tags and styles, you can
capture the output of these tags and the Sass `@import` statement, then create
new tags or included files based on this output, careful not to change
anything that causes the interaction between these components to fail.

Alternately, you can inspect the code of this Gem (all paths relative to
`lib/jekyll_pages_api_search/`):

- `{% jekyll_pages_api_search_interface %}`: includes `search.html`
- `{% jekyll_pages_api_search_load %}`: generated by the `LoadSearchTag` class
  from `tags.rb`
- `@import "jekyll_pages_api_search";`: includes
  `sass/jekyll_pages_api_search.scss`

### Under the hood

This plugin depends on [jQuery](https://jquery.com/),
[AngularJS](https://angularjs.org/), [RequireJS](http://requirejs.org/),
[angularAMD](https://github.com/marcoslin/angularAMD), and
[angular-livesearch](https://github.com/mauriciogentile/angular-livesearch).

All of these components are bundled together with `assets/js/search.js` into
`assets/js/search-main.min.js` using the RequireJS optimizer. If you already
use some of these components, you may wish to fetch the `assets/js/search.js`
file directly from this repository and use `assets/js/search-main.js` as a
guide to update or create your own RequireJS configuration.

### Developing

Building the gem requires [Node.js](https://nodejs.org/) and several Node
packages. The `Rakefile` will prompt you to install Node.js and any packages
that are missing from your system when running `bundle exec rake build`.

* Run `bundle` to install any necessary gems.
* Run `bundle exec rake -T` to get a list of build commands and descriptions.
* Run `bundle exec rake update_js_components` download and install the latest
  JavaScript components listed in `bower.json`.
* Run `bundle exec rake test` to run the tests.
* Run `bundle exec rake build` to ensure the entire gem can build.
* Commit an update to bump the version number of
  `lib/jekyll_pages_api_search/version.rb` before running `bundle exec rake
  release`.

While developing this gem, add this to the Gemfile of any project using the
gem to try out your changes (presuming the project's working directory is a
sibling of the gem's working directory):

```ruby
group :jekyll_plugins do
  gem 'jekyll_pages_api_search', :path => '../jekyll_pages_api_search'
end
```

### Contributing

1. Fork the repo (or just clone it if you're an 18F team member)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

Feel free to ping [@mbland](https://github.com/mbland) with any questions you
may have, especially if the current documentation should've addressed your
needs, but didn't.

### Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in
[CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright
> and related rights in the work worldwide are waived through the
> [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication.
> By submitting a pull request, you are agreeing to comply with this waiver of
> copyright interest.
