# Kramdown Prismic ![build](https://img.shields.io/github/actions/workflow/status/stormz/kramdown-prismic/ci.yml?branch=master) [![gem](https://img.shields.io/gem/v/kramdown-prismic)](https://rubygems.org/gems/kramdown-prismic)

A [Kramdown][] parser and converter to convert documents into [prismic][] rich text format and the other way around.

A useful usage is to convert markdown documents to prismic for [import purpose][prismic-import]. Then you can even convert prismic format back to markdown.

## Status

The converter part (kramdown to prismic) is working and fairly complete. See *Difference between markdown and rich text* below to know more about limitations.

The parser part is quite new and not feature complete.

## Install

```ruby
gem 'kramdown-prismic', '~> 0.1'
```

## Usage

### With executables

*markdown2prismic*:

    markdown2prismic $'# My Title\n\nHello world'

*html2prismic*:

    html2prismic '<h1>My Title</h1><p>Hello world</p>'

*prismic2markdown*:

    prismic2markdown '[{"type":"heading1","content":{"text":"My Title","spans":[]}},{"type":"paragraph","content":{"text":"Test","spans":[]}}]'

### As a library

**Convert kramdown documents to Prismic**

```ruby
require 'kramdown-prismic'

kramdown = '# Hello world'
Kramdown::Document.new(kramdown).to_prismic
```

**Convert markdown documents to Prismic**

```ruby
require 'kramdown-prismic'

markdown = '# Hello world'
Kramdown::Document.new(markdown, input: :markdown).to_prismic
```

**Convert HTML documents to Prismic**

```ruby
require 'kramdown-prismic'

html = '<h1>Hello world</h1>'
Kramdown::Document.new(html, input: :html).to_prismic
```

**Convert Prismic to markdown**

```ruby
require 'kramdown-prismic'
prismic = [
  {
    type: "heading1",
    content: {
      text: "This is the document title",
      spans: []
    }
  }
]
Kramdown::Document.new(prismic, input: :prismic).to_kramdown
```

You can also convert to or from others formats supported by Kramdown. See [kramdown documentation](https://kramdown.gettalong.org/documentation.html).

### Lookup for warnings

If there is some elements that cannot be converted (see the status table), a warning will be emitted.

For instance, html elements in the markdown is not supported:

```ruby
require 'kramdown-prismic'

markdown = '<h1>Hello world</h1>'
result = Kramdown::Document.new(markdown, input: :markdown)
result.to_prismic
p result.warnings
```

### Difference between markdown and rich text

Some elements cannot be converted, due to some Prismic limitations. The table below explain the difference and limitations of the current converter:

| Markdown         | Prismic                    |
|------------------|----------------------------|
| blockquote       | converted to preformatted  |
| hr               | nothing                    |
| img              | moved to the top level     |
| nested list      | moved to the top level     |
| entity           | converted to unicode       |
| typographic_sym  | converted to unicode       |
| smart_quote      | converted to unicode       |
| dl               | not supported              |
| dt               | not supported              |
| dd               | not supported              |
| table            | not supported              |
| thead            | not supported              |
| tobdy            | not supported              |
| tfoot            | not supported              |
| tr               | not supported              |
| td               | not supported              |
| math             | not supported              |
| footnote         | not supported              |
| abbreviation     | not supported              |
| html_element     | not supported              |
| xml_comment      | not supported              |
| xml_pi           | not supported              |
| comment          | not supported              |
| raw              | not supported              |

## Develop

Install dependencies:

    bundle install

Run tests:

    bundle exec rake test

## License

MIT

[Kramdown]: https://kramdown.gettalong.org/
[prismic]: https://prismic.io/
[prismic-import]: https://prismic.io/docs/core-concepts/how-to-import-content
