# Ruby TDD Project Template with thoughtbot's RuboCop Configuration 

This project template is set up for test-driven development (TDD) using Guard for automated test re-running, RSpec for writing and running tests, and RuboCop with thoughtbot’s style guide for code quality and style consistency.

## Setup 

### Gemfile 
The `Gemfile` defines the main dependencies for the project. To get started, install the gems by running:

```sh
bundle install
```
Your `Gemfile` should look something like this:

```ruby
source "https://rubygems.org"

gem "guard"              # Runs tasks automatically
gem "guard-rspec"        # Integrates RSpec with Guard
gem "guard-rubocop"      # Integrates RuboCop with Guard
gem "rspec"              # Test framework
gem "rspec-mocks"        # Mocking library for RSpec
gem "rubocop"            # Linter and code style enforcer
gem "rubocop-performance" # RuboCop cops for performance improvements
```

### thoughtbot’s RuboCop Configuration 

Include thoughtbot’s RuboCop Configuration:
To use the thoughtbot Ruby style guide, you can inherit it directly in your `.rubocop.yml`:

```yaml
inherit_from:
  - https://raw.githubusercontent.com/thoughtbot/guides/main/ruby/.rubocop.yml

# Custom rules for this project

# Example: Disable block length check
Metrics/BlockLength:
  Enabled: false

# Example: Increase max line length
Layout/LineLength:
  Max: 100
```

This configuration will apply thoughtbot’s style guide while allowing you to make project-specific changes as needed.
**Run RuboCop:** Once your `.rubocop.yml` file is set up, you can check for code style issues by running:

```sh
bundle exec rubocop
```
RuboCop will apply thoughtbot’s style guide and any additional configurations specified in your `.rubocop.yml`.
### Guardfile 
The `Guardfile` defines the setup for Guard to watch files and automatically trigger RuboCop and RSpec tasks when files change. 
- **Guard-Rubocop** : Runs RuboCop on all `.rb` files or when the `.rubocop.yml` file changes.
 
- **Guard-Rspec** : Watches all `spec/` files and triggers RSpec tests automatically on any changes.
Add this configuration to your `Guardfile`:

```ruby
guard :rubocop, cli: ["-A"] do
  watch(/.+\.rb$/)
  watch(%r{(?:.+/)?\.rubocop(?:_todo)?\.yml$}) { |m| File.dirname(m[0]) }
end

guard :rspec, cmd: "bundle exec rspec" do
  require "guard/rspec/dsl"
  dsl = Guard::RSpec::Dsl.new(self)

  rspec = dsl.rspec
  watch(rspec.spec_helper) { rspec.spec_dir }
  watch(rspec.spec_support) { rspec.spec_dir }
  watch(rspec.spec_files)
end
```

To start Guard and enable automatic code checks and test running, simply run:


```sh
bundle exec guard
```

## Running Tests 

RSpec is set up as the testing framework:
 
- Write your test files in the `spec/` directory.
 
- Run all tests with `bundle exec rspec`.

- Guard will automatically re-run tests upon file changes, providing instant feedback.
