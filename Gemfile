source "https://rubygems.org"

# Gem do Jekyll
gem "jekyll", "~> 4.3.0"

# Tema Minima
gem "minima", "~> 2.5"

# Plugins do Jekyll
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-seo-tag", "~> 2.8"
  gem "jekyll-sitemap", "~> 1.4"
  gem "jekyll-paginate", "~> 1.1"
end

# Windows e JRuby não incluem arquivos zoneinfo
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster para watching directories on Windows
gem "wdm", "~> 0.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

# GitHub Pages gem (opcional, descomente se quiser usar localmente)
# gem "github-pages", group: :jekyll_plugins
