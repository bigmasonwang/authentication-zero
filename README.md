# Authentication Zero

The purpose of authentication zero is to generate a pre-built authentication system into a rails application (web or api-only) that follows both security and rails best practices. By generating code into the user's application instead of using a library, the user has complete freedom to modify the authentication system so it works best with their app.

## Installation

```
$ bundle add authentication-zero
```

If you are using Rails < 7.2, you must use version 3.

```
$ bundle add authentication-zero --version "~> 3"
```

If you are using Rails < 7.1, you must use version 2.

```
$ bundle add authentication-zero --version "~> 2"
```

## Usage

```
$ rails generate authentication_zero
```

## Developer responsibilities

Since Authentication Zero generates this code into your application instead of building these modules into the gem itself, you now have complete freedom to modify the authentication system, so it works best with your use case. The one caveat with using a generated authentication system is it will not be updated after it's been generated. Therefore, as improvements are made to the output of `rails generate authentication_zero`, it becomes your responsibility to determine if these changes need to be ported into your application. Security-related and other important improvements will be explicitly and clearly marked in the `CHANGELOG.md` file and upgrade notes.

## Features

### Essential

- Sign up
- Email and password validations
- Checks if a password has been found in any data breach (--pwned)
- Authentication by cookie
- Authentication by token (--api)
- Two factor authentication + recovery codes (--two-factor)
- Two factor authentication using a hardware security key (--webauthn)
- Verify email using a link with token
- Ask password before sensitive data changes, aka: sudo (--sudoable)
- Reset the user password and send reset instructions
- Reset the user password only from verified emails
- Lock mechanism to prevent email bombing (--lockable)
- Send e-mail confirmation when your email has been changed
- Manage multiple sessions & devices
- Activity log (--trackable)
- Log out

### More

- Social login with omni auth (--omniauthable)
- Passwordless authentication (--passwordless)
- Send invitations (--invitable)
- "Sign-in as" button (--masqueradable)
- Multi-tentant application (--tenantable)

## Generated code

- [has_secure_password](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password): Adds methods to set and authenticate against a bcrypt password.
- [authenticate_by](https://edgeapi.rubyonrails.org/classes/ActiveRecord/SecurePassword/ClassMethods.html#method-i-authenticate_by): Given a set of attributes, finds a record using the non-password attributes, and then authenticates that record using the password attributes.
- [generates_token_for](https://edgeapi.rubyonrails.org/classes/ActiveRecord/TokenFor/ClassMethods.html#method-i-generates_token_for): Defines the behavior of tokens generated for a specific purpose.
- [signed cookies](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html): Returns a jar that'll automatically generate a signed representation of cookie value and verify it when reading from the cookie again.
- [httponly cookies](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html): A cookie with the httponly attribute is inaccessible to the JavaScript, this precaution helps mitigate cross-site scripting (XSS) attacks.
- [signed_id](https://api.rubyonrails.org/classes/ActiveRecord/SignedId.html): Returns a signed id that is tamper proof, so it's safe to send in an email or otherwise share with the outside world.
- [current attributes](https://api.rubyonrails.org/classes/ActiveSupport/CurrentAttributes.html): Abstract super class that provides a thread-isolated attributes singleton, which resets automatically before and after each request.
- [action mailer](https://api.rubyonrails.org/classes/ActionMailer/Base.html): Action Mailer allows you to send email from your application using a mailer model and views.
- [log filtering](https://guides.rubyonrails.org/action_controller_overview.html#log-filtering): Parameters 'token' and 'password' are marked [FILTERED] in the log.
- [functional tests](https://guides.rubyonrails.org/testing.html#functional-tests-for-your-controllers): In Rails, testing the various actions of a controller is a form of writing functional tests.
- [system testing](https://guides.rubyonrails.org/testing.html#system-testing): System tests allow you to test user interactions with your application, running tests in either a real or a headless browser.

### Sudoable

Use `before_action :require_sudo` in controllers with sensitive information, it will ask for your password on the first access or after 30 minutes.

### Tenantable

Some artifacts are generated in the application, which makes it possible to implement row-level multitenancy applications. The `Current.account` is set using the current user account.

You should follow some steps to make it work:

- Add `account_id` to each scoped table. ex: `rails g migration add_account_to_projects account:references`.
- Add `include AccountScoped` to scoped models. It set up the account relationship and default scope using the current account.

Set `Current.account` through the URL. `http://myapp.com/:account_id`. (optional)

- Add `require_relative "../lib/account_middleware"` to `config/application.rb`.
- Add `config.middleware.use AccountMiddleware` to your application class.
- More customization is required...

## Development

To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/lazaronixon/authentication-zero. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/lazaronixon/authentication-zero/blob/main/CODE_OF_CONDUCT.md).

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the AuthenticationZero project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/lazaronixon/authentication-zero/blob/main/CODE_OF_CONDUCT.md).
