# Snapi

Snapi is a modular API functionality definition tool.

## Installation

This hasn't been put on Rubygems yet so you still have to build the gem
manually for now. 

```sh
git clone git@github.com:pwnieexpress/snapi.git
cd snapi
gem build snapi.gemspec
gem install snapi
```
This has only been used on `1.9.3` but is should run fine on `1.9+` and `2.x+`

## Usage

Usually code just so here is a sample implementation

```ruby
require 'snapi'
require 'json'

class Scanner 
  include Snapi::BasicCapability
  function :scan do |fn|
    fn.argument :target do |arg|
      arg.required true
      arg.list true
      arg.type :string
      arg.format :address
    end
    fn.argument :port do |arg|
      arg.type :string
    end
    fn.return :structured
  end
end
```

The `Scanner` class in this example would respond to a `functions` message as
follows. 

```ruby
irb> puts JSON.pretty_generate Nmap.functions
#=> nil

{
  "scan": {
    "return_type": "structured",
    "target": {
      "default_value": null,
      "format": "address",
      "required": true,
      "list": false,
      "type": "string",
      "values": null
    },
    "port": {
      "default_value": null,
      "format": null,
      "required": false,
      "list": null,
      "type": "string",
      "values": null
    }
  }
}
```

*Note:* this was converted to JSON for readability

## Project Goals & Name

I literally woke up in the middle of the night with the idea for a modular
Sinatra Api plugin system and wrote down the name "Snapi" which is a
contraction of "Sinatra Api". It might better be spelled "SnApi" but I prefer
"Snapi" because it snake cases to `snapi`, not `sn_api`. I like the concept of
snap-in modularity.

As of now the only real behavior is that of declaring functions and arguments
for a class as a way of doing meta-programming funcationality / arrity
disclosure.

The ultimate goal being for an API to be able to define itself dynamically and
dislose that functionality to remote systems and even do things like:

* Dynamic Form Generation
* API generation (TODO `snapi generate hosts plugin:crud`)
* Self Documentation
