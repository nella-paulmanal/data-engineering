# Databricks Free Edition: Collaboration Workflow

A practical guide for the **FTW Batch 12 Data Engineering Scholars** on how we collaborate using Databricks Free Edition and GitHub.

---

## Purpose

I created this guide to document the Databricks collaboration workflow that we can use for our data engineering projects.

Since I initially set up our Databricks workspace and GitHub integration, this documentation is meant to make the setup and workflow easier for everyone to understand and follow, especially when working on our group projects.

The goal is to have a simple and consistent process for:

* Testing and developing queries
* Sharing work within the team
* Managing finalized queries
* Using GitHub for version control
* Creating branches
* Reviewing changes through Pull Requests
* Keeping the `main` branch organized

> **Note:** This workflow is designed specifically around the capabilities and limitations of **Databricks Free Edition**.

### Basic Workflow

> **Develop → Test → Finalize → Pull `main` → Create Branch → Add Code → Test → Commit → Push → Pull Request → Review → Merge**


## Table of Contents

1. [Purpose](#purpose)
2. [Collaboration Setup](#collaboration-setup)
3. [Setting Up](#setting-up)
   - [Create a GitHub Testing Repository](#1-create-a-github-testing-repository)
   - [Create a Databricks Free Edition Account](#2-create-a-databricks-free-edition-account)
   - [Set Up the Shared Workspace](#3-set-up-the-shared-workspace)
   - [Create Individual Testing Notebooks](#4-create-individual-testing-notebooks)
   - [Set Up Team Access](#5-set-up-team-access)
   - [Why We Use a Shared Testing Area](#6-why-we-use-a-shared-testing-area)
4. [Development and Git Workflow](#development-and-git-workflow)
   - [Finalize the Query](#7-finalize-the-query)
   - [Pull the Latest `main`](#8-pull-the-latest-main)
   - [Create a New Branch](#9-create-a-new-branch)
   - [Add the Finalized Query to the Git Folder](#10-add-the-finalized-query-to-the-git-folder)
   - [Test the Git-Managed Version](#11-test-the-git-managed-version)
   - [Commit the Changes](#12-commit-the-changes)
   - [Push the Branch](#13-push-the-branch)
   - [Create a Pull Request](#14-create-a-pull-request)
   - [Review the Pull Request](#15-review-the-pull-request)
   - [Merge the Pull Request](#16-merge-the-pull-request)
   - [Start the Next Task](#17-start-the-next-task)
5. [Complete Collaboration Workflow](#complete-collaboration-workflow)
6. [Team Responsibilities](#team-responsibilities)
7. [Team Rules](#team-rules)
8. [Why This Workflow?](#why-this-workflow)
9. [Shared Workspace vs Git Folder vs GitHub](#shared-workspace-vs-git-folder-vs-github)
10. [Key Concepts](#key-concepts)
11. [Final Workflow](#final-workflow)
---

## Collaboration Setup

We use Databricks and GitHub for different purposes.

```text
                    DATABRICKS
                         │
              ┌──────────┴──────────┐
              │                     │
              ▼                     ▼
      Shared Workspace      Shared Git Folder
              │                     │
              │                     │
       Develop + Test          Finalized Work
              │                     │
              └──────────┬──────────┘
                         │
                         ▼
                       GitHub
                         │
                  Pull Requests
                         │
                         ▼
                        main
```

### Databricks

#### Shared Workspace

The Shared Workspace is where we develop and test queries. This is our testing or `sandbox` area.

#### Shared Git Folder

The Git Folder contains the finalized project files that are connected to our GitHub repository. This is where code goes once it is ready to become part of the project.

### GitHub

GitHub is used for:

* Version control
* Branches
* Pull Requests
* Code review
* Merging changes into `main`

---

# Setting Up

## 1. Create a GitHub Testing Repository

This time I created a separate GitHub repository so we can demonstrate the collaboration workflow without affecting an actual project repository.

Example repository:

```text
databricks-collaboration-test
```

## 2. Create a Databricks Free Edition Account

For the tutorial, I created a new Gmail account and used it to create a new Databricks Free Edition account.

The purpose of using a new account is to demonstrate the setup from a clean environment.

## 3. Set Up the Shared Workspace

From the Databricks workspace, go to:

```text
Workspace → Shared
```

Inside the Shared folder, create a folder for our testing notebooks.

For example:

```text
Shared/
└── instacart-test-queries/
```

This folder is where we can develop and test queries before moving finalized work into the Git-managed project.

## 4. Create Individual Testing Notebooks

Each member can create their own testing notebooks.

Example:

```text
instacart-test-queries/
├── Bri/
│   └── testing_queries
├── Sara/
│   ├── testing_queries_mart
│   └── testing_queries_analytics
├── Virna/
│   └── testing_queries
├── Tricia/
│   └── testing_queries
└── Sam/
    └── testing_queries
```

These notebooks are our **development sandbox**.

We can use them to:

* Explore datasets
* Write SQL queries
* Test transformations
* Check results
* Debug errors
* Try different approaches
* Validate our logic

> **Note:** The testing notebooks do not need to follow the final project structure.

## 5. Set Up Team Access

To make the shared workspace accessible to the team, go to: 

```text
Settings > Identity and Access > Users > Manage > Add users.
```

Example:

```text
data-engineering-team
├── Bri
├── Sara
├── Virna
├── Tricia
└── Sam
```

## 6. Why We Use a Shared Testing Area

The testing area gives us somewhere to experiment without immediately changing the version-controlled project.

For example, we might start with:

```sql
SELECT *
FROM products;
```

Then test different filters, joins, transformations, or validation logic.

We might change the query several times before deciding what the final version should look like.

```text
Testing Notebook
       │
       ├── Experiment
       ├── Modify
       ├── Test
       ├── Debug
       └── Test again
              │
              ▼
          Finalize
```

We do not need to create a Git commit for every experiment.

---

# Development and Git Workflow

## 7. Finalize the Query

Once the query has been tested and we are satisfied with the result, we can consider it finalized.

Before moving it into the Git-managed project, check:

* Does the query work?
* Does it produce the expected result?
* Is the logic correct?
* Is the naming consistent?
* Is the query in the appropriate project layer?
* Does it follow the project's existing structure?

The testing notebook can remain as our development workspace.

The Git-managed project should contain the finalized version.

## 8. Pull the Latest `main`

Before starting Git-based work, pull the latest changes from `main`.

This is important because another team member may have already merged changes since we last worked on the project.

```text
GitHub
   │
   ▼
 main
   │
   ▼
Pull latest changes
   │
   ▼
Databricks Git Folder
```

Always start from the most recent version of `main` when creating a new branch.

## 9. Create a New Branch

After pulling the latest `main`, create a branch for the task.

For example:

```text
feature/orders-validation
```

Other examples:

```text
feature/dim-product
feature/data-cleaning
feature/data-quality
feature/orders-analysis
fix/missing-product-id
```

The branch allows us to work on a specific task without directly modifying `main`.

Our repository might look like:

```text
main
│
├── feature/orders-validation
├── feature/dim-product
└── feature/data-quality
```

Each branch should represent a specific piece of work.

## 10. Add the Finalized Query to the Git Folder

Once the branch has been created, add the finalized query from the testing notebook to the appropriate file or notebook in the Git Folder.

For example:

```text
Git Folder/
└── instacart/
    ├── bronze/
    ├── silver/
    ├── gold/
    └── validation/
        └── orders_validation.sql
```

The important distinction is:

```text
Shared Testing Folder
        ↓
    Experiment
        ↓
       Test
        ↓
     Finalize
        ↓
Git-managed Project
```

The Git Folder represents the project that we are maintaining under version control.

## 11. Test the Git-Managed Version

After adding the finalized query to the Git Folder, run it again. This gives us another `checkpoint` before committing the changes.

The workflow becomes:

```text
Testing Notebook
       ↓
     Finalize
       ↓
   Git Folder
       ↓
    Test Again
       ↓
     Commit
```

The version that we commit should be the version that we actually tested.

## 12. Commit the Changes

Once the Git-managed version has been tested, commit the changes.

Use a descriptive commit message.

Good examples:

```text
Add orders data quality validation
Create dim_product transformation
Add null checks for orders table
```

Avoid vague commit messages such as:

```text
updated
changes
test
final
```

A commit message should give us an idea of what changed without requiring archaeology six weeks later.

## 13. Push the Branch

After committing the changes, push the branch to GitHub.

```text
Databricks Git Folder
        │
        ▼
   Feature Branch
        │
      Commit
        │
       Push
        ▼
      GitHub
```

The feature branch is now available in the GitHub repository.

## 14. Create a Pull Request

Go to the GitHub repository.

Create a Pull Request from the feature branch into `main`.

For example:

```text
feature/orders-validation
          │
          ▼
    Pull Request
          │
          ▼
         main
```

The Pull Request is where we ask another team member to review the changes before they are merged.

## 15. Review the Pull Request

The reviewer should check the changes before approving the Pull Request.

Things to review include:

* Is the query correct?
* Does it produce the expected result?
* Is the logic clear?
* Is the file in the correct folder?
* Does it follow our naming conventions?
* Has the query been tested?
* Does it introduce unnecessary changes?
* Does it follow the project's existing approach?

If changes are requested, the developer can modify the same branch and push the changes again.

The Pull Request will update automatically with the new commits.

## 16. Merge the Pull Request

Once the Pull Request has been reviewed and approved, merge it into `main`.

```text
Feature Branch
      │
      ▼
Pull Request
      │
      ▼
Code Review
      │
      ▼
Approved
      │
      ▼
Merge
      │
      ▼
main
```

The finalized work is now part of the `main` project.

## 17. Start the Next Task

When starting another task, pull the latest version of `main` again to keep everyone's Git-managed project up to date.

The cycle repeats:

```text
Pull main
    ↓
Create branch
    ↓
Add changes
    ↓
Test
    ↓
Commit
    ↓
Push
    ↓
Pull Request
    ↓
Review
    ↓
Merge
```

---

# Complete Collaboration Workflow

## 18. End-to-End Workflow

Our complete workflow is:

```text
┌──────────────────────────────┐
│     Shared Testing Area      │
│                              │
│  Individual Testing Notebooks│
└──────────────┬───────────────┘
               │
               ▼
            Develop
               │
               ▼
              Test
               │
               ▼
            Finalize
               │
               ▼
      Pull latest `main`
               │
               ▼
         Create branch
               │
               ▼
      Add finalized code
               │
               ▼
          Test again
               │
               ▼
            Commit
               │
               ▼
             Push
               │
               ▼
┌──────────────────────────────┐
│            GitHub            │
│                              │
│       Create Pull Request    │
└──────────────┬───────────────┘
               │
               ▼
             Review
               │
               ▼
             Merge
               │
               ▼
              `main`
```

### In One Line

> **Develop → Test → Finalize → Pull `main` → Create Branch → Add Code → Test → Commit → Push → Pull Request → Review → Merge**

---

# Team Responsibilities

## 19. Everyone Follows the Same Basic Process

### When Developing

Use the Shared Testing folder.

```text
Shared/
└── instacart-test-queries/
    └── <your-name>/
```

### When the Work Is Finalized

Move the finalized work into the Git-managed project.

### Before Making Git Changes

Pull the latest `main`.

### When Working on a Task

Create a feature branch.

### Before Merging

Test the changes and create a Pull Request.

### When Reviewing

Check the logic, structure, and correctness of the changes.

### After Merging

Pull the updated `main` before starting the next Git-based task.

---

# Team Rules

## 20. Do

* Use the Shared Workspace for experimentation and testing.
* Keep individual testing notebooks organized.
* Pull the latest `main` before starting Git-based work.
* Create a separate branch for each task.
* Use descriptive branch names.
* Test finalized code before committing.
* Use meaningful commit messages.
* Create Pull Requests for changes going into `main`.
* Review each other's work.
* Keep `main` stable.

## Avoid

* Directly modifying `main`.
* Using the Git Folder as a random testing area.
* Committing untested queries.
* Overwriting another person's work.
* Using vague commit messages.
* Making unrelated changes in the same branch.
* Working from an outdated `main`.
* Assuming a query is final just because it ran successfully once.

---

# Why This Workflow?

## 21. Separating Experimentation, Finalization, and Collaboration

The purpose of this setup is not to make our process unnecessarily complicated. We are simply separating three things:

### 1. EXPERIMENT

**Shared Databricks Workspace**

Used for:

* Exploration
* Testing
* Debugging
* Experimentation

### 2. FINALIZE

**Databricks Git Folder**

Used for:

* Finalized project code
* Project structure
* Version-controlled development

### 3. COLLABORATE

**GitHub + Pull Requests**

Used for:

* Version control
* Code review
* Collaboration
* Merging changes into `main`

This lets us experiment freely while keeping the actual project organized and version-controlled.

It also gives everyone a common workflow, so we do not have five different interpretations of what "I already pushed my changes" means.

---

# Shared Workspace vs Git Folder vs GitHub

## 22. Purpose of Each Location

| Location                        | Purpose                               |
| ------------------------------- | ------------------------------------- |
| **Shared Testing Folder**       | Experimentation and query development |
| **Individual Testing Notebook** | Personal development and testing      |
| **Databricks Git Folder**       | Finalized project code                |
| **GitHub Feature Branch**       | Version-controlled task development   |
| **Pull Request**                | Code review                           |
| **`main`**                      | Shared finalized version              |

---

# Key Concepts

## 23. Definitions

### Shared Workspace

A place where team members can access shared Databricks resources and collaborate.

### Testing Notebook

A notebook used for experimentation and development before work is finalized.

### Git Folder

A Databricks folder connected to a Git repository where version-controlled project work is maintained.

### Branch

A separate line of development used to work on a specific task without directly changing `main`.

### Commit

A recorded set of changes in Git.

### Push

Sending committed changes from the local Git-managed environment to the remote GitHub repository.

### Pull Request

A request to merge changes from one branch into another, usually accompanied by review.

### `main`

The primary branch containing the shared finalized version of the project.

---

# Final Workflow

## 24. FTW Batch 12 Collaboration Workflow

For our FTW Batch 12 projects, the workflow we will use is:

```text
                 DEVELOP
                    │
                    ▼
          Shared Testing Area
                    │
                    ▼
                  TEST
                    │
                    ▼
                FINALIZE
                    │
                    ▼
            Pull latest `main`
                    │
                    ▼
             Create Branch
                    │
                    ▼
          Add finalized code
                    │
                    ▼
              Test again
                    │
                    ▼
                Commit
                    │
                    ▼
                 Push
                    │
                    ▼
          ┌─────────────────┐
          │     GitHub      │
          │  Pull Request   │
          └────────┬────────┘
                   │
                   ▼
                 Review
                   │
                   ▼
                 Merge
                   │
                   ▼
                  `main`
```


This is a beginner-friendly collaboration workflow for Databricks Free Edition.

It gives us a practical way to work together using the tools available to us while keeping our finalized project code organized, version-controlled, and reviewed.

Remember, the point is not to build the world's most sophisticated development pipeline. We're learning data engineering, not trying to recreate Microsoft's engineering organization in a Saturday class:)

The point is to have a workflow that everyone can _understand, repeat, and maintain_.
