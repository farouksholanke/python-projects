
# S3 Object Organizer with AWS Lambda and Boto3 📦

This project is a serverless Python-based AWS Lambda function that automates the process of organizing objects in an Amazon S3 bucket. Using the Boto3 library, the script groups S3 objects by their creation date into dynamically created folders, making it easier to maintain and manage S3 storage.


## What the Code Does
1. **Lists All S3 Objects**: The Lambda function connects to an Amazon S3 bucket and retrieves a list of all objects within it.
2. **Checks for a Daily Folder**: If a folder for the current date (in `YYYYMMDD` format) does not exist in the bucket, the function creates it.
3. **Moves Objects into Date-Based Folders**:
    - Objects are categorized based on their creation date.
    - If an object was created on the current date, it is moved to the corresponding folder by copying it into the folder and deleting the original object.

This ensures that the S3 bucket is always organized, with objects grouped into folders by their creation date.
## Features
- **Dynamic Folder Creation**: Automatically creates a folder for the current date if it doesn't already exist.
- **Efficient Organization**: Moves objects into folders based on their creation date.
- **Automation**: Runs automatically when triggered by an event (e.g., an S3 event or a CloudWatch schedule).
- **Cost-Effective**: Utilizes AWS Lambda for a pay-as-you-go serverless approach.
## How It Works
**Core Components**

- **Amazon S3**:
    
    - Stores the objects to be organized.
    - New folders are created dynamically within the bucket.
- **AWS Lambda**:
    
    - Executes the Boto3-based script to list, move, and delete objects in the S3 bucket.
- **Python Boto3**:
    
    - The AWS SDK for Python is used to interact with the S3 bucket (listing, creating folders, copying, and deleting objects).
## Code Highlights

**Check and Create Folder:**
```
directory_name = todays_date + "/"
if directory_name not in get_all_s3_objects_and_folder_names:
    s3_client.put_object(Bucket=bucket_name, Key=(directory_name))
```
This creates a folder for the current date if it doesn't exist.

**Move Objects to Date-Based Folders:**
```
if object_creation_date == directory_name and "/" not in object_name:
    s3_client.copy_object(Bucket=bucket_name, CopySource=bucket_name+"/"+object_name, Key=directory_name+object_name)
    s3_client.delete_object(Bucket=bucket_name, Key=object_name)
```
Copies the object into the folder and deletes the original, effectively "moving" the object.
## Prerequisites
- Python 3.x
- Boto3 installed (if running locally)
- AWS account with appropriate permissions

## Example Use Case
Suppose you upload files daily to an S3 bucket. Over time, it becomes difficult to manage these objects. This Lambda function automatically groups the objects into folders based on their creation date, helping you maintain a clean and organized structure.
## IAM Policy for Lambda Role
Below is a sample IAM policy to grant the Lambda function necessary permissions for S3 operations:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:PutObject",
                "s3:CopyObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::zayd-organise-s3-objects",
                "arn:aws:s3:::zayd-organise-s3-objects/*"
            ]
        }
    ]
}
```

## Conclusion
This project demonstrates how Boto3 can simplify AWS resource management. The Lambda function provides an automated way to organize S3 objects, making storage management efficient and scalable. It's a great example of combining serverless computing and Python to solve real-world problems.
