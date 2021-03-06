# Mithril support for Opal

## Description
Ruby in the front !

[Opal](http://opalrb.com/) wrapper for [mithril js](https://mithril.js.org/) javascript library.

forked from : [opal-virtual-dom](https://github.com/fazibear/opal-virtual-dom)

## Usage

Server side (config.ru, Rakefile, Rails, Sinatra, etc.)

```ruby
require 'opal-mithril'
```

Browser side

```html
<!doctype html>
<html lang="en">
  <head>
    <title>Opal mithril test</title>

    <link rel="stylesheet" href="https://unpkg.com/tachyons@4.9.1/css/tachyons.min.css">
  </head>

  <body id="app">
    <script src="/js/test.js"></script>
  </body>
</html>
```

```ruby
require 'opal'
require 'browser'     # not required
require 'mithril'

class SampleList
  include Mithril

  def initialize(elements = [])
    @elements = %w(one two three)
  end

  def create_hook(vnode)
    puts "I'm created"
    `console.log(#{vnode})`
  end

  def on_reverse(event)
    @elements.reverse!
  end

  def view
    # You can use chained methods
    # to add class to elements
    # use bang method to define element id
    # to bind event use onXXX(:callback_name) or onXXX(method(:callback_name))

    # <div id="app">
    div.app! do
      # <p id="example">
      p.example! do
        # <ul class="simple-list">
        ul.simple_list.oncreate(:create_hook) do
          @elements.each { |string| li.text(string) }
        end
      end

      # I recommend the use of tachyons css to embed the css in ruby :
      div.grow.br_pill.ph3.pv2.mb2.dib.white.bg_hot_pink.onclick(:on_reverse) do
        text("click on me to reverse the list !")
      end
    end
  end
end

$document.ready do
  Mithril.mount($document.body, SampleList.new())
end
```
