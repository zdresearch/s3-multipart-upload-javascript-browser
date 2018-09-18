# AWS S3 Multipart Upload in Browser
## What is this?
Uploading files from the browser directly to S3 is needed in many applications. Fine-grained authorization is handled by the server, and the browser only handles file upload. This library does that.

## What's so special?
Unfortunately S3 does not allow uploading files larger than 5GB in one chunk, and  all the examples in AWS docs either support one chunk, or support **multipart uploads** only on the server.

As we don't want to proxy the upload traffic to a server (which negates the whole purpose of using S3), we need an S3 multipart upload solution from the browser.

The code by [s3-multipart-upload-browser](https://github.com/ienzam/s3-multipart-upload-browser) does some of that, but many of its features don't work. Most notably, it does not support AWS API V3. 

## Features

This uploader supports upload prefixes (uploading only in a certain directory), fine-grained feedback, parallelized chunks, cancellation of the upload (and removal of the parts on S3), automated parts cleanup, drag & drop, progress bar and chunk size management.

## Requirements
- The browser Javascript code relies on jQuery. It also requires a browser with Blob, File and XHR2 support (most do since 2012).
- The server-side code is in PHP, but is straightforward enough to port to any language and is less than 200 LOC.
- If using with PHP, it needs AWS credentials and the AWS PHP SDK V3+ as well.

That's all!

## Installation
Just put all files in a web directory, set the AWS credentials in index.php or in keys.php file, set your bucket's CORS config (can be done via a function in the code too), and you're ready.

## Notes
I strongly recommend setting your S3 bucket for [auto-removal of unfinished multipart upload parts](https://aws.amazon.com/blogs/aws/s3-lifecycle-management-update-support-for-multipart-uploads-and-delete-markers/). Otherwise, any incomplete upload will leave useless files on your bucket, for which you will be charged.

I also suggest using the largest supported chunk size (5GB) to make the XHR connections minimal. Uploading a 200GB file with 5GB chunks needs 40 parts, but using 100MB chunks requires 2000. Authorizing parts with AWS is both slow and pricy. 

## License
This work is released under MIT license.
