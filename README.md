import requests
import pandas as pd

def get_user_repos(username):
    url = f"https://api.github.com/users/{username}/repos"
    response = requests.get(url)
    
    if response.status_code != 200:
        print("Error:", response.json())
        return None
    
    repos = response.json()
    data = []
    for repo in repos:
        data.append({
            "Name": repo["name"],
            "Stars": repo["stargazers_count"],
            "Forks": repo["forks_count"],
            "Language": repo["language"],
            "URL": repo["html_url"]
        })
    return data

def save_to_excel(data, username):
    df = pd.DataFrame(data)
    file_name = f"{username}_repos_report.xlsx"
    df.to_excel(file_name, index=False)
    print(f"Report saved as {file_name}")

# Example usage
username = "torvalds"  # replace with any GitHub username
repos_data = get_user_repos(username)
if repos_data:
    save_to_excel(repos_data, username)
