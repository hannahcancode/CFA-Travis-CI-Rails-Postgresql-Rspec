# Travis CI, Rails, rspec and postgresql integration

- Set up your Travis CI hook to Github here: [Sign in to Travis CI](https://travis-ci.org/auth)

- Create a new file called `.travis.yml` in your root directory (i.e. at the same level as your `Gemfile`)

- In this `.travis.yml` directory, paste the following:

  ```yml
  language: ruby
  rvm:
    - 2.4.0
  cache: bundler
  services:
    - postgresql
  before_script:
    - bundle exec rake db:create
    - bundle exec rake db:migrate
  script: bundle exec rspec
  ```

- The above files do the following (explained in comments):

  ```yml
  # sets the language
  language: ruby

  # sets the ruby version/type
  # you can have more than one, just add more lines
  rvm:
    - 2.4.0

  # these actions will only be redone if they change
  # we only need to bundle if we add new things, and it
  # can take a long time so best to only do it when needed
  cache: bundler

  # sets up services like your databases
  services:
    - postgresql

  # scripts you run before your test
  # in this case, it creates and migrates our postgresql
  # database
  before_script:
    - bundle exec rake db:create
    - bundle exec rake db:migrate

  # the script that runs your tests
  script: bundle exec rspec
  ```

- You should already have a file `config/database.yml` that Travis CI will look into to see what test database it should create when the `db:create` command is run above. For example:

  ```yml
  test:
  <<: *default
  database: NameOfProject_test
  ```
