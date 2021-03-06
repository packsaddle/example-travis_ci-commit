# Example::TravisCi

run rubocop and pull request review comment

[Actual script for TravisCI](./bin/run-tests.sh)

```
# .travis.yml
script: ./bin/run-tests.sh

# bin/run-tests.sh
#!/bin/bash
set -v
if [ -n "${TRAVIS_PULL_REQUEST}" ] && [ "${TRAVIS_PULL_REQUEST}" != "false" ]; then
  gem install --no-document rubocop-select rubocop rubocop-checkstyle_formatter \
              checkstyle_filter-git saddler saddler-reporter-github

  git diff -z --name-only origin/master \
   | xargs -0 rubocop-select \
   | xargs rubocop \
       --require rubocop/formatter/checkstyle_formatter \
       --format RuboCop::Formatter::CheckstyleFormatter \
   | checkstyle_filter-git diff origin/master \
   | saddler report \
      --require saddler/reporter/github \
      --reporter Saddler::Reporter::Github::PullRequestReviewComment
fi
bundle exec rake
```

[Travis CI: Encryption keys](http://docs.travis-ci.com/user/encryption-keys/)

```
$ gem install travis
$ travis encrypt -r <repos_name> "GITHUB_ACCESS_TOKEN=<github_token>"
```

## Contributing

1. Fork it ( https://github.com/packsaddle/example-ruby-travis-ci/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
