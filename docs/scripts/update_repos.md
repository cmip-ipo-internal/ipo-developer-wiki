# CRON update github repositories. 

We have a number of repositories on our VM that we would wish to keep up to date. 

To ensure this is the case we run a timed job (CRON) which checks and updates a list of repositories.

The basic code required to accomplish this can be found: 

```bash 
#!/bin/bash

# Define the list of repositories
repositories=(
    "$HOME/repo1-location"
    "$HOME/repos/repo2-location"
    ...
)

# Specify the branch to check for updates
branch="main"

# Specify the log file with date
log_file="$HOME/.logs/repo_update_$(date +\%Y\%m\%d).log"

# Specify the backup folder
backup_folder="$HOME/backup"

# Create the backup folder if it doesn't exist
mkdir -p "$backup_folder"

# Loop through each repository and update if changes are found
for repo in "${repositories[@]}"; do
    echo "------------------------------"
    echo "Checking for updates in $repo"
    
    # Navigate to the repository
    cd "$repo" || exit 1
    
    # Fetch remote changes
    git fetch origin "$branch"

    # Check if there are any changes on the specified branch
    if [[ $(git rev-list HEAD..origin/"$branch" --count) -gt 0 ]]; then
        echo "Updating $repo"
        
        # Create a backup with date timestamp
        backup_file="$backup_folder/$(basename "$repo")_backup_$(date +\%Y\%m\%d_\%H\%M\%S).tar.gz"
        tar -czf "$backup_file" . >> "$log_file" 2>&1
        
        # Pull changes and force overwrite conflicts
        git pull origin "$branch" --force >> "$log_file" 2>&1
        
        # Log the status of the update with the date
        echo "$(date): Update status for $repo: Success" >> "$log_file"
        echo "$(date): Repository backed up to $backup_file" >> "$log_file"
    else
        echo "No updates found in $repo"
        
        # Log the status of the update with the date
        echo "$(date): Update status for $repo: No updates found" >> "$log_file"
    fi
    
    # Return to the script's directory
    cd - || exit 1
done

```