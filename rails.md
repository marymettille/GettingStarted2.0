# Rails

### Summary
Our backend is in Ruby and Rails. As a frontend engineer, you may need to periodically read and/or write both. Here are some fast tips for when need to work in rails.

### Opening a rails terminal

To open a rails terminal, type: `docker-compose run web bundle exec rails c` into a new terminal. *I've aliased this as railsTerminal in my .zshrc*.

### Declaring variables

You can declare variables two different ways in rails:

```rails
User.find_by(email:'test@example.com')
// Some results will show up here
user = _
```
The underscore in the above will alias the above line of code. You can also do something like this:

```rails
user = User.find_by(email:'test@example.com')
```

### Changing the company the user belongs to

Some of our whitelabels have different login settings. Checkwriters is one of these - which means that you are going to have problems if you close your dev environment and attempt to log back in. In order to change your company back to hiringthing, you will want to do this sequence of commands:

```rails
user = User.find_by(email:'test@example.com')
hiringthing = WhitelabelAccount.find_by(system_version: 'hiringthing')
user.company.update(whitelabel_account: hiringthing)
```