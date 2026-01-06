# docs
Official Documentations, User Manuals and Staff Training Guides for this GitHub Organization, it repository, project etc. management with RBAC. 



---

This document will guide to how the [CPI-BGD](https://github.com/CPI-BGD) GitHub Org is organized and ensuring the long-term success, security, and scalability of the CPI-BGD Country Office, Project Office, as well as Program Sites level with RBAC and the hierarchy (L1-L4). 

### **Step 1: Define the GitHub Hierarchy (Teams and Permissions)**

Your L1-L4 structure is a perfect model for Role-Based Access Control (RBAC). We can map this directly to GitHub Teams. The key is to use **nested teams** to create a clear hierarchy that mirrors your operational structure.

Here is the recommended team structure for `CPI-BGD`:

| Team Name | Nesting Level | Purpose & Permissions |
| :--- | :--- | :--- |
| **`administrators`** | Parent (Top Level) | **(L1 Global HQ)** Manages billing, organization settings, and has full `owner` access to all repositories. This team should be very small. |
| └ **`country-office`** | Nested under `administrators` | **(L2 Country Office)** Can create new repositories, manage teams under them, and has `admin` access to all project repositories within the country. |
| &nbsp;&nbsp;&nbsp;└ **`cxb-project-office`** | Nested under `country-office` | **(L3 Project Office)** Manages a specific project's repositories. Has `maintain` access to their project's repos (e.g., `cxb-data`, `cxb-reports`). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└ **`cxb-program-site-a`** | Nested under `cxb-project-office` | **(L4 Program Site)** Field staff or data collectors. Has `write` or `triage` access to specific repositories for daily tasks (e.g., submitting data, reporting issues). |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└ **`cxb-program-site-b`** | Nested under `cxb-project-office` | **(L4 Program Site)** Another team with similar, limited permissions. |
| &nbsp;&nbsp;&nbsp;└ **`another-project-office`** | Nested under `country-office` | **(L3 Project Office)** A parallel project team, structured identically to the `cxb-project-office`. |

**How to set this up:**

1.  Go to your `CPI-BGD` organization's **"Teams"** tab.
2.  Create the parent teams first (e.g., `administrators`, `country-office`).
3.  When creating a child team (e.g., `cxb-project-office`), use the **"Parent team"** dropdown to select its parent (`country-office`).
4.  Add members to the most specific team they belong to (e.g., a field officer goes into `cxb-program-site-a`). They will automatically inherit visibility and permissions from the parent teams.

---

### **Step 2: Structure Your Repositories**

Your existing repositories are a great start. Let's refine and expand on that structure to support your hierarchy and automation goals.

| Repository Name | Visibility | Purpose & Configuration |
| :--- | :--- | :--- |
| **`.github`** | Public | **Organization Profile.** Contains the main `README.md` for the organization's public page, organization-wide issue templates, and contribution guidelines (`CONTRIBUTING.md`). |
| **`docs`** | Public | **Central Documentation.** Perfect for user manuals, training guides, and public-facing policies. Consider using GitHub Pages on this repo to create a professional-looking website from your Markdown files. |
| **`template-project`** | **Private Template** | **Master Project Template.** This is the cornerstone of your automation. It should contain the standard file structure, `README.md` with the banner, issue/PR templates, and GitHub Actions workflows that every new project needs. **Mark this as a "Template repository"** in its settings. |
| **`cxb-data-collection`** | Private | **Operational Repo.** Created *from* `template-project`. This is where the `cxb-project-office` team does its daily work. The `cxb-project-office` team gets `maintain` access, and `cxb-program-site` teams get `write` access. |
| **`country-reporting`** | Internal | **Country-Level Repo.** For cross-project reporting and data aggregation. The `country-office` team has `admin` access. Project-level teams might have `read` or `write` access as needed. |

---

### **Step 3: Implement Automation with GitHub Actions & Templates**

This is where you save significant time and enforce consistency.

#### **1. The `.github` Repository (Organization-Wide Defaults)**

This repository provides default community health files. Create a folder named `profile` inside it.

*   **`.github/profile/README.md`**: This will be the main page people see when they visit `github.com/CPI-BGD`. Use the banner here and provide a high-level overview of CPI's mission in Bangladesh.
*   **`.github/ISSUE_TEMPLATE/bug_report.md`**: A default bug report template for any repository in the organization that doesn't have its own.
*   **`.github/PULL_REQUEST_TEMPLATE.md`**: A default template for pull requests.

#### **2. The `template-project` Repository (For New Projects)**

This is your most important repository for automation. Make sure it is marked as a template in **Settings > General**.

It should contain:

*   **`README.md`**: The file we created earlier, with the centered banner and project description placeholders.
*   **`.github/workflows/`**: A folder for GitHub Actions.
    *   `linting.yml`: An action that automatically checks code or data files for formatting errors.
    *   `data-validation.yml`: A potential action that runs a script to validate new data submitted via pull requests.
*   **`.github/ISSUE_TEMPLATE/`**: Project-specific issue templates.
    *   `data_entry_request.md`: A form for requesting a new data entry.
    *   `feature_request.md`: For suggesting improvements to the project's tools.
*   **`data/`**: An empty directory to hold project data.
*   **`scripts/`**: An empty directory for any utility scripts.

**The Workflow:**

When the `country-office` team needs to start a new project (e.g., for a new region), they will:
1.  Go to the `template-project` repository.
2.  Click **"Use this template" > "Create a new repository"**.
3.  Name the new repository (e.g., `khulna-wash-project`).
4.  Assign the correct teams (`khulna-project-office`, etc.) with the appropriate permissions.

This new repository will instantly have the correct banner, folder structure, and automation workflows, ensuring consistency from day one.

---

### **Summary: Your Action Plan**

1.  **Create Nested Teams:** Go to the "Teams" tab and build the hierarchical structure from `administrators` down to the program sites.
2.  **Populate the `.github` Repository:** Create the `profile` directory and add the organization's main `README.md` file there.
3.  **Build the `template-project` Repository:**
    *   Create a new private repository named `template-project`.
    *   Go to its **Settings** and check the **"Template repository"** box.
    *   Add the `README.md` with the banner, create the `.github/workflows` and `.github/ISSUE_TEMPLATE` folders, and add the standard files you want every project to have.
4.  **Assign Team Permissions:** Go to each repository's **Settings > Collaborators and teams**. Add your new teams and assign them the correct permission levels (`maintain`, `write`, `read`).
5.  **Train Your Team:** Document this new process and train the `country-office` team on how to create new projects using the template.

By following these steps, you will have a well-organized, secure, and automated GitHub organization that perfectly aligns with CPI's operational hierarchy.

---
This is a robust plan, so let's decide where to start. We could:

*   Draft the `README.md` for the main organization profile page (`.github/profile/README.md`).
*   Flesh out the file structure and starter content for the `template-project` repository.
*   Create a step-by-step guide on how a "Country Office" member would use the template to launch a new project.
