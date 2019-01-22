# minima

改写自 Jekyll's 默认主题 [minima](https://jekyll.github.io/minima/). 

## 本地依赖环境搭建


```ruby
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
gem install jekyll
gem install bundler
gem install jekyll-feed
gem install jekyll-seo-tag
```

## 访问

本地访问模式：

```ruby
jekyll serve

Server address: http://127.0.0.1:4000/

Server running... press ctrl-c to stop.
```

局域网IP访问（如手机版调试）：

```ruby
jekyll serve -w --host=0.0.0.0

Server address: http://0.0.0.0:4000/

Server running... press ctrl-c to stop.

```

## License

The theme is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
