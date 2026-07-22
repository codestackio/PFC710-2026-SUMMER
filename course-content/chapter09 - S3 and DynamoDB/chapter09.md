# Chapter 9: S3 and DynamoDB

---

## Introduction

In earlier chapters you wrote Python programs that run on your computer. You also used **AWS Lambda** and **S3** in Project 1. In this chapter you will learn how to call AWS services **from a local Python script** using **boto3**, how to **upload and download** files in S3, and how to **store records** in **DynamoDB**.

These skills prepare you for pipelines where one program writes data to the cloud and another program (or Lambda) reads and saves it.

---

## Chapter Goals

In this chapter you will learn:

- To store AWS credentials in your home folder (`.aws` on Windows)
- To install and use **boto3**, the AWS SDK for Python
- To upload and download JSON objects in **S3**
- To create unique object keys with **uuid**
- To create a **DynamoDB** table with a **partition key**
- To write and read items with **`put_item`** and **`get_item`**

---

[← Back to Course Index](../table-of-contents.md)

## Contents

- AWS credentials on your computer (9.1)
- Using boto3 (9.2)
- Amazon S3 from Python (9.3)
- Amazon DynamoDB (9.4)
- Chapter summary (9.5)

---

## 9.1 AWS Credentials on Your Computer

To call AWS from a script on **your machine**, Python (and boto3) look for login details in a folder in your **home directory** called `.aws`.

On **Windows**, that folder is usually:

```text
C:\Users\YourUsername\.aws\
```

Replace `YourUsername` with your Windows user name. Inside it you will use two files:

| File | Purpose |
|------|---------|
| `credentials` | Access key, secret key, and session token |
| `config` | Default region (for example `us-east-1`) |

### Where to get the values

1. Start your lab in **AWS Academy**.
2. Open **AWS Details** (or the credential export for your lab).
3. Copy **Access Key**, **Secret Access Key**, and **Session Token**.
4. Note the **region** your lab uses (often `us-east-1`).

Academy labs use **temporary** credentials. You **must** include the **session token**. Without it, calls fail even if the access key and secret look correct.

### Create the `.aws` folder (Windows)

1. Open **File Explorer**.
2. Go to `C:\Users\YourUsername\`.
3. If you do not see a folder named `.aws`, create one:
   - Right-click → **New → Folder** → name it `.aws`
   - If Windows complains about the leading dot, create the folder from **Command Prompt**:

```text
mkdir %USERPROFILE%\.aws
```

### Create the `credentials` file

1. In `C:\Users\YourUsername\.aws\`, create a file named **`credentials`** (no file extension — not `credentials.txt`).
2. Open it in Notepad and paste this (replace the placeholder values with your lab values):

```ini
[default]
aws_access_key_id = paste-access-key-here
aws_secret_access_key = paste-secret-here
aws_session_token = paste-session-token-here
```

3. Save the file.

**Tip:** In Notepad, use **File → Save As**, set **Save as type** to **All Files**, and name the file exactly `credentials` so Windows does not add `.txt`.

### Create the `config` file

In the same `.aws` folder, create a file named **`config`** (again, no extension) with:

```ini
[default]
region = us-east-1
```

Use your lab’s region if it is different.

### After that

Run your Python script normally (for example from PyCharm). boto3 will read `[default]` from these files automatically. You do **not** need to set credential environment variables in PyCharm for this course.

**Important:**

- Do **not** put real keys in your `.py` files.
- Do **not** commit the `.aws` folder (or keys) to git.
- Session tokens **expire**. When calls start failing, open **AWS Details** again, copy fresh values, and update `credentials`.

---

## 9.2 Using boto3

**boto3** is the official AWS library for Python. It lets your code talk to services such as S3 and DynamoDB.

### Install boto3 (local projects)

In your project virtual environment (PyCharm terminal is fine):

```bash
pip install boto3
```

**Note:** Inside **AWS Lambda**, boto3 is already installed. You only need `pip install boto3` for scripts that run on your computer.

### Create a client

A **client** is how you call one AWS service:

```python
import boto3

s3 = boto3.client("s3")
```

Later you will use `boto3.resource("dynamodb")` for DynamoDB tables. Both styles are fine for this course.

---

## 9.3 Amazon S3 from Python

**Amazon S3** stores **objects** (files) in a **bucket**. Each object has a **key** (the path/name inside the bucket), for example:

```text
bills/550e8400-e29b-41d4-a716-446655440000.json
```

You used S3 from Lambda in Project 1. Here you will use it from a **local script**.

### Upload JSON with `put_object`

```python
import json
import boto3

s3 = boto3.client("s3")

bucket_name = "your-bucket-name"
key = "data/example.json"

bill = {
    "accountNumber": 45231,
    "billNumber": 187,
    "amountDue": 42.5,
}

s3.put_object(
    Bucket=bucket_name,
    Key=key,
    Body=json.dumps(bill),
    ContentType="application/json",
)

print("Uploaded:", key)
```

- **`Bucket`**: your bucket name.
- **`Key`**: where the object lives inside the bucket.
- **`Body`**: the file contents (here, a JSON string).
- **`json.dumps`**: turns a Python dictionary into a JSON text string.

### Unique keys with `uuid`

If every upload used the same key, the new file would **overwrite** the old one. Use a **UUID** (a random unique id) so each upload gets its own name:

```python
import uuid

bill_id = str(uuid.uuid4())
key = f"bills/{bill_id}.json"
```

Example key:

```text
bills/550e8400-e29b-41d4-a716-446655440000.json
```

### Download JSON with `get_object`

Uploading puts the file in S3. To **read** it back:

```python
import json
import boto3

s3 = boto3.client("s3")

response = s3.get_object(
    Bucket="your-bucket-name",
    Key="bills/550e8400-e29b-41d4-a716-446655440000.json",
)

body_text = response["Body"].read().decode("utf-8")
bill = json.loads(body_text)

print(bill["accountNumber"])
print(bill["billNumber"])
print(bill["amountDue"])
```

Steps:

1. Call **`get_object`** with bucket and key.
2. Read **`response["Body"]`** and decode it to a string.
3. Use **`json.loads`** to turn the string back into a dictionary.

**Remember:** An S3 **event message** (you will see these in the next chapter) often tells you the **bucket and key**, not the file contents. Your code still needs **`get_object`** to download the JSON.

### Use environment variables for the bucket name

Do not hard-code the bucket name if your assignment says to use configuration:

```python
import os

bucket_name = os.environ.get("S3_BUCKET")
if not bucket_name:
    raise ValueError("S3_BUCKET is not set")
```

Set `S3_BUCKET` in PyCharm (or in Lambda’s environment variables when the code runs in the cloud).

---

## 9.4 Amazon DynamoDB

**Amazon DynamoDB** is a managed database on AWS. You create a **table**, then store **items** (records) in it.

Think of it like this:

| Idea | Meaning |
|------|---------|
| **Table** | A collection of items (like a spreadsheet or one database table) |
| **Item** | One record (like one row) |
| **Attribute** | A field on an item (like a column value) |
| **Partition key** | The required unique id for each item |

In this course you will use a table with **only a partition key** (no other key fields).

### Create a table in the console

1. Open **DynamoDB** in the AWS Console.
2. Click **Create table**.
3. **Table name:** for example `bills`.
4. **Partition key:** for example `billId`, type **String**.
5. Leave other options at the defaults that your lab recommends (often **On-demand** capacity).
6. Create the table and wait until status is **Active**.

Use the **same region** as your other resources when possible.

### What the partition key means

- Every item **must** include the partition key attribute (for example `billId`).
- Two items **cannot** share the same partition key value.
- If you call **`put_item`** again with the **same** partition key, DynamoDB **replaces** the old item with the new one.

### Write an item with `put_item`

```python
import os
from decimal import Decimal

import boto3

table_name = os.environ.get("DYNAMODB_TABLE")
if not table_name:
    raise ValueError("DYNAMODB_TABLE is not set")

table = boto3.resource("dynamodb").Table(table_name)

table.put_item(
    Item={
        "billId": "550e8400-e29b-41d4-a716-446655440000",
        "accountNumber": 45231,
        "billNumber": 187,
        "amountDue": Decimal("42.5"),
        "s3Bucket": "your-bucket-name",
        "s3Key": "bills/550e8400-e29b-41d4-a716-446655440000.json",
    }
)

print("Saved item")
```

Notes:

- The attribute name **`billId`** must match the partition key you chose when creating the table.
- Extra attributes (`accountNumber`, `amountDue`, and so on) are allowed.
- Common Python types map in a simple way: `str` → string, `int` → number.
- **Do not use `float`.** boto3 rejects floats for DynamoDB numbers. Use `Decimal` instead (for example `Decimal("42.5")`). Pass the value as a **string** to `Decimal` so you keep exact decimal precision.

### Read an item with `get_item`

```python
response = table.get_item(
    Key={"billId": "550e8400-e29b-41d4-a716-446655440000"}
)

item = response.get("Item")
if item is None:
    print("Not found")
else:
    print(item)
```

You look up by the **partition key** only.

### Handle errors

Wrap AWS calls in **`try`/`except`** and print or log a clear message:

```python
try:
    table.put_item(Item={...})
except Exception as e:
    print("DynamoDB error:", e)
```

---

## 9.5 Chapter Summary

### Credentials and boto3

- On Windows, put Academy credentials in `%USERPROFILE%\.aws\credentials` (include **`aws_session_token`**).
- Put the region in `%USERPROFILE%\.aws\config`.
- Install **boto3** locally with `pip install boto3`.
- Never commit secrets to git.

### S3

- **`put_object`** uploads; **`get_object`** downloads.
- Use a **key** like `bills/<uuid>.json` so each upload is unique.
- **`json.dumps`** / **`json.loads`** convert between dictionaries and JSON text.

### DynamoDB

- A table stores **items**.
- You need a **partition key** (for this course, that is enough).
- **`put_item`** writes (or replaces) an item; **`get_item`** reads by partition key.
- Prefer environment variables for bucket and table names.

---

## Review

- [ ] I can put Academy credentials in `.aws\credentials` and the region in `.aws\config`.
- [ ] I can install boto3 and create an S3 client.
- [ ] I can upload a JSON dictionary to S3 with `put_object`.
- [ ] I can build a unique key with `uuid.uuid4()`.
- [ ] I can download and parse JSON with `get_object` and `json.loads`.
- [ ] I can create a DynamoDB table with a **partition key**.
- [ ] I can write an item with `put_item` and read it with `get_item`.
- [ ] I read bucket and table names from environment variables when required.
