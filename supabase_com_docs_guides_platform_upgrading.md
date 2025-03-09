Platform

# Upgrading

* * *

Supabase ships fast and we endeavor to add all new features to existing projects wherever possible. In some cases, access to new features require upgrading or migrating your Supabase project.

You can upgrade your project using `pg_upgrade` or by pausing and restoring your project.

## `pg_upgrade` [\#](https://supabase.com/docs/guides/platform/upgrading\#pgupgrade)

This upgrade method is currently in Beta.

For security purposes, passwords for custom roles are not backed up and, following a restore, they
would need to be reset. See [here](https://supabase.com/docs/guides/platform/backups#daily-backups) for more details

pg\_upgrade performs an in-place upgrade on your database. For projects larger than 1GB, pg\_upgrade is generally faster than a pause+restore cycle, and the speed advantage grows with the size of the database.

1. Plan for an appropriate downtime window, and ensure you have reviewed the [caveats](https://supabase.com/docs/guides/platform/upgrading#caveats) section of this document before executing the upgrade.
2. Use the "Upgrade project" button on the [Infrastructure](https://supabase.com/dashboard/project/_/settings/infrastructure) section of your dashboard.

Additionally, if a pg\_upgrade upgrade should fail, your original DB would be brought back up online and be able to service requests.

As a rough rule of thumb, pg\_upgrade operates at ~100mbps (when executing an upgrade on your data). Using the size of your database, you can use this metric to derive an approximate sense of the downtime window necessary for the upgrade. During this window, you should plan for your DB and associated services to be unavailable.

## Pause and restore [\#](https://supabase.com/docs/guides/platform/upgrading\#pause-and-restore)

We recommend using the pg\_upgrade method, as it is faster, and more reliable. Additionally, only Free-tier projects are eligible to use the Pause + Restore method.

When you pause and restore a project, the restored database includes the latest features. This method _does_ include downtime, so be aware that your project will be inaccessible for a short period of time.

1. On the [General Settings](https://supabase.com/dashboard/project/_/settings/general) page in the Dashboard, click **Pause project**. You will be redirected to the home screen as your project is pausing. This process can take several minutes.
2. After your project is paused, click **Restore project**. The restoration can take several minutes depending on how much data your database has. You will receive an email once the restoration is complete.

Note that a pause + restore upgrade involves tearing down your project's resources before bringing them back up again. If the restore process should fail, manual intervention from Supabase support will be required to bring your project back online.

### Caveats [\#](https://supabase.com/docs/guides/platform/upgrading\#caveats)

Regardless of the upgrade method, a few caveats apply:

#### Logical replication [\#](https://supabase.com/docs/guides/platform/upgrading\#logical-replication)

If you are using logical replication, the replication slots will not be preserved by the upgrade process. You will need to manually recreate them after the upgrade with the method `pg_create_logical_replication_slot`. Refer to the Postgres docs on [Replication Management Functions](https://www.postgresql.org/docs/current/functions-admin.html#FUNCTIONS-REPLICATION) for more details about the method.

#### Breaking changes [\#](https://supabase.com/docs/guides/platform/upgrading\#breaking-changes)

Newer versions of services can break functionality or change the performance characteristics you rely on. If your project is eligible for an upgrade, you will be able to find your current service versions from within [the Supabase dashboard](https://supabase.com/dashboard/project/_/settings/infrastructure).

Breaking changes are generally only present in major version upgrades of Postgres and PostgREST. You can find their respective release notes at:

- [Postgres](https://www.postgresql.org/docs/release/)
- [PostgREST](https://github.com/PostgREST/postgrest/releases)

If you are upgrading from a significantly older version, you will need to consider the release notes for any intermediary releases as well.

#### Time limits [\#](https://supabase.com/docs/guides/platform/upgrading\#time-limits)

Starting from 2024-06-24, when a project is paused, users then have a 90-day window to restore the project on the platform from within Supabase Studio.

The 90-day window allows Supabase to introduce platform changes that may not be backwards compatible with older backups. Unlike active projects, static backups can't be updated to accommodate such changes.

During the 90-day restore window a paused project can be restored to the platform with a single button click from [Studio's dashboard page](https://supabase.com/dashboard/projects).

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-90-day.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

After the 90-day restore window, you can download your project's backup file, and Storage objects from the project dashboard. See [restoring a backup locally](https://supabase.com/docs/guides/platform/upgrading#restoring-a-downloaded-backup-locally) for instructions on how to load that backup locally to recover your data.

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-dl-backup.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

If you upgrade to a paid plan while your project is paused, any expired one-click restore options are reenabled. Since the backup was taken outside the backwards compatibility window, it may fail to restore. If you have a problem restoring your backup after upgrading, contact [Support](https://supabase.com/support).

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-paid-tier.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

#### Restoring a downloaded backup locally [\#](https://supabase.com/docs/guides/platform/upgrading\#restoring-a-downloaded-backup-locally)

If you want to restore your backup to a hosted Supabase project, you will need to use the
[Migrating within Supabase guide](https://supabase.com/docs/guides/platform/migrating-within-supabase)

If the 90 day project restore window has expired but you need to access data contained within your project using SQL, you can use the [Supabase CLI](https://supabase.com/docs/guides/local-development) to restore the project into a local Postgres instance.

First, download your project's backup file from dashboard and identify its backup image version (following the `PG:` prefix):

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-dl-image-version.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

Given Postgres version `15.6.1.115`, start Postgres locally with `db_cluster.backup` being the path to your backup file.

`
supabase init
echo '15.6.1.115' > supabase/.temp/postgres-version
supabase db start --from-backup db_cluster.backup
`

Note that the earliest Supabase Postgres version that supports a local restore is `15.1.0.55`. If your hosted project was running on earlier versions, you will likely run into errors during restore. Before submitting any support ticket, make sure you have attached the error logs from `supabase_db_*` docker container.

Once your local database starts up successfully, you can connect using psql to verify that all your data is restored.

`
psql 'postgresql://postgres:postgres@localhost:54322/postgres'
`

If you want to use other services like Auth, Storage, and Studio dashboard together with your restored database, restart the local development stack.

`
supabase stop
supabase start
`

A Postgres database started with Supabase CLI is not production ready and should not be used outside of local development.

#### Disk sizing [\#](https://supabase.com/docs/guides/platform/upgrading\#disk-sizing)

When upgrading, the Supabase platform will "right-size" your disk based on the current size of the database. For example, if your database is 100GB in size, and you have a 200GB disk, the upgrade will reduce the disk size to 120GB (1.2x the size of your database).

#### Objects dependent on Postgres extensions [\#](https://supabase.com/docs/guides/platform/upgrading\#objects-dependent-on-postgres-extensions)

pg\_upgrade does not support upgrading of databases containing reg\* data types referencing system OIDs.
If you have created any objects that depend on the following extensions, you will need to recreate them after the upgrade.

#### `pg_cron` records [\#](https://supabase.com/docs/guides/platform/upgrading\#pgcron-records)

[pg\_cron](https://github.com/citusdata/pg_cron#viewing-job-run-details) does not automatically clean up historical records. This can lead to extremely large `cron.job_run_details` tables if the records are not regularly pruned; you should clean unnecessary records from this table prior to an upgrade.

During the `pg_upgrade` process, the `pg_cron` extension gets dropped and recreated. Prior to this process, the `cron.job_run_details` table is duplicated to avoid losing historical logs. The instantaneous disk pressure created by duplicating an extremely large details table can cause at best unnecessary performance degradation, or at worst, upgrade process failures.

#### Extensions [\#](https://supabase.com/docs/guides/platform/upgrading\#extensions)

pg\_upgrade does not currently support upgrading of databases using extensions older than the following versions:

- TimescaleDB 2.16.1
- plv8 3.1.10

To upgrade to a newer version of Postgres, you will need to drop the extensions before the upgrade, and recreate them after the upgrade.

#### Authentication method changes - deprecating md5 in favor of scram-sha-256 [\#](https://supabase.com/docs/guides/platform/upgrading\#authentication-method-changes---deprecating-md5-in-favor-of-scram-sha-256)

The md5 hashing method has [known weaknesses](https://en.wikipedia.org/wiki/MD5#Security) that make it unsuitable for cryptography.
As such, we are deprecating md5 in favor of [scram-sha-256](https://www.postgresql.org/docs/current/auth-password.html), which is the default and most secure authentication method used in the latest Postgres versions.

We automatically migrate Supabase-managed roles' passwords to scram-sha-256 during the upgrade process, but you will need to manually migrate the passwords of any custom roles you have created, else you won't be able to connect using them after the upgrade.

To identify roles using the md5 hashing method and migrate their passwords, you can use the following SQL statements after the upgrade:

`
-- List roles using md5 hashing method
SELECT
rolname
FROM pg_authid
WHERE rolcanlogin = true
AND rolpassword LIKE 'md5%';
-- Migrate a role's password to scram-sha-256
ALTER ROLE <role_name> WITH PASSWORD '<password>';
`

#### Database size reduction [\#](https://supabase.com/docs/guides/platform/upgrading\#database-size-reduction)

As part of the upgrade process, maintenance operations such as [vacuuming](https://www.postgresql.org/docs/current/routine-vacuuming.html#ROUTINE-VACUUMING) are also executed. This can result in a reduction in the reported database size.

#### Post-upgrade validation [\#](https://supabase.com/docs/guides/platform/upgrading\#post-upgrade-validation)

Supabase performs extensive pre- and post-upgrade validations to ensure that the database has been correctly upgraded. However, you should plan for your own application-level validations, as there might be changes you might not have anticipated, and this should be budgeted for when planning your downtime window.

### Is this helpful?

NoYes

### On this page

[pg\_upgrade](https://supabase.com/docs/guides/platform/upgrading#pgupgrade) [Pause and restore](https://supabase.com/docs/guides/platform/upgrading#pause-and-restore) [Caveats](https://supabase.com/docs/guides/platform/upgrading#caveats)

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-90-day.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-dl-backup.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-paid-tier.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

![Project Paused: 90 Days Remaining](https://supabase.com/docs/_next/image?url=%2Fdocs%2Fimg%2Fguides%2Fplatform%2Fpaused-dl-image-version.png&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings