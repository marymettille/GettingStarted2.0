# Cheat Sheet Codes

### Starting the ATS Locally

```sh
./atsdev start # || dc up
```
### Closing the ATS Locally

```sh
control^ + C # || dc down
```

### Screaming Nuke

```sh
NUKEPARTONE ()
{
    docker-compose kill;
    docker-compose down;
    docker-sync clean;
    docker container prune -y;
    docker image prune -a -y;
    docker volume prune -y;
    docker system prune -y
    docker image ls -a;
    docker container ls -a;
    docker volume ls;
    ./atsdev build;
    docker-compose down;
    docker-sync clean;
    docker-compose up -d db;
    docker-composeup web assets
}

NUKEPARTTWO ()
{
    #In a new terminal, while your stack is up!
    docker compose exec web rails db:reset SEED_INITIAL_DEVELOPMENT_COMPANY=true;
    docker compose exec test rails db:reset SEED_INITIAL_DEVELOPMENT_COMPANY=true;
    ./atsdev start
}
```

### Updating Migrations

```sh
docker compose run web bin/rails db:migrate RAILS_ENV=development
```

## Running Local Version of storybook

```sh
dc exec assets yarn storybook
```

### Pull new docker images

- Checkout Master
- Pull master
- Stop local server
- `docker-compose pull`

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

- To access a staging server, read this confluence article: https://hiringthing.atlassian.net/wiki/spaces/HTR/pages/886046763/Stage+and+testing+URLs

### Database out of sync with master // Rebuilding database

Sometimes the database on your local machine will become out of sync with the master. When this happens, you will not be able to simply merge the change in structure.sql, because your local will say that the default in master is "wrong". To fix this you need to either:

- Rebuild your stack with ```./atsdev build```
- Reset your database intirely with 
```sh 
dc run web bin/rails db:drop
dc run web bin/rails db:create
dc run web bin/rails db:schema:load
dc run -e SEED_INITIAL_DEVELOPMENT_COMPANY=1 web bin/rails db:seed
```

### Installing new dependencies
- ```yarn install -W WHATEVER WHATEVER```
- ```dcl build assets```
- Bring down your stack with ```dc down```
- Bring up your stack with ```dcl up -d web``` or ```dcl up``` (./astdev will not work!)
- ```docker-compose -f docker-compose.yml -f docker-compose.local.yml```
- The thing that matters is that docker-compose.yml specifies a cached (downloaded) Docker image for the assets container that doesn't include your new modules
- dcl will use an overlay so that the assets container is built on your local instead, so that it has the updated modules.
- Our CI process always builds the JS env from scratch (and is also responsible for pushing new docker images to the repository for caching)

## Testing

### Enzyme

- ```yarn test``` runs the suite
- ```yarn testfile path/to/file.spec.js path/to/otherFile.spec.js``` will test the file(s) once
- ```yarn watchfile path/to/file.spec.js path/to/otherFile.spec.js``` will have the test run on each change
- ```yarn watchfile **/fileName.spec.js``` the ** glob will match any number of directories so this can save a lot of keystrokes.

Per Hovis: "If you're editing an existing feature and not changing functionality then I would rely on the existing tests. If the existing tests fail then they should be updated.  Tests have to be rewritten basically never."

Per Hovis: "If the existing tests are so gappy that they don't cover a bug you added that's an "our fault" instead of a "your fault". If you touch a component that has no tests...you should really add some. Ask for advice in this case."

### Jest
- `yarn jest NAME_OF_TEST` will test the jest test once
    - Example `yarn jest ScorecardReviews`
- `yarn jest NAME_OF_TEST --watchAll` will test the jest test and rerun all tests again

### Cypress
- `docker-compose up cypress_server` starts up the cypress server
- `yarn run cypress open` runs the cypress test runner

### Ruby // Rails
- `docker-compose exec test FILEPATH` will test the rails test if your stack is down
- `docker-compose exec test rails test  FILEPATH` will test the rails test of your choosing if your stack is up

