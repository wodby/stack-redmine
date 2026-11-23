# Redmine application stack for Kubernetes on Wodby

Deploy Redmine on Kubernetes with Wodby.

This repository defines the Wodby stack manifests and default service
composition for Redmine.

<!-- wodby:generated:start -->

## Stack contract

- [Redmine stack on Wodby](https://wodby.com/stacks/redmine)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Redmine stack guide](https://wodby.com/docs/2.0/stacks/catalog/redmine/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Redmine service](https://github.com/wodby/service-redmine)
- [PostgreSQL service](https://github.com/wodby/service-postgres)
- [Ganesha NFS provisioner service](https://github.com/wodby/service-nfs-provisioner)
- [OpenSMTPD service](https://github.com/wodby/service-opensmtpd)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Redmine<br>`redmine` | required; enabled by default; volumes: `files` 20 GB; links: `db` → `postgres`, `files` → `files-nfs`, `sendmail` → `opensmtpd` |
| PostgreSQL<br>`postgres` | required; enabled by default; volumes: `data` 10 GB |
| Redmine attachments NFS storage<br>`files-nfs` | required; enabled by default; volumes: `data` 25 GB |
| OpenSMTPD<br>`opensmtpd` | optional; disabled by default |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Deploy this stack

The stack needs no repository: Redmine comes with the prebuilt [`wodby/redmine`](https://github.com/wodby/redmine)
image.

Enter the administrator's password, and optionally a login, email and language, in the Redmine service's settings when
you create the app. The first deployment installs Redmine with that account in place of Redmine's stock `admin`/`admin`
account, loads the default trackers, statuses and roles in that language, and sets Redmine's host name to the app's
address. Later deployments run the database migrations a new image brings.

Attachments are kept on the shared `files` volume, so Redmine can run more than one replica. Enable OpenSMTPD to send
email notifications.

Plugins and themes need your own image built on `wodby/redmine`, used from a custom service based on the
[Redmine service](https://github.com/wodby/service-redmine).

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target
and derivative reference. Stack-local names and referenced service names are
distinct identifiers.
