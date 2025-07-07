# Error Handling in Shell Scripting with AWS S3 Bucket Creation
## Explanation:
Error handling in shell scripting ensures scripts behave predictably even when issues arise. In this project, we implement error handling while creating S3 buckets for different departments. The script uses a `for` loop to iterate through department names and attempts to create a uniquely named S3 bucket for each. Before creating a bucket, the script checks if it already exists using `aws s3api head-bucket`. If it exists, an informative message is displayed. If not, the script attempts to create the bucket and checks the success using `$?`, printing appropriate messages. This ensures that errors such as duplicate buckets or failed API calls are gracefully handled.

## Shell Script: `create_s3_buckets.sh`

```
#!/bin/bash

# Function to create S3 buckets for different departments
create_s3_buckets() {
    company="datawise"
    departments=("Marketing" "Sales" "HR" "Operations" "Media")
    region="us-east-1"

    for department in "${departments[@]}"; do
        bucket_name="${company,,}-${department,,}-data-bucket"  # Convert to lowercase for S3 naming compliance

        # Check if the bucket already exists
        if aws s3api head-bucket --bucket "$bucket_name" 2>/dev/null; then
            echo "S3 bucket '$bucket_name' already exists. Skipping creation."
        else
            # Try to create the bucket
            aws s3api create-bucket --bucket "$bucket_name" --region "$region" --create-bucket-configuration LocationConstraint="$region"
            
            if [ $? -eq 0 ]; then
                echo "S3 bucket '$bucket_name' created successfully."
            else
                echo "Error: Failed to create S3 bucket '$bucket_name'."
            fi
        fi
    done
}

# Call the function
create_s3_buckets
```
## Key Features Demonstrated:

* Use of `for` loop to iterate through departments.
* Use of `if` condition with `aws s3api head-bucket` to check for existence.
* Use of `$?` to capture command success/failure.
* Informative error messages.
* Follows AWS bucket naming rules (lowercase).
