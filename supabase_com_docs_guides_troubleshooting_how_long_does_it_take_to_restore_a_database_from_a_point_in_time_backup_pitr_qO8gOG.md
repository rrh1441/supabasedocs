# How long does it take to restore a database from a Point-in-Time backup (PITR)?

Last edited: 1/18/2025

* * *

The time required for a PIT restoration isn't fixed. It depends on several factors:

**Time Since Last Full Backup:**

Full backups occur weekly. The time elapsed since the last full backup can affect restoration time.

**Write-Ahead Logging (WAL) Activity:**

The volume of WAL activity since the last full backup is a critical factor. More activity can lead to longer restoration times.

**Database Size:**

While important, the size of the database isn't the sole determinant of restoration time.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings