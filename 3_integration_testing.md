_[< back to index](README.md)_

## Integration testing of chef cookbooks

#### Test Kitchen
From http://kitchen.ci/:
>Test Kitchen is a test harness tool to execute your configured code on one or more platforms in isolation. [...] Many testing frameworks are already supported out of the box including Bats, shUnit2, RSpec, Serverspec, with others being created weekly.

You can think about test kitchen as a tool that takes chef cookbook and converge node to some state that later can be verified.

The point of kitchen tests (integration tests) is to verify end state of node.

Because of that, it makes sense to:
- avoid only-for-kitchen logic and recipes
- use suits to test different cases (other node attribute, other platforms)
- use .kitchen.yml to setup test environment (attributes, run list, environment)

##### How to with environment and data bags data bags
Sometimes you want to test cookbook which behavior depends on environment or data bags.
To achieve this, you can create your test data bags and environment, put them somewhere in test directory, e.g `test/integration/environments/..` and add following configuration to your kitchen file.
```ruby
provisioner:
  name: chef_zero
  data_path: 'test/integration'
  data_bags_path: 'test/integration/data_bags/'
  environments_path: 'test/integration/environments'
  cookbooks_path: '../../Cookbooks
```
##### How to test custom resources
When working with custom resources, especially in library cookbook, there is no recipes that we could use in testing with kitchen.
At the same time we still want to do integration testing of custom resources and achieve it without cluttering library cookbook with recipes which are only for tests.

To solved this problem we can create test cookbook inside cookbook we are workng with and use it kitchen tests.

In more details, lets assume we are working on `mycookbook` which provide `myfoo` custom resource.
Now to test that custom resource we need some recipe that will use it. To keep "real" logic separated from test logic, let create another cookbook, e.g. `testcookbook` and place it in `mycookbook/test/fixtures/cookbooks/testcookbook`.
Once we have created test cookbook, we can add there recipe using our custom resource `myfoo`.
Let's adjust berks file in main cookbooks to resolve properly test cookbook, e.g. in mycookbook berks file there should be entry:
```ruby
cookbook 'testcookbook', path: 'test/fixtures/cookbooks/testcookbook'
```
Now we are ready to start writing serverspec/inspec for kitchen.
We just need to remember to setup correct run list in kitchen file, e.g.
```ruby
suites:
  - name: default
    run_list:
      - "recipe[testcookbook::default]"
```
#### serverspec vs inspec
coming soon...

#### docker driver vs vagrant driver
coming soon...

#### Example of rake task for kitchen tests
```ruby
require 'kitchen/rake_tasks'
Kitchen::RakeTasks.new
```

#### Links
- [Chef docs on kitchen](https://docs.chef.io/kitchen.html)
- [Kitchen docs](http://kitchen.ci/)
- [Integration testing](https://semaphoreci.com/community/tutorials/integration-testing-for-chef-driven-infrastructure-with-test-kitchen)
- [Example of using cookbook inside of cookbook for testing](https://github.com/chef-cookbooks/apt/tree/master/test/fixtures/cookbooks/apt_test)
