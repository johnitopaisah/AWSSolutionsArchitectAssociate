# ☁️ AWS S3 Bash Scripting Lab Notes

This lab provides a structured guide to managing AWS S3 buckets and objects using **Bash scripts** and the **AWS CLI**. You'll create, list, delete buckets and objects, and sync files — all programmatically.

---

## ✅ Prerequisites

- AWS CLI installed and configured (`aws configure`)
- GitPod or any Bash-enabled environment
- `jq` installed (for JSON processing):  
  ```bash
  sudo apt-get install jq
  ```
- Basic knowledge of Bash scripting

---

## 📁 Step 1: Set Up Environment

### 1.1 Clone/Explore Lab Reference

Check the reference GitHub repo:  
🔗 https://github.com/ExamProCo/AWS-Examples/tree/main/s3/bash-scripts

### 1.2 Create Directories

```bash
mkdir -p S3/bash-scripts
cd S3/bash-scripts
```

### 1.3 Make Scripts Executable

```bash
chmod u+x *.sh
```

---

## 🪣 Step 2: Create and Delete Buckets

### 2.1 `create_bucket.sh`

```bash
#!/bin/bash
set -e
if [ -z "$1" ]; then
    echo "No bucket name provided. Usage: $0 bucket-name"
    exit 1
fi
BUCKET_NAME=$1
REGION=${2:-us-east-1}
aws s3api create-bucket --bucket "$BUCKET_NAME" --create-bucket-configuration LocationConstraint="$REGION"
```

### 2.2 `delete_bucket.sh`

```bash
#!/bin/bash
set -e
if [ -z "$1" ]; then
    echo "No bucket name provided. Usage: $0 bucket-name"
    exit 1
fi
BUCKET_NAME=$1
aws s3api delete-bucket --bucket "$BUCKET_NAME"
```

---

## 📂 Step 3: Sync Files to Bucket

### 3.1 `generate_files.sh`

```bash
#!/bin/bash
set -e
OUTPUT_DIR="./temp"
mkdir -p $OUTPUT_DIR
rm -rf $OUTPUT_DIR/*
NUM_FILES=$((RANDOM % 6 + 5))
for i in $(seq 1 $NUM_FILES); do
    FILE_NAME="$OUTPUT_DIR/file_$i.txt"
    head -c 100 </dev/urandom > $FILE_NAME
    echo "Created $FILE_NAME"
done
```

### 3.2 `sync_files.sh`

```bash
#!/bin/bash
set -e
if [ -z "$1" ]; then
    echo "No bucket name provided. Usage: $0 bucket-name"
    exit 1
fi
BUCKET_NAME=$1
FILE_PREFIX=${2:-files}
aws s3 sync ./temp s3://$BUCKET_NAME/$FILE_PREFIX
```

---

## 📋 Step 4: List and Delete Objects

### 4.1 `list_objects.sh`

```bash
#!/bin/bash
set -e
if [ -z "$1" ]; then
    echo "No bucket name provided. Usage: $0 bucket-name"
    exit 1
fi
BUCKET_NAME=$1
aws s3api list-objects --bucket "$BUCKET_NAME" --query 'Contents[].{Key: Key, Size: Size}'
```

### 4.2 `delete_objects.sh`

```bash
#!/bin/bash
set -e
if [ -z "$1" ]; then
    echo "No bucket name provided. Usage: $0 bucket-name"
    exit 1
fi
BUCKET_NAME=$1
OBJECTS=$(aws s3api list-objects --bucket "$BUCKET_NAME" --query 'Contents[].{Key: Key}' --output json)
if [ "$OBJECTS" == "null" ]; then
    echo "Bucket is empty"
    exit 0
fi
echo '{"Objects":' > delete.json
echo $OBJECTS | jq '.' >> delete.json
echo '}' >> delete.json
aws s3api delete-objects --bucket "$BUCKET_NAME" --delete file://delete.json
rm delete.json
```

---

## 🛠️ Step 5: Additional Utility Scripts

### 5.1 `getnewestbucket.sh`

```bash
#!/bin/bash
aws s3api list-buckets --query 'Buckets | sort_by(@, &CreationDate) | [-1:].Name' --output text
```

### 5.2 `list_buckets.sh`

```bash
#!/bin/bash
aws s3 ls
```

---

## 🚀 Step 6: Running the Scripts

Make sure you're in the `S3/bash-scripts` directory.

```bash
# 6.1 Generate random files
./generate_files.sh

# 6.2 Create a new bucket
./create_bucket.sh my-new-bucket-20250408 us-east-1

# 6.3 Sync files to bucket
./sync_files.sh my-new-bucket-20250408 files

# 6.4 List objects in bucket
./list_objects.sh my-new-bucket-20250408

# 6.5 Delete all objects in bucket
./delete_objects.sh my-new-bucket-20250408

# 6.6 Delete the bucket
./delete_bucket.sh my-new-bucket-20250408
```

---

## ✅ Conclusion

This lab demonstrates how Bash scripting can powerfully automate AWS S3 operations. By mastering these scripts, you'll be equipped to manage cloud storage efficiently — a critical skill for DevOps and Cloud Engineers.

Happy Scripting! ⚡
