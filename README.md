# Bootstrap for Sass

[![Build Status](https://secure.travis-ci.org/thomas-mcdonald/bootstrap-sass.png?branch=master)](http://travis-ci.org/thomas-mcdonald/bootstrap-sass) [![Code Climate](https://codeclimate.com/github/thomas-mcdonald/bootstrap-sass.png)](https://codeclimate.com/github/thomas-mcdonald/bootstrap-sass)

`bootstrap-sass` is a Sass-powered version of [Bootstrap](http://github.com/twbs/bootstrap), ready to drop right into your Sass powered applications.

## Installation

Please see the appropriate guide for your environment of choice:

### a. Rails

`bootstrap-sass` is easy to drop into Rails with the asset pipeline.

In your Gemfile you need to add the `bootstrap-sass` gem, and ensure that the `sass-rails` gem is present - it is added to new Rails applications by default.

```ruby
gem 'sass-rails', '>= 3.2' # sass-rails needs to be higher than 3.2
gem 'bootstrap-sass', '~> 3.0.2.0'
```

`bundle install` and restart your server to make the files available through the pipeline.

### b. Compass (no Rails)

Install the gem
```sh
gem install bootstrap-sass
```

If you have an existing Compass project:

```ruby
# config.rb:
require 'bootstrap-sass'
```

```console
bundle exec compass install bootstrap
```

If you are creating a new Compass project, you can generate it with bootstrap-sass support:

```console
bundle exec compass create my-new-project -r bootstrap-sass --using bootstrap
```

This will create a new Compass project with the following files in it:

* [_variables.scss](/templates/project/_variables.scss.erb) - all of bootstrap variables (override them here).
* [styles.scss](/templates/project/styles.scss) - main project SCSS file, import `variables` and `bootstrap`.


### c. Sass-only (no Compass, nor Rails)

Require the gem, and load paths and Sass helpers will be configured automatically:

```ruby
require 'bootstrap-sass'
```

When using outside ruby (e.g. as a bower package), disable ruby asset lookup helper:

```sass
$bootstrap-sass-asset-helper: false
```


#### JS and fonts

If you are using Rails or Sprockets, see Usage.

If none of Rails/Sprockets/Compass were detected the fonts will be referenced as:

```sass
"#{$icon-font-path}/#{$icon-font-name}.eot"
```

`$icon-font-path` defaults to `bootstrap/`.

When not using an asset pipeline, you have to copy fonts and javascripts from the gem.

```bash
mkdir public/fonts
cp -r $(bundle show bootstrap-sass)/vendor/assets/fonts/ public/fonts/
mkdir public/javascripts
cp -r $(bundle show bootstrap-sass)/vendor/assets/javascripts/ public/javascripts/
```

In ruby you can get the assets' location in the filesystem like this:

```ruby
Bootstrap.stylesheets_path
Bootstrap.fonts_path
Bootstrap.javascripts_path
```

## Usage

### Sass

Import Bootstrap into a Sass file (for example, `application.css.scss`) to get all of Bootstrap's styles, mixins and variables!
We recommend against using `//= require` directives, since none of your other stylesheets will be [able to access][antirequire] the Bootstrap mixins or variables.

```scss
@import "bootstrap";
```

You can also include optional bootstrap theme:

```scss
@import "bootstrap/theme";
```

The full list of bootstrap variables can be found [here](http://getbootstrap.com/customize/#less-variables). You can override these by simply redefining the variable before the `@import` directive.
For example:
```scss
$navbar-default-bg: #312312;
$light-orange: #ff8c00;
$navbar-default-color: $light-orange;

@import "bootstrap";
```

You can also import components explicitly. To start with a full list of modules copy this file from the gem:

```bash
cp $(bundle show bootstrap-sass)/vendor/assets/stylesheets/bootstrap/bootstrap.scss \
   vendor/assets/stylesheets/bootstrap-custom.scss
```

In your `application.sass`, replace `@import 'bootstrap'` with:

```scss
  @import 'bootstrap-custom';
```

Comment out any modules you don't need from `bootstrap-custom`.

### Javascript

We have a helper that includes all Bootstrap javascripts. If you use Rails (or Sprockets separately), 
put this in your Javascript manifest (usually in `application.js`) to load the files in the [correct order](/vendor/assets/javascripts/bootstrap.js):

```js
// Loads all Bootstrap javascripts
//= require bootstrap
```

You can also load individual modules, provided you also require any dependencies. You can check dependencies in the [Bootstrap JS documentation][jsdocs].

```js
//= require bootstrap/scrollspy
//= require bootstrap/modal
//= require bootstrap/dropdown
```

---

## Development and Contributing

If you'd like to help with the development of bootstrap-sass itself, read this section.

### Upstream Converter

Keeping bootstrap-sass in sync with upstream changes from Bootstrap used to be an error prone and time consuming manual process. With Bootstrap 3 we have introduced a converter that automates this.

**Note: if you're just looking to *use* Bootstrap 3, see the [installation](#installation) section above.**

Upstream changes to the Bootstrap project can now be pulled in using the `convert` rake task.

Here's an example run that would pull down the master branch from the main [twbs/bootstrap](https://github.com/twbs/bootstrap) repo:
    
    rake convert
    
This will convert the latest LESS to SASS and update to the latest JS.
To convert a specific branch or version, pass the branch name or the commit hash as the first task argument:
    
    rake convert[e8a1df5f060bf7e6631554648e0abde150aedbe4]

The latest converter script is located [here][converter] and does the following:

* Converts upstream bootstrap LESS files to its matching SCSS file.
* Copies all upstream JavaScript into `vendor/assets/javascripts/bootstrap`
* Generates a javascript manifest at `vendor/assets/javascripts/bootstrap.js`
* Copies all upstream font files into `vendor/assets/fonts/bootstrap`
* Sets `Bootstrap::BOOTSTRAP_SHA` in [version.rb][version] to the branch sha.

This converter fully converts original LESS to SCSS. Conversion is automatic but requires instructions for certain transformations (see converter output).
Please submit GitHub issues tagged with `conversion`.

## Credits

bootstrap-sass has a number of major contributors:

<!-- feel free to make these link wherever you wish -->
* [Thomas McDonald](https://twitter.com/thomasmcdonald_)
* [Tristan Harward](http://www.trisweb.com)
* Peter Gumeson
* [Gleb Mazovetskiy](https://github.com/glebm)

and a [significant number of other contributors][contrib].

## You're in good company
bootstrap-sass is used to build some awesome projects all over the web, including
[Diaspora](http://diasporaproject.org/), [rails_admin](https://github.com/sferik/rails_admin),
Michael Hartl's [Rails Tutorial](http://railstutorial.org/), [gitlabhq](http://gitlabhq.com/) and
[kandan](http://kandanapp.com/).

[converter]: https://github.com/thomas-mcdonald/bootstrap-sass/blob/3/tasks/converter.rb
[version]: https://github.com/thomas-mcdonald/bootstrap-sass/blob/3/lib/bootstrap-sass/version.rb
[contrib]: https://github.com/thomas-mcdonald/bootstrap-sass/graphs/contributors
[antirequire]: https://github.com/thomas-mcdonald/bootstrap-sass/issues/79#issuecomment-4428595
[jsdocs]: http://getbootstrap.com/javascript/#transitions
