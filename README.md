# Jira GitHub Integration

This repository demonstrates a **Proof of Concept (POC)** for integrating Jira with GitHub Actions. The primary goal is to automate the creation of Jira bug tickets whenever a GitHub issue is labeled as `bug`.

---

## 🛠 Features
- Automatically sync GitHub issues labeled as `bug` to Jira as bug tickets.
- Map issue details (e.g., title, description, reporter) directly into Jira tickets.
- Leverages **GitHub Actions** for seamless workflow automation.
- Provides a clear audit trail with links back to the original GitHub issue.

---

## 📂 Project Structure
```
jira-github-integration/
├── .github/
│   └── workflows/
│       └── jira-integration.yml  # GitHub Actions workflow file
└── README.md                     # Project documentation
```

---

## 🔧 Prerequisites
Before you begin, ensure you have the following:
1. **Jira Account**: Access to a Jira instance with admin rights to a project.
2. **GitHub Repository**: A repository where you have admin permissions to set up Actions.
3. **API Token**: A Jira API token for authentication. [Learn how to generate one](https://confluence.atlassian.com/cloud/api-tokens-938839638.html).
4. **GitHub Secrets**:
   - `JIRA_BASE_URL`: Your Jira instance URL (e.g., `https://your-domain.atlassian.net`).
   - `JIRA_USER_EMAIL`: The email address associated with your Jira account.
   - `JIRA_TOKEN`: Your Jira API token.

---

## 🔧 Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/ashikkumar23/jira-github-integration.git
cd jira-github-integration
```

### 2. Configure Secrets
Go to your repository's **Settings** > **Secrets and variables** > **Actions**, and add the following secrets:
- `JIRA_BASE_URL`
- `JIRA_USER_EMAIL`
- `JIRA_TOKEN`

### 3. Update the Workflow File
The workflow file is located at `.github/workflows/jira-integration.yml`. Ensure the following is set:
- Replace `JGI` with your Jira project key.
- Adjust other parameters if needed.

### 4. Test the Workflow
- Create a new issue in the repository.
- Add the `bug` label to the issue.
- Check your Jira project for a new bug ticket.

---

## 🚀 How It Works
1. When you add the `bug` label to a GitHub issue, the workflow is triggered.
2. The workflow uses the [Atlassian Jira GitHub Action](https://github.com/marketplace/actions/jira-create-issue) to create a bug ticket in Jira.
3. The issue details (title, description, reporter, and URL) are mapped to the Jira ticket.

---

## 📝 Example Workflow File

The following is the GitHub Actions workflow file used in this project:

```yaml name=.github/workflows/jira-integration.yml
name: Create Jira Issue on Label

on:
  issues:
    types:
      - labeled

jobs:
  create-jira-issue:
    if: contains(github.event.label.name, 'bug') # Trigger only when the 'bug' label is added
    runs-on: ubuntu-latest
    steps:
      - name: Create Jira Issue
        uses: atlassian/gajira-create@v3.1.0
        with:
          jira-base-url: ${{ secrets.JIRA_BASE_URL }}
          jira-user-email: ${{ secrets.JIRA_USER_EMAIL }}
          jira-api-token: ${{ secrets.JIRA_TOKEN }}
          project-key: JGI
          summary: "Bug: ${{ github.event.issue.title }}"
          description: |
            Reporter: ${{ github.actor }}
            
            Issue URL: ${{ github.event.issue.html_url }}
            
            Description:
            ${{ github.event.issue.body }}
          issue-type: Bug
          assignee: ${{ github.actor }}
```

---

## 🌟 Limitations
- Currently supports only issues labeled as `bug`.
- Does not sync updates or status changes from Jira back to GitHub.

---

## 📖 References
- [Jira API Documentation](https://developer.atlassian.com/cloud/jira/platform/rest/v3/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Atlassian Jira GitHub Action](https://github.com/marketplace/actions/jira-create-issue)

---

## 🛡 License
This project is licensed under the [MIT License](LICENSE).

---

## 🤝 Contributing
Contributions are welcome! Feel free to open issues or submit pull requests.

---

## 📞 Support
If you encounter any issues or have questions, please feel free to open an issue in this repository.