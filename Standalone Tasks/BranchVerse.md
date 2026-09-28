# Task ID: BranchVerse

`Full Stack Web Development` `DevOps` `Docker` `GitHub API` `Webhooks`

**Difficulty:** `Medium`

## Description

Build **BranchVerse**, a platform that automatically creates isolated preview deployments for GitHub Pull Requests.

Each Pull Request represents a separate version, or **universe**, of the application. When a PR is opened, BranchVerse should build and deploy that branch to a temporary preview URL so developers can test and review changes before merging them into the main branch.

---

## Core Requirements

### 1. GitHub Integration

Connect a GitHub repository and use **GitHub Webhooks** to handle:

- **PR opened** → Create a new preview environment
- **New commit pushed** → Automatically rebuild and redeploy the preview
- **PR merged** → Mark the environment as merged
- **PR closed** → Automatically destroy the preview environment

---

### 2. Preview Deployments

For every active Pull Request:

- Fetch the corresponding branch or commit
- Build the application
- Deploy it in an isolated environment using **Docker** or a similar approach
- Generate a unique preview URL
- Maintain deployment states such as:
  - `Building`
  - `Deploying`
  - `Live`
  - `Build Failed`
  - `Closed`
  - `Merged`

Each Pull Request should have its own independent preview deployment.

---

### 3. Dashboard

Build a dashboard displaying all Pull Request preview environments.

For each deployment, display:

- PR title and number
- Author
- Branch name
- Commit hash
- Deployment status
- Preview URL
- Creation time / latest deployment time
- Build logs

Build failures should be clearly visible along with their logs.

---

### 4. Timeline Comparison

Provide a review interface where developers can compare:

- The current **main branch**
- The deployed **Pull Request version**

Both versions should be viewable side-by-side so reviewers can visually inspect changes before merging.

---

## Extra Points

The following features are optional and may be implemented for additional points:

- **Synchronized Comparison:** Sync scrolling, navigation, or viewport size between both previews.
- **Visual Diff:** Use Playwright or Puppeteer to capture screenshots and highlight visual differences.
- **Health Checks:** Automatically test important routes after deployment.
- **GitHub PR Comments:** Post or update the preview URL and deployment status directly on the Pull Request.
- **Deployment History:** Store previous deployments and logs for each Pull Request.
- **Rollback:** Redeploy an older successful commit.
- **AI Build Explanation:** Use an LLM to analyze failed build logs and suggest possible fixes.
- **Deployment Expiry:** Automatically expire preview environments after a configurable amount of time.

---

## Deliverables

Your repository should include:

1. Complete frontend and backend source code
2. Deployment and container configuration
3. A `README.md` containing:
   - Setup instructions
   - Required environment variables
   - GitHub Webhook configuration
   - Instructions to run the project locally
4. A working deployment or demo video demonstrating:
   - PR creation
   - Preview deployment
   - Automatic redeployment after a new commit
   - Main vs PR comparison
   - Automatic cleanup after the PR is closed

---

## Evaluation Criteria

Submissions will be evaluated based on:

- Correct GitHub Webhook handling
- Reliability of preview deployments
- Automatic redeployment and cleanup
- Isolation between Pull Request environments
- Dashboard and comparison experience
- Error handling and build logs
- Code quality and project structure
- Documentation

A reliable implementation of the core requirements is more important than completing every extra feature.

---

## Useful Resources

- [GitHub Webhooks Documentation](https://docs.github.com/en/webhooks)
- [GitHub REST API Documentation](https://docs.github.com/en/rest)
- [GitHub Pull Requests API](https://docs.github.com/en/rest/pulls)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Playwright Documentation](https://playwright.dev/)
- [Puppeteer Documentation](https://pptr.dev/)

---

## Tips

- Start by correctly receiving and handling GitHub webhook events.
- First support one simple type of web application before trying to generalize the deployment process.
- Treat builds and deployments as asynchronous jobs rather than blocking API requests.
- Persist deployment states so the dashboard remains accurate even after server restarts.
- Make sure preview environments are properly cleaned up when Pull Requests are closed.
- Focus on reliability before attempting the extra-point features.
