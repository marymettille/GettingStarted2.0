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
