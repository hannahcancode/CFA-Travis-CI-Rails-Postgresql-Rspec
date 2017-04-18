# Travis CI, Rails, rspec and postgresql integration

## About

This guide shows you how to integrate Travis CI with your Ruby on Rails project that uses rspec for testing and postgresql for database.

## How To

- Set up your Travis CI hook to Github here: [Sign in to Travis CI](https://travis-ci.org/auth)

- Navigate to your profile and "turn on" the repository you want to test in

- In your rails project, create a new file called `.travis.yml` in your root directory (i.e. at the same level as your `Gemfile`)

- In this `.travis.yml` file, paste the following:

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

- The above does the following (explained in comments):

  ```yml
  # sets the language
  language: ruby

  # sets the ruby version/type
  # if you have more than one version, just add more lines
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
  # in this case, it creates and migrates our postgresql database
  before_script:
    - bundle exec rake db:create
    - bundle exec rake db:migrate

  # the script that runs your tests
  script: bundle exec rspec
  ```

- You should already have a file `config/database.yml` that Travis CI will look into to see what test database it should create when the `db:create` command is run above. For example, in the test section:

  ```yml
  test:
  <<: *default
  database: NameOfProject_test
  ```

- When you're happy, push to your GitHub repository. Travis CI will automatically detect you have pushed and run your tests.

## Get your badge!

- To get your "Build: Passing" badge in your README.md, click into your repository on Travis CI, and click on the badge next to your repository name. Select the dropdown menu with 'Image URL' and select 'Markdown'. Copy and paste the Markdown code into your README.md.

## Troubleshooting

- If you get a build error it could be that Travis CI couldn't run your project properly - check you gave it a valid ruby version, that your dependencies are all ok, and that your databases are being created. You can follow along what's actually happening in the logs and see where you're getting an error.
