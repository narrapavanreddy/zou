# Data migration

## Raw mode

To migrate a Kitsu from an instance to another, you can simply dump the
database and restore it to the new instance. Once done, you have to move all 
the files stored in the preview folder to the preview folder of the new
instance.

## With Zou CLI

We assume here that you are evolving in the zou virtualenv environment and that
all your environment variables are loaded.

### Prepare the new instance (reset data)

Clear original database and rebuild tables:

```
zou clear_db
zou reset_migrations
zou upgrade_db
```

### Get data

Retrieve base data:

```
SYNC_LOGIN="admin@yourstudio.com" \
SYNC_PASSWORD="password" \
zou sync_full --target http://yourpreviouskitsu.url/api --no-projects
```

Retrieve project data:

```
SYNC_LOGIN="admin@yourstudio.com" \
SYNC_PASSWORD="password" \
zou sync_full --target http://yourpreviouskitsu.url/api --only-projects
```

Retrieve a given project:

```
SYNC_LOGIN="admin@yourstudio.com" \
SYNC_PASSWORD="password" \
zou sync_full --target http://yourpreviouskitsu.url/api --project AwesomeProject
```

If some changes occured after the migration, you can run the command again and
retrieve the difference. Beware that deletion won't be handled.

### Get files

The previous steps were used to retrieve the data stored in the database.

Retrieve all files:

```
SYNC_LOGIN="admin@yourstudio.com" \
SYNC_PASSWORD="password" \
zou sync_full_files --target http://yourpreviouskitsu.url/api
```

Retrieve files for a given project:

```
SYNC_LOGIN="admin@yourstudio.com" \
SYNC_PASSWORD="password" \
zou sync_full_files --target http://yourpreviouskitsu.url/api
```

If some changes occured after the migration, you can run the command again and
retrieve the difference. Beware that deletion won't be handled.