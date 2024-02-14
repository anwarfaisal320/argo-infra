#!/bin/bash

#folder_path=test  # Replace with your desired directory path
attribute="custom_cluster_tags"
for file in *.json; do
    if [[ -f "$file" ]]; then
        # Read the JSON file
        
        json=$(cat "$file")
        #echo "$json"
        # Check if "custom-tag" attribute exists
        if [[ $(echo "$json" | grep -c '"custom_cluster_tags"') -gt 0 ]]; then
            echo "anwar"
            # Get the current value of "custom-tag"
            #current_value=$(echo "$json" | grep -o '(?<="custom_cluster_tags": ")[^"]*')
            current_value=$(grep -o "\"$attribute\": *\"[^\"]*\"" "$file" | sed -E "s/\"$attribute\": *\"([^\"]*)\"/\1/")
            echo "$current_value"
            # Check if "custom-tag" has an empty value
            if [[ -z "$current_value" ]]; then
                # Add "zp-n-test" to "custom-tag"
                echo "basha"
                updated_json=$(echo "$json" | sed 's/"custom_cluster_tags": ""/"custom_cluster_tags": "zp-n-test"/')
            else
                # Append "zp-n-test" to "custom-tag"
                echo "hello"
                updated_json=$(echo "$json" | sed 's/"custom_cluster_tags": "\(.*\)"/"custom_cluster_tags": "\1,zp-n-test"/')
            fi

            # Write the updated JSON back to the file
            echo "$updated_json" > "$file"
        fi
    fi
done
