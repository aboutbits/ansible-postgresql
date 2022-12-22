# Ansible PostgreSQL Role

PostgreSQL installation role.

## Role Variables

- `postgresql_postgres_password`: The password that should be set for the default postgres user
- `postgresql_backup`: All information related to backups and if they should be enabled or not
- `postgresql_users`: A list of users that should be created.
- `postgresql_databases`: A list of databases including the schemas that should be created.
- `postgresql_permissions`: A list of permissions that should be granted to different users to different databases.

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
        postgresql_permissions:
          - database: acme-product
            user: acme-user
            schema: app
            privileges: ALL
        postgresql_backup:
          enabled: true
          directory: /usr/local/share/postgres
```

## Versioning

In order to have a verioning in place and working, create leightweight tags that point to the appropriate minor release versions.

Creating a new minor release:

```bash
git tag 1.0
git push --tags
```

Replacing an already existing minor release:

```bash
git tag -d 1.0
git push origin :refs/tags/1.0
git tag 1.0
git push --tags
```
