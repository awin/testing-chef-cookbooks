_[< back to index](README.md)_
## Style checks and static analysis of chef cookbooks

## Rubocop (part of chef dk)

From https://docs.chef.io/rubocop.html
> Most of the code that is authored when working with Chef is written as Ruby. Just about every file within a cookbook—with few exceptions!—is a Ruby file.
>
> Use RuboCop to author better Ruby code:
>
> * Enforce style conventions and best practices
> * Evaluate the code in a cookbook against metrics like “line length” and “function size”
> * Help every member of a team to author similary structured code
> * Estabilish uniformity of source code
> * Set expectations for fellow (and future) project contributors

Rubocop is detecting issues like:
- wrong / mixed indentation
- using double quotes (string interpolation) when not needed
- syntax errors

##### Using rubocop
To run rubocop simply execute following command in chef cookbook directory
`rubocop`

To run auto-correct
`rubocop -a`

To configure rubocop, create and modify `.rubocop.yml`

##### Rakefile task example
```ruby
require 'rubocop/rake_task'
RuboCop::RakeTask.new(:cookstyle) do |task|
  task.options = ['--fail-level', 'E']
end
```

##### Links
* [rubocop github page](https://github.com/bbatsov/rubocop)
* [chef docs on rubocop](https://docs.chef.io/rubocop.html)
* [Ruby style guide](https://github.com/bbatsov/ruby-style-guide)


## Cookstyle
From https://github.com/chef/cookstyle
> Cookstyle - Version Pinned rubocop and sane defaults for Chef Cookbooks

It is basically rubocop with selected cops that make sense for chef cookbooks.

Example of cops that are disabled in cookstyle:
* line length
* method length
* class length

##### Using cookstyle
As cookstyle is not part of chef dk, it needs to be first installed with `gem install cookstyle`
From now on, it can be used either as custom rule set for rubocop:
`rubocop -r cookstyle -D --format offenses`
or on its own:
`cookstyle -D --format offenses`

##### Rakefile task example
Setup is the same like for rubocop except for need to import cookstyle fist.
```ruby
require "cookstyle"
require "rubocop/rake_task"
RuboCop::RakeTask.new do |task|
  task.options << "--display-cop-names"
end
```
##### Links
* [Github page of cookstyle](https://github.com/chef/cookstyle)

## Foodcritic (part of chef dk)
From https://docs.chef.io/foodcritic.html
> Use Foodcritic to check cookbooks for common problems:
> * Style
> * Correctness
> * Syntax
> * Best practices
> * Common mistakes
> * Deprecations
>
> Foodcritic looks for lint-like behavior and reports it!

Foodcritic, unlike rubocop, detects style and syntax issues specific to chef cookbooks.

Example of issues that Foodcritic can rise:
* [Use a service resource to start and stop services](http://www.foodcritic.io/#FC004)
* [Use strings in preference to symbols to access node attributes](http://www.foodcritic.io/#FC001)
* [Invalid search syntax](http://www.foodcritic.io/#FC010)
* [Use file_cache_path rather than hard-coding tmp paths](http://www.foodcritic.io/#FC013)
* [LWRP does not declare a default action](http://www.foodcritic.io/#FC016)

##### Using foodcritic
To run foodcritic check, execute `foodcritic COOKBOOK_PATH`, to change foodcritic configuration create / update file `.foodcritic.yml`

##### Foodcritic task example
```ruby
require 'foodcritic'
  FoodCritic::Rake::LintTask.new(:foodcritic) do |task|
    task.options = { exclude: 'test/fixtures' }
  end
```
##### Links
* [Chef docs on foodcritic](https://docs.chef.io/foodcritic.html)
* [Foodcritic page](http://www.foodcritic.io/)
