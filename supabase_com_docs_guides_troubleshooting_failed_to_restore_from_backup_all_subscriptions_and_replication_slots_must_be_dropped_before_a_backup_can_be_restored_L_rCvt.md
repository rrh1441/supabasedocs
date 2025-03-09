# Failed to restore from backup: All subscriptions and replication slots must be dropped before a backup can be restored.

Last edited: 1/18/2025

* * *

As the error suggests, you must first drop any current subscriptions or replication slots to restore backups.

You can check those with:

`
SELECT * FROM pg_replication_slots;
SELECT * FROM pg_subscription;
`

You can drop them with:

`
DROP SUBSCRIPTION <subscription>;
SELECT pg_drop_replication_slot(slot_name);
`

NOTE: These are destructive actions. This is fine since you will overwrite your database with a backup.

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings