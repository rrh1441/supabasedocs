Storage

# Storage Quickstart

## Learn how to use Supabase to store and serve files.

* * *

This guide shows the basic functionality of Supabase Storage. Find a full [example application on GitHub](https://github.com/supabase/supabase/tree/master/examples/user-management/nextjs-user-management).

## Concepts [\#](https://supabase.com/docs/guides/storage/quickstart\#concepts)

Supabase Storage consists of Files, Folders, and Buckets.

### Files [\#](https://supabase.com/docs/guides/storage/quickstart\#files)

Files can be any sort of media file. This includes images, GIFs, and videos. It is best practice to store files outside of your database because of their sizes. For security, HTML files are returned as plain text.

### Folders [\#](https://supabase.com/docs/guides/storage/quickstart\#folders)

Folders are a way to organize your files (just like on your computer). There is no right or wrong way to organize your files. You can store them in whichever folder structure suits your project.

### Buckets [\#](https://supabase.com/docs/guides/storage/quickstart\#buckets)

Buckets are distinct containers for files and folders. You can think of them like "super folders". Generally you would create distinct buckets for different Security and Access Rules. For example, you might keep all video files in a "video" bucket, and profile pictures in an "avatar" bucket.

File, Folder, and Bucket names **must follow** [AWS object key naming guidelines](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-keys.html) and avoid use of any other characters.

## Create a bucket [\#](https://supabase.com/docs/guides/storage/quickstart\#create-a-bucket)

You can create a bucket using the Supabase Dashboard. Since the storage is interoperable with your Postgres database, you can also use SQL or our client libraries. Here we create a bucket called "avatars":

DashboardSQLJavaScriptDartSwiftPython

1. Go to the [Storage](https://supabase.com/dashboard/project/_/storage/buckets) page in the Dashboard.
2. Click **New Bucket** and enter a name for the bucket.
3. Click **Create Bucket**.

## Upload a file [\#](https://supabase.com/docs/guides/storage/quickstart\#upload-a-file)

You can upload a file from the Dashboard, or within a browser using our JS libraries.

DashboardJavaScriptDart

1. Go to the [Storage](https://supabase.com/dashboard/project/_/storage/buckets) page in the Dashboard.
2. Select the bucket you want to upload the file to.
3. Click **Upload File**.
4. Select the file you want to upload.

## Download a file [\#](https://supabase.com/docs/guides/storage/quickstart\#download-a-file)

You can download a file from the Dashboard, or within a browser using our JS libraries.

DashboardJavaScriptDartSwiftPython

1. Go to the [Storage](https://supabase.com/dashboard/project/_/storage/buckets) page in the Dashboard.
2. Select the bucket that contains the file.
3. Select the file that you want to download.
4. Click **Download**.

## Add security rules [\#](https://supabase.com/docs/guides/storage/quickstart\#add-security-rules)

To restrict access to your files you can use either the Dashboard or SQL.

DashboardSQL

1. Go to the [Storage](https://supabase.com/dashboard/project/_/storage/buckets) page in the Dashboard.
2. Click **Policies** in the sidebar.
3. Click **Add Policies** in the `OBJECTS` table to add policies for Files. You can also create policies for Buckets.
4. Choose whether you want the policy to apply to downloads (SELECT), uploads (INSERT), updates (UPDATE), or deletes (DELETE).
5. Give your policy a unique name.
6. Write the policy using SQL.

* * *

Introduction to Supabase Storage - SupabaseTips - YouTube

Supabase

45.5K subscribers

[Introduction to Supabase Storage - SupabaseTips](https://www.youtube.com/watch?v=J9mTPY8rIXE)

Supabase

Search

Info

Shopping

Tap to unmute

If playback doesn't begin shortly, try restarting your device.

You're signed out

Videos you watch may be added to the TV's watch history and influence TV recommendations. To avoid this, cancel and sign in to YouTube on your computer.

CancelConfirm

Share

Include playlist

An error occurred while retrieving sharing information. Please try again later.

Watch later

Share

Copy link

Watch on

0:00

/ •Live

•

[Watch on YouTube](https://www.youtube.com/watch?v=J9mTPY8rIXE "Watch on YouTube")

Watch video guide

![Video guide preview](https://supabase.com/docs/_next/image?url=https%3A%2F%2Fimg.youtube.com%2Fvi%2FJ9mTPY8rIXE%2F0.jpg&w=3840&q=75&dpl=dpl_9xAnUGkSbk4dufV62sNRezafXykJ)

### Is this helpful?

NoYes

### On this page

[Concepts](https://supabase.com/docs/guides/storage/quickstart#concepts) [Files](https://supabase.com/docs/guides/storage/quickstart#files) [Folders](https://supabase.com/docs/guides/storage/quickstart#folders) [Buckets](https://supabase.com/docs/guides/storage/quickstart#buckets) [Create a bucket](https://supabase.com/docs/guides/storage/quickstart#create-a-bucket) [Upload a file](https://supabase.com/docs/guides/storage/quickstart#upload-a-file) [Download a file](https://supabase.com/docs/guides/storage/quickstart#download-a-file) [Add security rules](https://supabase.com/docs/guides/storage/quickstart#add-security-rules)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings