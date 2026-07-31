# Chapter 10: Event-Driven Programming

---

## Introduction

So far, many of your programs have been **call-driven**: you run a script, and each step happens because *your code* calls the next function—read a file, then process it, then print a result.

In this chapter you will learn **event-driven** programming. Work starts because something **happened**—a file appeared, a message arrived, a timer fired—and a separate piece of code **reacts**. The code that caused the change often does **not** call the reactor directly.

On AWS you will see one concrete form of this idea: uploading an object to **S3** can cause **Lambda** to run automatically.

---

## Chapter Goals

In this chapter you will learn:

- What **event-driven** programming means
- The roles of **producers**, **events**, and **consumers**
- How an **S3** object-created notification can invoke **Lambda**
- How to write a small **producer** that uploads JSON to S3 with boto3
- How to read useful fields from an S3 event
- Why an event often carries **metadata**, not the full payload—and when to call **`get_object`**

---

[← Back to Course Index](../table-of-contents.md)

## Contents

- Call-driven vs event-driven (10.1)
- Producers, events, and consumers (10.2)
- Everyday analogies (10.3)
- A cloud example: S3 notifies Lambda (10.4)
- Reading an S3 event (10.5)
- Metadata vs object contents (10.6)
- Wiring an S3 trigger (10.7)
- A small producer program (10.8)
- A small consumer example (10.9)
- Chapter summary (10.10)

---

## 10.1 Call-Driven vs Event-Driven

### Call-driven

In a typical script you control the order:

```text
open file → parse lines → print report
```

Function A finishes, then function B runs because A (or `main`) **called** B. Everything is one conversation your program is having with itself.

### Event-driven

In an event-driven design:

1. Something **changes** in the world (or in a system).
2. That change is described as an **event** (often a small packet of data).
3. One or more **handlers** run because they are **subscribed** to that kind of event.

The code that caused the change may have already finished. It does not wait for every reaction to complete.

```text
Call-driven:     do A, then do B, then do C   (you schedule every step)

Event-driven:    A happens → event is published → B and C react if they care
```

Both styles are valid. Event-driven shines when parts of a system should stay **independent**: uploaders should not need to know every program that later cares about new files.

---

## 10.2 Producers, Events, and Consumers

### Producer

A **producer** is whatever **creates** the change or the message.

Examples:

- A phone app that uploads a photo
- A cash-register program that saves a receipt file
- An AWS service that notices “a new object exists” and publishes a notification

Producers usually **hand off** and continue. They are not required to know who will react.

### Event

An **event** is a description of what happened. It is often JSON (or similar) with fields such as *when*, *where*, and *what kind* of change—not necessarily a full copy of a large file.

### Consumer (handler)

A **consumer** is code that **receives** the event and does work: resize an image, send an email, update a database, write a log line, and so on.

Many systems allow **several** consumers for the same kind of event. One upload might trigger a virus scan *and* a search-index update without the uploader calling either one.

| Role | Responsibility |
|------|----------------|
| Producer | Causes a change or sends a message |
| Event | Carries news about the change |
| Consumer | Reacts when that news arrives |

---

## 10.3 Everyday Analogies

These are not AWS features—they are mental models.

**Doorbell.** Someone presses the button (**producer**). The chime rings (**event** reaches the house). You walk to the door (**consumer**). The visitor does not personally walk into your kitchen to “call” you; the doorbell system connects the two sides.

**Email.** A classmate sends a message (**producer**). The mail server delivers it (**event / message**). Your inbox app shows a notification and you read it later (**consumer**). The sender does not run your email app.

**Library return slot.** A borrower drops a book in the slot (**producer**). Staff later scan returned books (**consumers**). The borrower does not walk to the catalog computer and update the database themselves.

The shared idea: **something happened**, **news traveled**, **someone reacted**—often asynchronously.

---

## 10.4 A Cloud Example: S3 Notifies Lambda

AWS services often publish events when their state changes. One common pattern:

1. A program **uploads** an object to an **S3** bucket (producer).
2. S3 emits an **ObjectCreated** event.
3. A **Lambda** function configured as a trigger **runs** and receives that event (consumer).

```text
Your script --put_object--> S3 --ObjectCreated--> Lambda
```

**Example scenario:** a small bakery’s tablet app uploads tonight’s menu as `menus/2026-07-29.json`. When the file lands, Lambda runs to **check** that required fields exist (`title`, `items`) and **print** a summary to the logs. The tablet app never imports or calls the Lambda function; S3 connects them.

Other triggers exist too (schedules, queues, HTTP APIs). This chapter focuses on **“a new object appeared in S3.”**

---

## 10.5 Reading an S3 Event

When S3 invokes Lambda, the `event` argument looks roughly like this (shortened):

```json
{
  "Records": [
    {
      "eventSource": "aws:s3",
      "eventName": "ObjectCreated:Put",
      "awsRegion": "us-east-1",
      "s3": {
        "bucket": {
          "name": "bakery-menus-example"
        },
        "object": {
          "key": "menus/2026-07-29.json",
          "size": 512
        }
      }
    }
  ]
}
```

Useful fields:

| Path | Meaning |
|------|---------|
| `record["s3"]["bucket"]["name"]` | Which bucket |
| `record["s3"]["object"]["key"]` | Which object (path/name inside the bucket) |
| `record["eventName"]` | What kind of change (for example `ObjectCreated:Put`) |

`event["Records"]` is a **list**. Always loop; AWS may deliver more than one record in a single invocation.

### URL-encoded keys

Keys may arrive **URL-encoded** (spaces and special characters). Decode before use:

```python
from urllib.parse import unquote_plus

key = unquote_plus(record["s3"]["object"]["key"])
```

---

## 10.6 Metadata vs Object Contents

An S3 event answers questions like:

- Which bucket?
- Which key?
- About how large is the object?
- Was this a Put (or another create type)?

It does **not** include the **bytes of the file** you uploaded (your menu JSON, a photo, a PDF). Think of the event as a **note pinned to the fridge**: “new file in drawer 3,” not the file itself.

If the consumer needs the contents, it must **fetch** them—for example with **`get_object`** (Chapter 9):

```python
import json
import boto3
from urllib.parse import unquote_plus

s3 = boto3.client("s3")

bucket = record["s3"]["bucket"]["name"]
key = unquote_plus(record["s3"]["object"]["key"])

response = s3.get_object(Bucket=bucket, Key=key)
text = response["Body"].read().decode("utf-8")
menu = json.loads(text)

print(menu["title"])
print(len(menu["items"]), "items")
```

**Rule of thumb:** the event tells you **where** to look; **`get_object`** (or another API) loads **what** was stored.

---

## 10.7 Wiring an S3 Trigger

High-level console steps:

1. Create a **Lambda** function (Python 3.9+). In this course, use execution role **`LabRole`**.
2. Open **Configuration → Triggers → Add trigger**.
3. Choose **S3**, select the bucket, and choose object-created events (for example **PUT** or all create events).
4. Optionally set a **prefix** (for example `menus/`) so only matching keys invoke the function.
5. Save. The console usually grants S3 permission to **invoke** the function.

If objects appear in the bucket but Lambda never runs, check that the trigger is listed and enabled, that the prefix matches your keys, and that S3 is allowed to invoke the function.

Configuration values (bucket names and similar) belong in **environment variables**, not hard-coded strings—see the [Lambda special topic](../special-topics/lambda.md).

---

## 10.8 A Small Producer Program

The **producer** runs on your computer. It builds a dictionary, turns it into JSON, and uploads it to S3 with **`put_object`**. It does **not** call Lambda.

Use Academy credentials in `.aws` as in Chapter 9.1, and install boto3 locally (`pip install boto3`).

**Scenario:** a bakery tablet uploads tonight’s menu under `menus/`.

```python
import json
import os
import uuid

import boto3

BUCKET = os.environ.get("S3_BUCKET")
if not BUCKET:
    raise ValueError("Set S3_BUCKET to your bucket name")

menu = {
    "title": "Tuesday Evening",
    "items": ["soup", "bread", "pie"],
}

menu_id = str(uuid.uuid4())
key = f"menus/{menu_id}.json"

s3 = boto3.client("s3")
s3.put_object(
    Bucket=BUCKET,
    Key=key,
    Body=json.dumps(menu),
    ContentType="application/json",
)

print("Uploaded:", key)
```

Notes:

- Set **`S3_BUCKET`** in your environment (or in PyCharm’s run configuration) to your real bucket name.
- **`uuid`** makes each upload a **new** key so you do not overwrite the previous menu file.
- **`json.dumps`** turns the dictionary into a JSON text string for the object body.
- After this script prints `Uploaded: ...`, its job is done. If an S3 trigger is attached, **Lambda** may run next—but this program never imports or invokes it.

---

## 10.9 A Small Consumer Example

**Scenario:** when a JSON menu is uploaded under `menus/`, Lambda loads it and logs the title and item count. No database is required for this lesson—the point is the **event → react** pattern.

```python
import json
import logging
from urllib.parse import unquote_plus

import boto3

logger = logging.getLogger()
logger.setLevel(logging.INFO)

s3 = boto3.client("s3")


def lambda_handler(event, context):
    for record in event.get("Records", []):
        try:
            bucket = record["s3"]["bucket"]["name"]
            key = unquote_plus(record["s3"]["object"]["key"])
            logger.info("New object: s3://%s/%s", bucket, key)

            response = s3.get_object(Bucket=bucket, Key=key)
            menu = json.loads(response["Body"].read().decode("utf-8"))

            title = menu.get("title")
            items = menu.get("items")
            if not title or not isinstance(items, list):
                logger.warning("Unexpected menu shape for key=%s", key)
                continue

            logger.info("Menu %r has %d items", title, len(items))

        except Exception:
            logger.exception("Could not process record")

    return {"ok": True}
```

Run the producer from Section 10.8, then check **CloudWatch Logs** for the Lambda function. You should see the menu title and item count if the trigger and permissions are set correctly.

Handle **each** record in **`try`/`except`** so one bad file does not hide problems for the others, and log enough context (bucket, key) to debug in CloudWatch.

---

## 10.10 Chapter Summary

- **Event-driven** programs react to **things that happened**, instead of only to a fixed call sequence inside one script.
- A **producer** causes a change; an **event** carries the news; a **consumer** reacts.
- A local producer can upload with **`put_object`**; it does not need to call the consumer.
- On AWS, **S3 ObjectCreated** can invoke **Lambda** without the uploader calling Lambda.
- The S3 event gives **bucket** and **key** (and related metadata). Load file bytes with **`get_object`** when you need the contents.
- Decode keys with **`unquote_plus`**, loop over **`Records`**, and handle errors per record.

---

## Review

- [ ] I can contrast call-driven and event-driven styles in my own words.
- [ ] I can name the roles of producer, event, and consumer.
- [ ] I can explain why a producer might not call the consumer directly.
- [ ] I can write a small local script that uploads JSON to S3 with `put_object`.
- [ ] I can find bucket and key in an S3 Lambda event.
- [ ] I understand that the event is usually metadata, not the full object body.
- [ ] I can sketch how to attach an S3 trigger and write a handler that logs or processes a new object.
