# Trunk-Based Development

Overview
1. You create a new branch.
2. You write and commit your code.
3. You push the commits to the remote repository on GitHub.
4. You open a Pull Request (PR).
5. You wait for the Continuous Integration pipeline to pass.
6. You request a review from other developers and discuss the code.
7. Once the PR is approved you merge it to the main branch.

# Clone repository
```bash
git clone <REPO_URL>
```

# Create a branch
```bash
git checkout -b <BRANCH_NAME>
```

# Stage changes

> Using the `.` stages all changes (use with caution)
```bash
git add .
```

# Check staged files
```bash
git status
```

# Commit changes
```bash
git commit -m "<COMMIT_MESSAGE>"
```

# Push to remote repository
```bash
git push origin <BRANCH_NAME>
```

> Pushing to the main branch
```bash
git push origin main
```

## (Theory) Pull Request / Merge Request

- With a "Pull Request" you *request* your branch to be *merged* with the main branch (or any other branch).

> *Hey folks, I wrote a bunch of code. I want to merge it so our users can benefit from it. Could you have a look and tell me if the code looks ok?*

- a description to explain the context and the code changes
- a place to discuss and comment the code
- a mechanism to ensure that only reviewed code can be merged

# (Optional) Request Reviewers

# Squash and Merge
- Fill out form that asks for title and a description
- clean up by deleting the branch

> Delete branches can be recovered

