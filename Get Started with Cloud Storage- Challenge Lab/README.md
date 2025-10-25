## copy the following script and paste in SSH terminal
```
#!/bin/bash

# Define color variables
PURPLE_TEXT=$'\033[0;35m'
GOLD_TEXT=$'\033[0;33m'
TEAL_TEXT=$'\033[0;36m'
LIME_TEXT=$'\033[0;92m'
MAROON_TEXT=$'\033[0;91m'
NAVY_TEXT=$'\033[0;94m'
WHITE_TEXT=$'\033[0;97m'
RESET_FORMAT=$'\033[0m'
BOLD_TEXT=$'\033[1m'

clear

echo "${TEAL_TEXT}${BOLD_TEXT}Please enter the required details when prompted.${RESET_FORMAT}"
echo

# Function to run form 1 code
run_form_1() {
    export BUCKET="$(gcloud config get-value project)"        

    gsutil mb -p $BUCKET gs://$Bucket_1
    gsutil retention set 30s gs://$Bucket_2
    echo "Cloud Storage Demo" > sample.txt
    gsutil cp sample.txt gs://$Bucket_3/
    echo "${LIME_TEXT}${BOLD_TEXT}Form 1 execution completed successfully!${RESET_FORMAT}"
}

# Function to run form 2 code
run_form_2() {
    gsutil mb -c nearline gs://$Bucket_1
    gcloud alpha storage buckets update gs://$Bucket_2 --no-uniform-bucket-level-access
    gsutil acl ch -u $USER_EMAIL:OWNER gs://$Bucket_2
    gsutil rm gs://$Bucket_2/sample.txt
    echo "Cloud Storage Demo" > sample.txt
    gsutil cp sample.txt gs://$Bucket_2
    gsutil acl ch -u allUsers:R gs://$Bucket_2/sample.txt
    gcloud storage buckets update gs://$Bucket_3 --update-labels=key=value
    echo "${LIME_TEXT}${BOLD_TEXT}Form 2 execution completed successfully!${RESET_FORMAT}"
}

# Function to run form 3 code
run_form_3() {
    gsutil mb -c nearline gs://$Bucket_1
    echo "This is an example of editing the file content for cloud storage object" | gsutil cp - gs://$Bucket_2/sample.txt
    gsutil defstorageclass set ARCHIVE gs://$Bucket_3
    echo "${LIME_TEXT}${BOLD_TEXT}Form 3 execution completed successfully!${RESET_FORMAT}"
}

# Main script starts here
echo

read -p "${GOLD_TEXT}${BOLD_TEXT}Enter Bucket_1 name: ${RESET_FORMAT}" Bucket_1
echo "${WHITE_TEXT}Bucket_1 set as: ${BOLD_TEXT}$Bucket_1${RESET_FORMAT}"

read -p "${GOLD_TEXT}${BOLD_TEXT}Enter Bucket_2 name: ${RESET_FORMAT}" Bucket_2
echo "${WHITE_TEXT}Bucket_2 set as: ${BOLD_TEXT}$Bucket_2${RESET_FORMAT}"

read -p "${GOLD_TEXT}${BOLD_TEXT}Enter Bucket_3 name: ${RESET_FORMAT}" Bucket_3
echo "${WHITE_TEXT}Bucket_3 set as: ${BOLD_TEXT}$Bucket_3${RESET_FORMAT}"

echo "${NAVY_TEXT}${BOLD_TEXT}Choose the form number to execute:${RESET_FORMAT}"
read -p "${NAVY_TEXT}${BOLD_TEXT}Enter Form number (1, 2, or 3): ${RESET_FORMAT}" form_number

# Execute the appropriate function based on the selected form number
case $form_number in
    1) run_form_1 ;;
    2) run_form_2 ;;
    3) run_form_3 ;;
    *) echo "${MAROON_TEXT}${BOLD_TEXT}Invalid form number. Please enter 1, 2, or 3.${RESET_FORMAT}" ;;
esac

echo
echo "${LIME_TEXT}${BOLD_TEXT}=================================================${RESET_FORMAT}"
echo "${LIME_TEXT}${BOLD_TEXT}        LAB COMPLETED SUCCESSFULLY!        ${RESET_FORMAT}"
echo "${LIME_TEXT}${BOLD_TEXT}=================================================${RESET_FORMAT}"
```

### Fix the issue of Task one:
Goto Bucket 1 -> Configuration -> Change the type from 'standard' to mentioned in task.