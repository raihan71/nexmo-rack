# Nexmo Rack Middleware

This repo contains Rack middleware that can be used to help integrate Nexmo in to your Rack-based application. It currently contains the following middleware:

* Verify [Nexmo signatures](https://developer.nexmo.com/concepts/guides/signing-messages).
* Plus more to be added

## Table of contents

* [Installation and Usage](#installation-and-usage)
    * [As a standalone application](#as-a-standalone-application)
    * [Mounted into a Rails application](#mounted-into-a-rails-application)
* [Contributing](#contributing)
* [License](#license)

## Installation and Usage

The verify signature middleware can be used standalone or integrated into a Ruby application. The middleware will return a `403` HTTP status code if the signature is not valid, and will continue the application if it is valid.

### Configuration

You'll need to provide a Nexmo signature secret and signature method using either `ENV` variables or the Rails credentials system.

`.env` example:

```
NEXMO_SIGNATURE_SECRET = 'your_secret_key'
NEXMO_SIGNATURE_METHOD = 'md5hash'
```

Alternatively, you can specify them in the Rails credentials system

```
EDITOR="code --wait" rails credentials:edit
```

You can replace the EDITOR variable with your preferred editor. Once the credentials file is open, you are able to add the Nexmo credentials with the following namespacing:

```yaml
nexmo:
    signature_secret: your_secret_key
    signature_method: md5hash
```

Finally, this middleware will ignore any requests that do not contain a `sig` key. To enforce all requests to be validated, set `NEXMO_SIGNATURE_REQUIRED` to `true` in the environment.

### As a standalone application

Install the gem on your system:

``` shell
$ gem install nexmo_rack
```

Then require it from within your `config.ru` Rack configuration:

``` ruby
use Nexmo::Rack::VerifySignature
```

An example [config.ru](examples/config.ru.example) can be found in the examples folder. More information on getting up and running with Rack can be found at the [Rack GitHub repository](https://github.com/rack/rack/wiki/(tutorial)-rackup-howto#with-a-ru-config-file).

### Mounted into a Rails Application

Require it in your `Gemfile`:

```ruby
gem nexmo_rack
```

And then add the middleware to your `config/application.rb` file to initialize it with your application:

```ruby
config.middleware.use Nexmo::Rack::VerifySignature
```

## Contributing
We ❤️ contributions from everyone! [Bug reports](https://github.com/Nexmo/nexmo_rack/issues), [bug fixes](https://github.com/Nexmo/nexmo_rack/pulls) and feedback on the library is always appreciated. Look at the [Contributor Guidelines](https://github.com/Nexmo/nexmo_rack/blob/master/CONTRIBUTING.md) for more information.

## License
This project is under the [MIT LICENSE](https://github.com/Nexmo/nexmo_rack/blob/master/LICENSE).
