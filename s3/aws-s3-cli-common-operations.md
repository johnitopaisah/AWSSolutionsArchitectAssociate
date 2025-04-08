# 📦 AWS CLI S3 Common Commands

A curated list of 50 useful AWS CLI commands to manage Amazon S3 buckets, objects, permissions, and advanced features. Use these to automate your S3 workflow effectively.

---

## 🔹 Bucket Operations

```bash
# 1. Create a bucket
aws s3 mb s3://my-bucket

# 2. List all buckets
aws s3 ls

# 3. Delete a bucket
aws s3 rb s3://my-bucket

# 4. Delete a non-empty bucket
aws s3 rb s3://my-bucket --force

# 5. Enable versioning
aws s3api put-bucket-versioning --bucket my-bucket --versioning-configuration Status=Enabled

# 6. Suspend versioning
aws s3api put-bucket-versioning --bucket my-bucket --versioning-configuration Status=Suspended

# 7. Get bucket versioning status
aws s3api get-bucket-versioning --bucket my-bucket

# 8. Enable server-side encryption (SSE-S3)
aws s3api put-bucket-encryption --bucket my-bucket --server-side-encryption-configuration file://encryption.json

# 9. Enable static website hosting
aws s3 website s3://my-bucket/ --index-document index.html --error-document error.html

# 10. Get bucket policy
aws s3api get-bucket-policy --bucket my-bucket

# 11. Put a bucket policy
aws s3api put-bucket-policy --bucket my-bucket --policy file://policy.json

# 12. Delete a bucket policy
aws s3api delete-bucket-policy --bucket my-bucket

# 13. Enable CORS configuration
aws s3api put-bucket-cors --bucket my-bucket --cors-configuration file://cors.json

# 14. Get bucket ACL
aws s3api get-bucket-acl --bucket my-bucket

# 15. Put bucket ACL (e.g., public-read)
aws s3api put-bucket-acl --bucket my-bucket --acl public-read
```


---

## 🔹 Object Operations
```bash
# 16. Upload a file
aws s3 cp file.txt s3://my-bucket/

# 17. Download a file
aws s3 cp s3://my-bucket/file.txt .

# 18. Upload a directory recursively
aws s3 cp my-folder/ s3://my-bucket/ --recursive

# 19. Download a directory
aws s3 cp s3://my-bucket/ my-folder/ --recursive

# 20. List objects in a bucket
aws s3 ls s3://my-bucket/

# 21. List objects with a prefix
aws s3 ls s3://my-bucket/my-folder/

# 22. Delete an object
aws s3 rm s3://my-bucket/file.txt

# 23. Delete multiple files
aws s3 rm s3://my-bucket/my-folder/ --recursive

# 24. Sync local folder to S3
aws s3 sync ./local-folder s3://my-bucket/

# 25. Sync S3 to local
aws s3 sync s3://my-bucket/ ./local-folder

# 26. Copy an object within S3
aws s3 cp s3://source-bucket/file.txt s3://dest-bucket/file.txt

# 27. Move an object
aws s3 mv s3://my-bucket/old.txt s3://my-bucket/new.txt

# 28. Make object public
aws s3api put-object-acl --bucket my-bucket --key file.txt --acl public-read

# 29. Get object ACL
aws s3api get-object-acl --bucket my-bucket --key file.txt

# 30. Get object metadata
aws s3api head-object --bucket my-bucket --key file.txt

```

---
## 🔹 Versioning and Object Management
```bash
# 31. List object versions
aws s3api list-object-versions --bucket my-bucket

# 32. Delete specific version
aws s3api delete-object --bucket my-bucket --key file.txt --version-id versionId123

# 33. Restore a previous version
aws s3 cp s3://my-bucket/file.txt --version-id versionId123 file.txt

# 34. Enable MFA delete (requires versioning)
aws s3api put-bucket-versioning --bucket my-bucket \
--versioning-configuration Status=Enabled,MFADelete=Enabled \
--mfa "arn:aws:iam::account:mfa/user mfa-code"
```

---
## 🔹 Presigned URLs and Multipart Upload
```bash
# 35. Generate presigned URL
aws s3 presign s3://my-bucket/file.txt

# 36. Generate presigned URL with expiration (600 seconds)
aws s3 presign s3://my-bucket/file.txt --expires-in 600

# 37. Start multipart upload
aws s3api create-multipart-upload --bucket my-bucket --key bigfile.zip

# 38. Upload a part
aws s3api upload-part --bucket my-bucket --key bigfile.zip --part-number 1 \
--body part1.zip --upload-id <UploadId>

# 39. List multipart uploads
aws s3api list-multipart-uploads --bucket my-bucket

# 40. Complete multipart upload
aws s3api complete-multipart-upload --bucket my-bucket --key bigfile.zip \
--upload-id <UploadId> --multipart-upload file://parts.json

# 41. Abort multipart upload
aws s3api abort-multipart-upload --bucket my-bucket --key bigfile.zip --upload-id <UploadId>
```

---
## 🔹 Access, Logging, and Security
```bash
# 42. Enable access logging
aws s3api put-bucket-logging --bucket my-bucket --bucket-logging-status file://logging.json

# 43. Get logging status
aws s3api get-bucket-logging --bucket my-bucket

# 44. Enable Object Lock (for compliance)
aws s3api put-object-lock-configuration --bucket my-bucket \
--object-lock-configuration file://lock.json
```

---
## 🔹 Lifecycle, Tagging, and Inventory
```bash
# 45. Set lifecycle policy
aws s3api put-bucket-lifecycle-configuration --bucket my-bucket \
--lifecycle-configuration file://lifecycle.json

# 46. Get lifecycle policy
aws s3api get-bucket-lifecycle-configuration --bucket my-bucket

# 47. Add tags to a bucket
aws s3api put-bucket-tagging --bucket my-bucket \
--tagging 'TagSet=[{Key=env,Value=dev}]'

# 48. Get bucket tags
aws s3api get-bucket-tagging --bucket my-bucket

# 49. Delete bucket tags
aws s3api delete-bucket-tagging --bucket my-bucket

# 50. Enable S3 Inventory
aws s3api put-bucket-inventory-configuration --bucket my-bucket \
--id my-inventory --inventory-configuration file://inventory.json
```