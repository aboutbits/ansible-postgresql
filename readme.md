# Ansible PostgreSQL Role

PostgreSQL installation role.

## Role Variables

- `postgresql_postgres_password`: The password that should be set for the default postgres user
- `postgresql_backup`: All information related to backups and if they should be enabled or not
- `postgresql_users`: A list of users that should be created.
- `postgresql_databases`: A list of databases including the schemas that should be created.
- `postgresql_database_permissions`: A list of permissions that should be granted to different users to different databases.
- `postgresql_schema_permissions`: A list of permissions that should be granted to different users to different database schemas.

## Example Playbook

```yaml
- hosts: all
  tasks:
    - ansible.builtin.include_role:
        name: ansible-postgresql
      vars:
        postgresql_postgres_password: xxx
        postgresql_users:
          - name: acme-user
            password: xxx
        postgresql_databases:
          - name: acme-product
            schema: app
            owner: acme-user
        postgresql_database_permissions:
          - database: acme-product
            user: acme-user
            privileges: CREATE
        postgresql_schema_permissions:
          - database: acme-product
            schema: app
            user: acme-user
            privileges: ALL
        postgresql_backup:
          enabled: true
          cron: 30 03 * * *
          directory: /usr/local/share/postgres
```

## Versioning

In order to have a verioning in place and working, create leightweight tags that point to the appropriate minor release versions.

Creating a new minor release:

```bash
git tag v1
git push --tags
```

Replacing an already existing minor release:

```bash
git tag -d v1
git push origin :refs/tags/v1
git tag v1
git push --tags
```
