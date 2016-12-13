_[< back to index](README.md)_

## Unit testing of chef cookbooks

### What is unit testing
From [Art of Unit Testing](http://artofunittesting.com/definition-of-a-unit-test/):
>A good unit test is:
>* Able to be fully automated
>* Has full control over all the pieces running (Use mocks or stubs to achieve this isolation when needed)
>* Can be run in any order  if part of many other tests
>* Runs in memory (no DB or File access, for example)
>* Consistently returns the same result (You always run the same test, so no random numbers, for example. save those for integration or range tests)
>* Runs fast
>* Tests a single logical concept in the system
>* Readable
>* Maintainable
>* Trustworthy (when you see its result, you don’t need to debug the code just to be sure)

From [Seth Vargo's blog](https://sethvargo.com/unit-testing-chef-cookbooks/):
>[...]
> We need to start thinking about messages, not results (in unit tests).
>In other words, your test is "Did I tell Chef to install the package?", not "Did Chef install the package?".  [...]

### Why it does make sense to unit test cookbooks?
From [Seth Vargo's blog](https://sethvargo.com/unit-testing-chef-cookbooks/):
>Answer: regression. If you tell Chef to install a package, it will install a package... But what if you don't? Or, what if you accidentally change the way a file is written out during a refactor? That's what your unit tests will catch!

Aside from regression, unit testing is essential when working with custom resource / LWRP to verify those work as expected.

Another reason is to test code branches (ifs / switches) if they behave as expected.

### RSpec
Behavior Driven Development for Ruby.

RSpec is testing framework for testing ruby. It provides such building blocks as `context`, `describe`, `it` as well lots of [built in matchers](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers).

Example of rspec tests
```ruby
require 'rspec'

describe 'simple test' do
    it 'sums two numbers' do
       expect(2+3).to eq 5
    end
end
```

### Chefspec (part of chef dk)
From [chef docs](https://docs.chef.io/chefspec.html)
>ChefSpec is a framework that tests resources and recipes as part of a simulated chef-client run. ChefSpec tests execute very quickly. When used as part of the cookbook authoring workflow, ChefSpec tests are often the first indicator of problems that may exist within a cookbook.

Chefspec is RSpec + chef specific functionality.
While RSpec is testing framework that provides all needed components for testing ruby code, chefspec provides library of functions and objects specific to chef allowing for easy testing chef resources, notifications, working with chef client / server, mocking data bags and so on.

Example of chefspec test:
```ruby
require 'chefspec'

describe 'cookbook::default' do
  context 'When all attributes are default, on an ubuntu 16.04' do
    let(:chef_run) do
      runner = ChefSpec::ServerRunner.new(platform: 'ubuntu', version: '16.04')
      runner.converge(described_recipe)
    end

    describe 'when my recipe is doing something' do
        it 'converges successfully' do
          expect { chef_run }.to_not raise_error
        end
    end
  end
end
```
Example above is executed by rspec, but chefspec provides definition and for ServerRunner and described_recipe.

#### Writing tests in chefspec
Everything you need and even more can be found in [docs of chefspec](https://github.com/sethvargo/chefspec) with lots of examples.


#### Links
* [Chefspec docs](https://github.com/sethvargo/chefspec)
* [Chef docs on chefspec](https://docs.chef.io/chefspec.html)
* [More chefspec examples](http://michaellihs.github.io/devops/2015/07/28/chefspec.html)
* [Webinar Writing Great Unit Tests with ChefSpec](https://blog.chef.io/2016/03/31/watch-writing-great-unit-tests-with-chefspec/)
