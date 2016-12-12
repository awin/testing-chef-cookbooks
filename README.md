## How to test chef cookbooks

#### 1. [Style check / static code analysis](1_style_checks.md)
#### 2. [Unit testing](2_unit_testing.md)
#### 3. [Integration testing](3_integration_testing.md)
#### 4. [Tooling](4_tooling.md)
#### 5. [Learning resources](5_learning_resources.md)  

---
### Example Rakefile

```ruby
namespace :style do
  require 'cookstyle'
  require 'rubocop/rake_task'
  RuboCop::RakeTask.new(:cookstyle) do |task|
    task.options = ['--fail-level', 'E']
  end

  require 'foodcritic'
  FoodCritic::Rake::LintTask.new(:foodcritic) do |task|
    task.options = { exclude: 'test/fixtures' }
  end
end
task style: ['style:cookstyle', 'style:foodcritic']

namespace :unit do
  require 'rspec/core/rake_task'
  RSpec::Core::RakeTask.new(:rspec) do |task|
    task.rspec_opts = '--format documentation'
  end
end
task unit: ['unit:rspec']

namespace :integration do
  require 'kitchen/rake_tasks'
  Kitchen::RakeTasks.new
end
task integration: %w(integration:kitchen:all)

desc 'Run style checks, unit and integration tests'
task test: %w(style unit integration)

task default: 'test'
```

---
### Example Guardfile

```ruby
# TODO
```
