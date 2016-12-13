_[< back to index](README.md)_

## Useful tools for chef cookbooks testing

### Rake
From https://ruby.github.io/rake/:
>Rake is a Make-like program implemented in Ruby. Tasks and dependencies are specified in standard Ruby syntax.

Rake can save lots of typing, when testing chef cookbooks.
Let's assume we just have finished working with cookbook and want to check is all is fine and sound by running style check and tests.

Without using rake or any other task tool, we would need to do following in cookbook:
```shell
rubocop
foodcritic .
rspec
kitchen test all
```
and this can even get more complicated when we want to customize any of this commands.

Now, when using rake (with content the same as on main page) to achieve the same we need to type only:
`rake test`

Rake file can be user for any type of task, not just for testing, e.g. for updating berks dependencies, uploading cookbook, etc.

##### Links
- [Rake docs](https://ruby.github.io/rake/)
- [Another exmpple of rakefile for chef](https://github.com/chef-cookbooks/chef-server/blob/master/Rakefile)

### Guard
From https://github.com/guard/guard:
>Guard automates various tasks by running custom rules whenever file or directories are modified.

Which means, we can use guard to automatically start tests whenever file is saved to disk. This can make feedback loop extremely short when developing cookbooks.
Perhaps kitchen test are too slow to be integrated with Guard, but it works great with style checking and unit tests.

##### Links
- [Guard github page](https://github.com/guard/guard)
- [Example of guard file for chef development](https://gist.github.com/micgo/8226787)
- [Guard::Foodcritic](https://github.com/Nordstrom/guard-foodcritic)
