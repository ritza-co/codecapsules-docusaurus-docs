---
slug: "/tutorials/build-and-host-your-portfolio-for-free"
description: >-
  Build a static portfolio site and host it for free on Code Capsules
  using an AI coding agent.
cover: /gitbook-assets/tutorials/free-portfolio-cover.jpg
---

# How to Build and Host Your Portfolio for Free on Code Capsules

Most portfolio hosting options either cost money or lock you into a specific framework. Code Capsules' free tier hosts static frontend sites with HTTPS and a public URL, so you can deploy a plain HTML portfolio without paying anything.

In this guide, we'll use [Claude Code](https://claude.ai/code) as a coding agent to fork a portfolio template, set up the Code Capsules skill, and deploy the site, all from a single terminal.

![The finished portfolio site running on Code Capsules](/gitbook-assets/tutorials/free-portfolio-live.png)

## Prerequisites for deploying to Code Capsules

Before starting, make sure you have the following:

- [Claude Code](https://claude.ai/code) installed and configured, with access to GitHub via `gh auth login`, SSH keys, or any existing Git authentication. If you don't have any of these set up yet, the quickest option is to [install the GitHub CLI](https://cli.github.com/) and run `gh auth login`.
- A [Code Capsules](https://codecapsules.io/) account (free tier is fine).
- A [GitHub](https://github.com/signup) account.

Open your terminal (Terminal on macOS, Command Prompt or WSL on Windows) and make sure Claude Code is working by running `claude` before continuing.

## Fork the portfolio template

Ask the agent to fork the template repo and clone it locally:

```
Fork codecapsules-io/demo-portfolio to my GitHub account and clone it.
```

Claude Code will fork the repo and clone it into your current directory. The template ships with placeholder content for a fictional person. You'll replace it with your own details in the [Customize and redeploy](#customize-and-redeploy) step which you can do before or after deploying. Every push to the connected branch triggers a new build automatically.

## Install the Code Capsules skill

The [Code Capsules skill](https://github.com/codecapsules-io/skills) gives Claude Code API access to create and manage Capsules from the terminal. You can also do all of this through the [web dashboard](https://app.codecapsules.io/dash/overview/quick-start), but the skill lets us stay in the terminal for this guide.

Ask the agent to install it:

```
Install the skill at: https://github.com/codecapsules-io/skills
```

Claude Code will clone the skill repo and copy the files into its local skills directory so it can manage Code Capsules deployments on your behalf.

For a deeper walkthrough of the skill and what it can do, see [Use Code Capsules with an Agent](/tutorials/use-codecapsules-with-an-agent).

### Add your Code Capsules credentials

The skill needs your Code Capsules login credentials to authenticate against the API. You can set them by asking Claude Code directly:

```
Set my CC_EMAIL and CC_PASSWORD environment variables in your settings.
```

The agent will add them to `~/.claude/settings.json` for you. These credentials are stored locally on your machine and are only sent to the Code Capsules API when the skill authenticates.

If you prefer to set them manually, add an `env` key with `CC_EMAIL` and `CC_PASSWORD` to `~/.claude/settings.json`, or export both variables from your shell profile (`~/.zshrc` or `~/.bashrc`).

### Connect GitHub to Code Capsules

Code Capsules needs read access to your GitHub repository so it can pull your code and build it. This is the one step that requires a browser, since GitHub's authorization flow involves OAuth permissions that an agent can't change on your behalf.

Follow [Connect Version Control](https://docs.codecapsules.io/platform/account/connect-version-control) to link your GitHub account and authorize access to the forked portfolio repo.

## Deploy the portfolio

From the cloned `demo-portfolio` directory, give Claude Code the following prompt:

```
Deploy this as a frontend capsule on Code Capsules using the free tier.
```

The agent will:

1. Read your project to determine the Capsule type (frontend, since it's static HTML)
2. Authenticate with the Code Capsules API
3. Create a frontend Capsule linked to your GitHub repo
4. Trigger a build

If you haven't created an `AGENTS.md` file with your Team and Space names, the agent will ask you to choose them interactively. You can avoid that back-and-forth by adding an `AGENTS.md` file to the project root first:

```markdown
# Code Capsules Deployment

## Account
- **Team**: your-team-name
- **Space**: your-space-name
- **GitHub repo**: your-github-username/demo-portfolio
- **Region**: your-region

## Preferences
- **Plan tier**: Free
```

Replace the Team, Space, repo, and region values with your own. You can find your Team and Space names in the Code Capsules dashboard (see [What is a Team?](/platform/teams/what-is-a-team) and [What is a Space?](/platform/spaces/what-is-a-space) if you haven't created them yet). For available regions, see [Regions](/platform/regions/what-regions-does-code-capsules-support).

## Get your live URL

The build usually takes a minute or two. The skill will tell you when it completes. Once it does, ask the agent:

```
What's the URL for my portfolio capsule?
```

The agent returns the public URL assigned by Code Capsules. It will look something like `https://demo-portfolio.codecapsules.co.za`. This is the URL you can share, add to your resume, or link from your social profiles.

The generated URL is permanent and works over HTTPS.

## Customize and redeploy

The portfolio is yours to edit. Ask the agent to personalize the template and push the changes:

```
Update the heading and intro with my name and bio, switch to a light theme with orange accents, then commit and push to GitHub.
```

Code Capsules detects the push and rebuilds automatically. Within a minute or two, the live site reflects your changes.

![The portfolio after changing the name, bio, and color scheme](/gitbook-assets/tutorials/free-portfolio-customized.png)

## Next steps

Now that your portfolio is live, here are some things you can try:

- **Add a contact form backend:** Create a backend Capsule with a database to handle form submissions. The [Use Code Capsules with an Agent](/tutorials/use-codecapsules-with-an-agent) guide walks through deploying backend and database Capsules with the same agent workflow.
- **Set up a custom domain:** Upgrade from the free tier and point your own domain at the Capsule. See the [frontend deployment guides](/frontend) for instructions.
- **Explore the full skill API:** The [Code Capsules skill repository](https://github.com/codecapsules-io/skills) documents every endpoint the agent can use, including environment variables, build logs, and scaling.
