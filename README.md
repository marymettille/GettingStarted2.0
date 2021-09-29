# Cheat Sheet Codes

### Starting the ATS Locally

```sh
./atsdev start # || up
```
### Closing the ATS Locally

```sh
control^ + C # || down
```

### Updating Migrations

```sh
docker compose run web bin/rails db:migrate RAILS_ENV=development
```

### Staging

First, go to the #stage-deploy channel in slack and see if there is an open staging environment to use. If not, you can message the group in that channel to see if you can get one for a few hours.

- Start PritunI and connect using the pin you created, as well as the access code in Google Authentication app
- In your editor, type this into the terminal:

```sh
ssh bastion-stage
```

- To deploy to a staging environment, type the below into your terminal:

```sh
qa-deploy <stage_server> <branch_name>
```
Example:
```sh
qa-deploy stage4 PI_1234
```
- If you need product to review anything (or for QA to happen) go to the #qa channel in slack and message them that your changes are up on stage#. They will follow up with any changes you may need.

- To close the staging environment, type ```exit``` into the the terminal



### Testing

- ```yarn test``` runs the suite
- ```yarn testfile path/to/file.spec.js path/to/otherFile.spec.js``` will test the file(s) once
- ```yarn watchfile path/to/file.spec.js path/to/otherFile.spec.js``` will have the test run on each change
- ```yarn watchfile **/fileName.spec.js``` the ** glob will match any number of directories so this can save a lot of keystrokes.

Per Hovis: "If you're editing an existing feature and not changing functionality then I would rely on the existing tests. If the existing tests fail then they should be updated.  Tests have to be rewritten basically never."

Per Hovis: "If the existing tests are so gappy that they don't cover a bug you added that's an "our fault" instead of a "your fault". If you touch a component that has no tests...you should really add some. Ask for advice in this case."
