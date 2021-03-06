# Autodoc
Generate documentation from your rack application & request-spec.

## Installation
```ruby
gem "autodoc", group: :test
```

## Usage
Run rspec with AUTODOC=1 to generate documents for your request-specs tagged with `:autodoc`.  
example: [doc/recipes.md](https://github.com/r7kamura/autodoc/blob/master/spec/dummy/doc/recipes.md), [doc/toc.md](https://github.com/r7kamura/autodoc/blob/master/spec/dummy/doc/toc.md)

```sh
# shell-command
AUTODOC=1 rspec
```

### Example for any Rack application with rack-test
```ruby
# spec/requests/entries_spec.rb
describe "Entries" do
  include Rack::Test::Methods

  let(:app) do
    MyRackApplication
  end

  describe "GET /entries", autodoc: true do
    get "/entries"
    last_response.status.should == 200
  end
end
```

### Example for Rails application with rspec-rails
```ruby
# spec/requests/recipes_spec.rb
describe "Recipes" do
  describe "POST /recipes", autodoc: true do
    it "creates a new recipe" do
      post "/recipes", name: "alice", type: 1
      response.status.should == 201
    end
  end
end
```

### Configuration
You can configure `Autodoc.configuration` to change its behavior:

* path - [String] location to put files (default: ./doc)
* suppressed_request_header - [Strings] filtered request header keys
* suppressed_response_header - [Strings] filtered response header keys
* template - [String] ERB template for each document (default: [document.md.erb](https://github.com/r7kamura/autodoc/blob/master/lib/autodoc/templates/document.md.erb))
* toc_template - [String] ERB template for ToC (default: [toc.md.erb](https://github.com/r7kamura/autodoc/blob/master/lib/autodoc/templates/toc.md.erb))
* toc - [Boolean] whether to generate toc.md (default: false)

```ruby
# example
Autodoc.configuration.path = "doc/api"
Autodoc.configuration.toc = true
Autodoc.configuration.template = File.read(File.expand_path("../autodoc/templates/document.md.erb", __FILE__))
```
