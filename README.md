[![Travis Build Status](http://img.shields.io/travis/abrom/rocketchat-ruby.svg?style=flat)](https://travis-ci.org/abrom/rocketchat-ruby)
[![Code Climate Score](http://img.shields.io/codeclimate/github/abrom/rocketchat-ruby.svg?style=flat)](https://codeclimate.com/github/abrom/rocketchat-ruby)
[![Gem Version](http://img.shields.io/gem/v/rocketchat.svg?style=flat)](#)

# Rocket.Chat REST API for Ruby

This is a gem wrapping the v1 REST API for [Rocket.Chat](https://rocket.chat/).

The gem is based on a fork of http://github.com/int2xx9/ruby-rocketchat however that gem implemented v0.1
of the Rocket.Chat API and it was not forward compatible. Thanks to [@int2xx9](http://github.com/int2xx9) for the
framework on which this gem was based 

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rocketchat'
```

And then execute:

    $ bundle


## Supported API calls

This gem supports the following Rocket.Chat APIs (Tested against Rocket.Chat v0.54)

#### Miscellaneous information
* /api/v1/info

#### Authentication
* /api/v1/login
* /api/v1/logout
* /api/v1/me

### Users
* /api/v1/users.create
* /api/v1/users.delete
* /api/v1/users.info
* /api/v1/users.setAvatar
* /api/v1/users.update


## Usage

To get Rocket.Chat version

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
info = rocket_server.info
puts "Rocket.Chat version: #{info.version}"
```


To logout from a server:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
# ... use the API ...
session.logout
```


To create a user:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
user = session.users.create('new_username', 'user@example.com', 'New User', '123456',
                     active: true, send_welcome_email: false)
```

Optional parameters for create are:

:active, :roles, :join_default_channels, :require_password_change, :send_welcome_email, :verified, :custom_fields


To update a user:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
user = session.users.update('LAjzCDLqggCT7B82M',
  email: 'updated@example.com',
  name: 'Updated Name',
  roles: ['user', 'moderator']
)
```

Optional parameters for update are:

:username, :email, :password, :name, :active, :roles, :join_default_channels, :require_password_change, :send_welcome_email, :verified, :custom_fields


To get user info:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
user = session.users.info(username: 'some_username')
```

Either user_id (RocketChat's ID) or username can be used.


To delete a user, the same options as an info request can be used (`user_id` or `username`).

To search for (list) users:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
users = session.users.list(query: { email: 'foo@example.com' })
```


To set a user's avatar:

```ruby
require 'rocketchat'

rocket_server = RocketChat::Server.new('http://your.server.address/')
session = rocket_server.login('username', 'password')
success = session.users.set_avatar('http://image_url')
```

There is an optional parameter user_id, that works if the setting user is allowed to set other's avatar.


## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/abrom/rocketchat-ruby.

Note that spec tests are appreciated to minimise regressions. Before submitting a PR, please ensure that:
 
```bash
$ rspec
```
and

```bash
$ rubocop
```
both succeed 

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
