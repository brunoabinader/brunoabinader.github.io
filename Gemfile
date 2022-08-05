source "https://rubygems.org"

# User dependencies
gem 'jemoji'

# ----
# Github Pages dependencies

# Help ensure the proper Jekyll version is running.
gem "jekyll", "~> 3.9.0"

# Default theme for new Jekyll sites.
gem 'minima', "~> 2.0"

group :jekyll_plugins do
  gem "github-pages", "~> 227"
  gem "jekyll-feed", "~> 0.6"
  gem "jekyll-readme-index"
  gem 'jekyll-coffeescript'
  gem 'jekyll-default-layout'
  gem 'jekyll-gist'
  gem 'jekyll-github-metadata'
  gem 'jekyll-optional-front-matter'
  gem 'jekyll-paginate'
  gem 'jekyll-relative-links'
  gem 'jekyll-titles-from-headings'
end

# We're using Ruby 3.0 and Jekyll 4.2.x or older.
gem 'webrick'

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0", :platforms => [:mingw, :x64_mingw, :mswin]

# kramdown v2 ships without the gfm parser by default. If you're using
# kramdown v1, comment out this line.
gem "kramdown-parser-gfm"
