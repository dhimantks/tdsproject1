# GitHub User and Repository Data Fetcher

This Python script interacts with the GitHub API to fetch data about users and their repositories based on specific criteria. It collects details about users in Tokyo with a minimum number of followers, cleans and processes this data, and saves it into CSV files for further analysis.

## Overview

The script performs the following main tasks:
1. Fetches user data from GitHub based on a specified location and minimum followers.
2. Cleans up user data, especially the company names.
3. Retrieves and processes repositories for each user, including details such as license information, project status, and wiki status.
4. Saves the collected data into two CSV files: `users.csv` and `repositories.csv`.

## Features

- Fetch user data based on location and follower count.
- Clean user company names for consistency.
- Fetch repository details, including:
  - Repository name
  - Owner login
  - License name
  - Project and Wiki status
- Save the fetched data into CSV files for easy access and analysis.

## Requirements

To run this script, you need:
- Python 3.x
- The following Python libraries:
  - Requests
  - Pandas

### Installation

You can install the required libraries using pip:

```bash
pip install requests pandas

GITHUB_API_URL = "https://api.github.com"
TOKEN = "your_github_token"  # Replace with your GitHub API token
HEADERS = {"Authorization": f"token {TOKEN}"}
USERS_FILE = "users.csv"
REPOS_FILE = "repositories.csv"

def clean_company_name(company):
    if not company:
        return ""
    company = company.strip()
    if company.startswith('@'):
        company = company[1:].strip()
    return company.upper()

def get_users(location="Tokyo", min_followers=200):
    users = []
    page = 1
    while True:
        url = f"{GITHUB_API_URL}/search/users"
        params = {"q": f"location:{location} followers:>{min_followers}", "per_page": 100, "page": page}
        response = requests.get(url, headers=HEADERS).json()
        if "items" not in response:
            break
        for user in response["items"]:
            user_details = requests.get(user["url"], headers=HEADERS).json()
            users.append({
                "login": user_details.get("login", ""),
                "name": user_details.get("name", ""),
                "company": clean_company_name(user_details.get("company", "")),
                "location": user_details.get("location", ""),
                "email": user_details.get("email", ""),
                "hireable": str(user_details.get("hireable", "")),
                "bio": user_details.get("bio", ""),
                "public_repos": user_details.get("public_repos", 0),
                "followers": user_details.get("followers", 0),
                "following": user_details.get("following", 0),
                "created_at": user_details.get("created_at", "")
            })
        page += 1
        if "items" not in response or not response["items"]:
            break
    return pd.DataFrame(users)

def save_to_csv(users_df, repos_df):
    users_df.to_csv(USERS_FILE, index=False)
    repos_df.to_csv(REPOS_FILE, index=False)


def main():
    print("Fetching users...")
    users_df = get_users()
    print("Fetching repositories...")
    repos_df = get_repositories(users_df)
    print("Saving data to CSV files...")
    save_to_csv(users_df, repos_df)
    print("Data saved successfully.")
