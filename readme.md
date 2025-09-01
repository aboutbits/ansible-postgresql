# Ansible PostgreSQL Role

PostgreSQL installation role.

## Role Variables

- `postgresql_postgres_password`: The password that should be set for the default postgres user
- `postgresql_backup`: All information related to backups and if they should be enabled or not
- `postgresql_users`: A list of users that should be created.
- `postgresql_databases`: A list of databases including the schemas that should be created.
- `postgresql_privileges`: A list of privileges that should be granted for the different users to the different databases/schemas.
- `postgresql_default_privileges`: A list of privileges that should be set as defaults for the different users to tables/sequences inside the given databases/schemas.

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
        postgresql_privileges:
          - database: acme-product
            schema: app
            user: acme-user
            permissions: WRITE
        postgresql_privileges:
          - database: acme-product
            schema: app
            user: acme-reporting
            permissions: READ
        postgresql_default_privileges:
          - database: acme-product
            schema: app
            user: acme-reporting
            owner: acme-product
            permissions: READ
        postgresql_backup:
          enabled: true
          cron: 30 03 * * *
          directory: /usr/local/share/postgres
```

## Build & Publish

To build and publish the role, visit the GitHub Actions page of the repository and trigger the workflow "Release Package" manually.
