# Gets JSON Randomimzed Value
This bashrandomizer script allows you to read a JSON file containing key-value pairs and retrieve a random value based on a randomly selected key.

## Functionality
* Validates if the provided argument is a JSON file;
* Counts the number of keys in the JSON file;
* Generates a random number based on the total number of keys;
* Displpays the value associated with the randomly selected key.

## JSON Structure
The script expects a simple JSON with key-value pairs. For example:
```
{
    "1": "1st Value",
    "2": "2nd Value",
    "3": "3rd Value",
    "4": "4th Value",
    "5": "5th Value"
}
```

## Prerequisites
* A compatible *bash terminal* (Linux, MacOS, or WSL on Windows);
* The *`shuf`* command (usually available on Linux distributions).

## Usage
* 1) Save your JSON file, such as *data.json*;
* 2) Move the script to a directory included in your `PATH`.

### For Linux and MacOS
To move the script to `/usr/local/bin`, use:
```
sudo mv /path/to/your/bashrandomizer /usr/local/bin
```
Make the script executable:
```
sudo chmod +x /usr/local/bin/bashrandomizer
```

### For Windows (WSL)
If you are using *Windows Subsystem for Linux (WSL)*, you can follow the same steps as above, moving the script to `/usr/local/bin`.

### Running the Script
Now you can run the script by simply typing:
```
bashrandomizer /path/to/data.json
```

## Example Output
If the *data.json* file contains the example above, the output could look like this:
```
You got: 5th Value
```

## Script Overview
Here is a summary of what each part of the script does:
```
#! /bin/bash

# Checks if the first argument is a file
if [[ -f "$1" ]]; then
    # Reads the JSON file content
    json_content=$(<"$1")

    # Counts the number of keys in the JSON
    item_count=$(echo "$json_content" | grep '"[^"]*":' | wc -l)

    # Generates a random number between 1 and the item count
    random_number=$(shuf -i 1-"$item_count" -n 1)

    # Extracts the value corresponding to the selected key
    random_value=$(echo "$json_content" | grep -oP "\"$random_number\": \"[^\"]*\"" | awk -F': ' '{print $2}' | tr -d '"')

    # Displays the selected value
    echo "You got: $random_value"
else
    # In case no valid file is provided
    echo "You must provide a file as the first argument!"
    echo "Usage: bashrandomizer /path/to/file.json"
fi
```

## Potential Improvements
* **Json Validation**: Verify that the JSON format is valid before processing;
* **Support for Non-Numeric Keys**: Allow keys that are not integers.
