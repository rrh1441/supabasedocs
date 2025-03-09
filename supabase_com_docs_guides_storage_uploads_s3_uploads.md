Storage

# S3 Uploads

## Learn how to upload files to Supabase Storage using S3.

* * *

You can use the S3 protocol to upload files to Supabase Storage. To get started with S3, see the [S3 setup guide](https://supabase.com/docs/guides/storage/s3/authentication).

The S3 protocol supports file upload using:

- A single request
- Multiple requests via Multipart Upload

## Single request uploads [\#](https://supabase.com/docs/guides/storage/uploads/s3-uploads\#single-request-uploads)

The `PutObject` action uploads the file in a single request. This matches the behavior of the Supabase SDK [Standard Upload](https://supabase.com/docs/guides/storage/uploads/standard-uploads).

Use `PutObject` to upload smaller files, where retrying the entire upload won't be an issue. The maximum file size on paid plans is 50 GB.

For example, using JavaScript and the `aws-sdk` client:

`
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3'
const s3Client = new S3Client({...})
const file = fs.createReadStream('path/to/file')
const uploadCommand = new PutObjectCommand({
Bucket: 'bucket-name',
Key: 'path/to/file',
Body: file,
ContentType: 'image/jpeg',
})
await s3Client.send(uploadCommand)
`

## Multipart uploads [\#](https://supabase.com/docs/guides/storage/uploads/s3-uploads\#multipart-uploads)

Multipart Uploads split the file into smaller parts and upload them in parallel, maximizing the upload speed on a fast network. When uploading large files, this allows you to retry the upload of individual parts in case of network issues.

This method is preferable over [Resumable Upload](https://supabase.com/docs/guides/storage/uploads/resumable-uploads) for server-side uploads, when you want to maximize upload speed at the cost of resumability. The maximum file size on paid plans is 50 GB.

### Upload a file in parts [\#](https://supabase.com/docs/guides/storage/uploads/s3-uploads\#upload-a-file-in-parts)

Use the `Upload` class from an S3 client to upload a file in parts. For example, using JavaScript:

`
import { S3Client } from '@aws-sdk/client-s3'
import { Upload } from '@aws-sdk/lib-storage'
const s3Client = new S3Client({...})
const file = fs.createReadStream('path/to/very-large-file')
const upload = new Upload(s3Client, {
Bucket: 'bucket-name',
Key: 'path/to/file',
ContentType: 'image/jpeg',
Body: file,
})
await uploader.done()
`

### Aborting multipart uploads [\#](https://supabase.com/docs/guides/storage/uploads/s3-uploads\#aborting-multipart-uploads)

All multipart uploads are automatically aborted after 24 hours. To abort a multipart upload before that, you can use the [`AbortMultipartUpload`](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html) action.

### Is this helpful?

NoYes

### On this page

[Single request uploads](https://supabase.com/docs/guides/storage/uploads/s3-uploads#single-request-uploads) [Multipart uploads](https://supabase.com/docs/guides/storage/uploads/s3-uploads#multipart-uploads) [Upload a file in parts](https://supabase.com/docs/guides/storage/uploads/s3-uploads#upload-a-file-in-parts) [Aborting multipart uploads](https://supabase.com/docs/guides/storage/uploads/s3-uploads#aborting-multipart-uploads)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings