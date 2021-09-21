# Cheat Sheet Codes

### Starting

```sh
./atsdev start # || up
```

### Updating Migrations

```sh
docker compose run web bin/rails db:migrate RAILS_ENV=development
```

### Staging Access

- Start PritunI and connect using the pin you created, as well as the access code in Google Authentication app
- In VSCode, type this into the terminal:

```sh
ssh bastion-stage
```

- To close the staging environment, type ```exit``` into the the terminal

### Testing

- ```yarn test``` runs the suite
- ```yarn testfile path/to/file.spec.js path/to/otherFile.spec.js``` will test the file(s) once
- ```yarn watchfile path/to/file.spec.js path/to/otherFile.spec.js``` will have the test run on each change
- ```yarn watchfile **/fileName.spec.js``` the ** glob will match any number of directories so this can save a lot of keystrokes.

Per Hovis: "If you're editing an existing feature and not changing functionality then I would rely on the existing tests. If the existing tests fail then they should be updated.  Tests have to be rewritten basically never."
Per Hovis: "If the existing tests are so gappy that they don't cover a bug you added that's an "our fault" instead of a "your fault". If you touch a component that has no tests...you should really add some. Ask for advice in this case."
