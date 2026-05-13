# 1. Log in and create a project

## Log in

1. Open the site and you'll land on the **Login** page (`/login`).
2. Tick the **"I understand and acknowledge the beta service notice"** checkbox.
   The login buttons stay disabled until you do.
3. Choose one of:
   - **Send Magic Link** — enter your email, then open the email and click the
     link. You'll be redirected through `/auth/callback` to the dashboard.
   - **Continue with Google** — Google OAuth flow.

Your organization is created/attached automatically the first time you sign in;
there is no separate org-selection step.

## Create a project

After login you land on the **Dashboard** (`/`).

1. Click the **New Project** button (top right of the dashboard).
2. In the **Create New Project** modal, fill in:
   - **Project Name** — display name.
   - **Project Slug** — auto-generated from the name; you may edit it before
     creation. Letters, numbers, and hyphens only. *This cannot be changed
     later.*
   - **Description** — optional.
3. Click **Create Project**. You'll be taken to the project overview at
   `/project/{projectId}`.

The left sidebar now shows the per-project navigation: **Overview, Files,
IR Datasets, Generate, Models, Benchmarks, Calibration, Jobs**.

---

Next: [Add a dataset (file upload)](../02-dataset-upload/README.md)
