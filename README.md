GitHub User and Repository Data Fetcher
Overview
This script fetches user and repository data from the GitHub API based on specified criteria. It retrieves users from a given location who have a minimum number of followers and then collects detailed information about their repositories. The data is cleaned and saved into two CSV files: users.csv and repositories.csv.

Features
Fetch Users: Retrieves users from a specified location (default: Tokyo) with a minimum number of followers (default: 200).
Clean Company Names: Trims whitespace from company names, removes leading @ symbols, and converts them to uppercase.
Fetch Repositories: For each user, the script fetches their repositories, capturing details such as:
Repository ID
Name
Full name
Private status
Owner's login
License name
Whether projects and wikis are enabled
Creation, update, and push timestamps
Repository statistics (size, stargazers count, watchers count, forks count, open issues count)
Save Data: The user and repository data are saved into CSV files for further analysis.
Requirements
Python 3.x
requests library
pandas library
You can install the required libraries using pip:

bash
Copy code
pip install requests pandas
Usage
Setup:
Replace your_github_token in the script with your personal GitHub API token. You can create a token by following this guide.
Run the Script: Execute the script in your Python environment:
bash
Copy code
python script_name.py
Output: The script generates two CSV files:
users.csv: Contains user data with fields such as login, name, company, location, email, hireable, bio, public_repos, followers, following, and created_at.
repositories.csv: Contains repository data with fields including id, name, full_name, private, owner_login, html_url, description, fork, created_at, updated_at, pushed_at, homepage, size, stargazers_count, watchers_count, language, forks_count, open_issues_count, license_name, login, has_projects, and has_wiki.
Code Explanation
The script consists of the following main functions:

clean_company_name(company): Cleans the company name by trimming whitespace, removing a leading @ symbol, and converting it to uppercase.

get_users(location, min_followers): Fetches users based on the specified location and minimum followers. It calls the GitHub API to retrieve user data, processes it, and returns a DataFrame.

get_repositories(users): Fetches repositories for each user in the provided DataFrame. It retrieves detailed repository information and returns a DataFrame.

save_to_csv(users_df, repos_df): Saves the user and repository DataFrames to CSV files.

main(): The main function that orchestrates the fetching and saving of data.

Limitations
The script may encounter rate limits imposed by the GitHub API. Consider implementing rate limiting or adding error handling to manage API responses gracefully.
Users without any repositories or invalid users may cause the script to return errors; proper error handling can be added for robustness.
Contribution
Feel free to fork the repository, make improvements, or open issues if you encounter any bugs or have feature requests.
