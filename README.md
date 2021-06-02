had trouble with rails on mac so ended up with Vagrant

## go through all the steps again, document

- start a new project with the code from rails-vagrant-muck
- wire it up to a new git repo
- get it to build locally with Vagrant (simulate building divetst locally)
- deploy to heroku and add add-ons to heroku using the heroku cli

## here goes

### Vagrant

- move `Vagrantfile` and `bootstrap.sh` into the rails project dir
- `vagrant up` -> `vagrant ssh` -> `cd /vagrant`
- `sudo chmod -R 777 /usr/local/rvm/gems/ruby-2.6.6`

### Setup and start rails app

- from vagrant machine:
  - `bundle install`
  - confirm that `config/database.yml` dev password is set properly (for this vagrant installation the root password is "root")
  - `rake db:setup`
  - `rails s -b 0.0.0.0`
  - confirm access to localhost:3000 from host

### Setup heroku app

- from host & from rails project root:
  - `heroku create [name of app]` (this creates a named app)
  - verify remote was added `git config --list | grep heroku`

### Setup heroku DB

- from host & from rails project root:
  - `heroku addons:create cleardb:ignite`
  - `heroku config | grep CLEARDB_DATABASE_URL`
  - copy the value of the `CLEARDB_DATABASE_URL` config variable
  - change `mysql://` to `mysql2://`
  - `heroku config:set DATABASE_URL='[value of edited CLEARDB_DATABASE_URL]'`
  - `heroku run rake db:migrate`

### Deploy to heroku

- from host & from rails project root:
- `git push heroku main`
