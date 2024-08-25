
# Comprehensive Guide to Publishing Your Obsidian Vault Online Using Quartz on Windows 10

Welcome! This guide will walk you through the process of publishing your Obsidian Vault named **"General"** to a live website using **Quartz**. We'll cover everything from setting up the necessary tools on your Windows 10 machine to deploying your site online and syncing future changes seamlessly.

---

## Table of Contents

1. [Prerequisites](#1-prerequisites)
2. [Setting Up Quartz Locally](#2-setting-up-quartz-locally)
3. [Integrating Your Obsidian Vault with Quartz](#3-integrating-your-obsidian-vault-with-quartz)
4. [Previewing Your Site Locally](#4-previewing-your-site-locally)
5. [Setting Up Git and GitHub](#5-setting-up-git-and-github)
6. [Deploying Your Site Online](#6-deploying-your-site-online)
    - [Option 1: Cloudflare Pages](#option-1-cloudflare-pages)
    - [Option 2: GitHub Pages](#option-2-github-pages)
7. [Syncing Future Changes](#7-syncing-future-changes)
8. [Additional Configurations](#8-additional-configurations)
9. [Troubleshooting](#9-troubleshooting)
10. [Conclusion](#10-conclusion)

---

## 1. Prerequisites

Before we begin, ensure you have the following installed on your Windows 10 machine:

- **Node.js v20 or higher** (You have **v22.7.0** ✅)
- **npm v9.3.1 or higher** (You have **v10.8.2** ✅)
- **Git** (If not installed, follow the instructions below)

### Installing Git

Git is essential for version control and deploying your site. If you don't have Git installed, follow these steps:

1. **Download Git for Windows**:
   - Visit the official Git website: [https://git-scm.com/download/win](https://git-scm.com/download/win)
   - Download the installer and run it.
   - Follow the installation prompts, leaving default settings unless you have specific preferences.

2. **Verify Git Installation**:
   ```powershell
   git --version
   ```
   - You should see something like `git version 2.x.x`.

---

## 2. Setting Up Quartz Locally

We'll clone the Quartz repository and set it up on your local machine.

### Step 1: Choose a Directory for Quartz

Decide where you want to place your Quartz project. For this guide, we'll use `D:\ACC_ID\OneDrive\Desktop\Quartz` as an example.

### Step 2: Clone the Quartz Repository

1. **Open PowerShell**:
   - Press `Win + X` and select **Windows PowerShell** or **Windows PowerShell (Admin)**.

2. **Navigate to Your Desired Directory**:
   ```powershell
   cd D:\ACC_ID\OneDrive\Desktop
   ```

3. **Clone the Quartz Repository**:
   ```powershell
   git clone https://github.com/jackyzha0/quartz.git
   ```

4. **Rename the Cloned Folder (Optional)**:
   - For clarity, you can rename the cloned `quartz` folder:
     ```powershell
     Rename-Item -Path .\quartz -NewName Quartz
     ```

5. **Navigate into the Quartz Directory**:
   ```powershell
   cd Quartz
   ```

### Step 3: Install Dependencies

1. **Install npm Dependencies**:
   ```powershell
   npm install
   ```

   - This command installs all necessary packages required by Quartz.

2. **Initialize Quartz with Default Content**:
   ```powershell
   npx quartz create
   ```

   - When prompted, choose to initialize with the default content. This sets up the necessary folder structure and example files.

---

## 3. Integrating Your Obsidian Vault with Quartz

Now we'll replace the default Quartz content with your own Obsidian vault content.

### Step 1: Backup Default Content (Optional)

It's a good idea to backup or remove the default content provided by Quartz.

1. **Remove Default Content**:
   ```powershell
   Remove-Item -Recurse -Force .\content\*
   ```

   - This command deletes all files and folders inside the `content` directory.

### Step 2: Copy Your Obsidian Vault into Quartz

1. **Locate Your Obsidian Vault**:
   - Your vault is located at `D:\ACC_ID\OneDrive\Desktop\ObsidianVaults\General`.

2. **Copy Vault Content to Quartz**:
   ```powershell
   Copy-Item -Recurse -Force D:\ACC_ID\OneDrive\Desktop\ObsidianVaults\General\* .\content\
   ```

   - This copies all your notes and folders into Quartz's `content` directory.

### Step 3: Verify the Structure

Ensure that all your markdown files (`.md`) and associated assets (images, PDFs, etc.) are correctly placed inside the `content` folder.

### Step 4: Update Quartz Configuration (Optional)

If your vault uses specific features or requires certain configurations, you may need to update the `quartz.config.ts` file.

1. **Open `quartz.config.ts` for Editing**:
   - Use your preferred text editor (e.g., VSCode).

2. **Set `baseUrl`**:
   - We'll set this later once we decide on the deployment URL.

3. **Customize Theme and Plugins**:
   - Adjust fonts, colors, and plugins as needed to match your preferences.

---

## 4. Previewing Your Site Locally

Before deploying, let's build and preview your site locally to ensure everything looks good.

### Step 1: Build and Serve the Site

1. **Run Build Command with Live Reloading**:
   ```powershell
   npx quartz build --serve
   ```

   - This command builds your site and starts a local server with live reloading.

2. **Access the Local Site**:
   - Open your web browser and navigate to: [http://localhost:8080](http://localhost:8080)

3. **Review Your Site**:
   - Browse through your notes to check for:
     - Proper formatting.
     - Correct link resolutions (especially for internal links).
     - Proper display of images and other media.

### Step 2: Troubleshoot Issues

If you encounter issues:

1. **Missing Links or Images**:
   - Ensure all internal links use relative paths and correct markdown syntax.
   - Check that all images are correctly referenced and located within the `content` directory.

2. **Formatting Errors**:
   - Review your markdown files for syntax errors.
   - Quartz supports standard markdown and Obsidian-flavored markdown. Ensure you're adhering to these standards.

3. **Console Errors**:
   - Check the PowerShell console and browser console for error messages. These can provide clues on what's going wrong.

### Step 3: Stop the Local Server

- When you're done reviewing:
  - Press `Ctrl + C` in PowerShell to stop the server.

---

## 5. Setting Up Git and GitHub

To deploy your site online and manage version control, we'll use Git and GitHub.

### Step 1: Create a GitHub Repository

1. **Log in to GitHub**:
   - Go to [https://github.com](https://github.com) and log in or create an account.

2. **Create a New Repository**:
   - Click the **New** button or navigate to [https://github.com/new](https://github.com/new).
   - **Repository Name**: Choose a name, e.g., `quartz-site`.
   - **Description**: (Optional) Add a brief description.
   - **Public/Private**: Choose according to your preference.
   - **Initialize Repository**: **Do not** add a README, .gitignore, or license file.
   - Click **Create repository**.

### Step 2: Connect Local Quartz to GitHub

1. **Navigate to Quartz Directory**:
   ```powershell
   cd D:\ACC_ID\OneDrive\Desktop\Quartz
   ```

2. **Initialize Git in the Directory** (if not already initialized):
   ```powershell
   git init
   ```

3. **Add All Files to Git**:
   ```powershell
   git add .
   ```

4. **Commit the Files**:
   ```powershell
   git commit -m "Initial commit with my Obsidian content"
   ```

5. **Set Remote Origin to Your GitHub Repository**:
   ```powershell
   git remote add origin https://github.com/your-username/quartz-site.git
   ```

   - Replace `your-username` and `quartz-site` with your actual GitHub username and repository name.

6. **Push to GitHub**:
   ```powershell
   git branch -M v4
   git push -u origin v4
   ```

   - Quartz uses the `v4` branch by default. This command pushes your local `v4` branch to GitHub and sets it as the upstream branch.

### Step 3: Verify on GitHub

1. **Visit Your Repository**:
   - Navigate to `https://github.com/your-username/quartz-site`.

2. **Check Files**:
   - Ensure all your files are present and correctly uploaded.

---

## 6. Deploying Your Site Online

Now that your site is on GitHub, let's deploy it so it's accessible online. We'll cover two options: **Cloudflare Pages** and **GitHub Pages**.

### Option 1: Cloudflare Pages

Cloudflare Pages offers fast and free hosting optimized for static sites like Quartz.

#### Step 1: Create a Cloudflare Account (If Not Already)

1. **Sign Up or Log In**:
   - Visit [https://dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up).

#### Step 2: Create a New Project

1. **Navigate to Cloudflare Pages**:
   - From the dashboard, select **Workers & Pages** > **Pages**.

2. **Create a New Project**:
   - Click **Create a project**.
   - Select **Connect to Git**.

3. **Connect GitHub Account**:
   - Authorize Cloudflare to access your GitHub repositories.
   - Select your `quartz-site` repository.

4. **Configure Build Settings**:
   - **Production Branch**: `v4`
   - **Framework Preset**: `None`
   - **Build Command**:
     ```bash
     git fetch --unshallow && npx quartz build
     ```
     - Adding `git fetch --unshallow &&` ensures full git history is available, which Quartz uses for timestamps.
   - **Build Output Directory**: `public`

5. **Start Deployment**:
   - Click **Save and Deploy**.
   - Wait for the build process to complete. This may take a few minutes.

6. **Access Your Site**:
   - Once deployed, Cloudflare provides a default domain like `your-project-name.pages.dev`.
   - Visit this URL to see your live site.

#### Step 3: Setting Up a Custom Domain (Optional)

1. **Navigate to Custom Domains**:
   - In your Cloudflare Pages project dashboard, go to the **Custom Domains** section.

2. **Add Custom Domain**:
   - Click **Set up a custom domain**.
   - Enter your domain name (e.g., `www.example.com`).

3. **Update DNS Records**:
   - Cloudflare will provide DNS records that you need to add to your domain registrar's DNS settings.
   - Typically, you'll add a CNAME record pointing your domain/subdomain to `your-project-name.pages.dev`.

4. **Verify and Secure**:
   - After updating DNS, Cloudflare will verify ownership and issue an SSL certificate.
   - Your site will now be accessible via your custom domain with HTTPS.

### Option 2: GitHub Pages

GitHub Pages allows you to host static sites directly from your GitHub repository.

#### Step 1: Configure GitHub Actions Workflow

1. **Create GitHub Actions Workflow File**:
   - In your local Quartz directory, create a new file at `.github/workflows/deploy.yml`.

   ```yaml
   name: Deploy Quartz site to GitHub Pages

   on:
     push:
       branches:
         - v4

   permissions:
     contents: read
     pages: write
     id-token: write

   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v4
           with:
             fetch-depth: 0 # Fetch all history for git info
         - uses: actions/setup-node@v4
           with:
             node-version: 22
         - name: Install Dependencies
           run: npm ci
         - name: Build Quartz
           run: npx quartz build
         - name: Deploy to GitHub Pages
           uses: peaceiris/actions-gh-pages@v3
           with:
             github_token: ${{ secrets.GITHUB_TOKEN }}
             publish_dir: ./public
   ```

2. **Commit and Push Workflow File**:
   ```powershell
   git add .github/workflows/deploy.yml
   git commit -m "Add GitHub Pages deployment workflow"
   git push
   ```

#### Step 2: Enable GitHub Pages

1. **Navigate to Repository Settings**:
   - Go to your repository on GitHub.
   - Click on the **Settings** tab.

2. **Configure GitHub Pages**:
   - In the sidebar, click **Pages**.
   - **Source**: Ensure it's set to **GitHub Actions**.
   - GitHub will automatically detect the workflow and start the deployment process.

3. **Access Your Site**:
   - Once deployment is complete, your site will be available at:
     ```
     https://your-username.github.io/quartz-site/
     ```
   - Visit this URL to view your live site.

#### Step 3: Setting Up a Custom Domain (Optional)

1. **Add Custom Domain in Settings**:
   - In the **Pages** settings, under **Custom domain**, enter your domain name (e.g., `www.example.com`).
   - GitHub will provide instructions and necessary DNS records to add to your domain registrar.

2. **Update DNS Records**:
   - Add an `A` or `CNAME` record as instructed by GitHub.
   - Wait for DNS propagation, which can take up to 24 hours.

3. **Enforce HTTPS**:
   - Once the custom domain is verified, enable **Enforce HTTPS** in the GitHub Pages settings.

---

## 7. Syncing Future Changes

Keeping your live site up-to-date with your local Obsidian vault is straightforward.

### Step 1: Make Changes Locally

- **Edit or Add Notes**:
  - Use Obsidian to create or modify notes within your vault (`D:\ACC_ID\OneDrive\Desktop\ObsidianVaults\General`).

### Step 2: Sync Changes to Quartz

1. **Copy Updated Content to Quartz**:
   ```powershell
   Copy-Item -Recurse -Force D:\ACC_ID\OneDrive\Desktop\ObsidianVaults\General\* D:\ACC_ID\OneDrive\Desktop\Quartz\content\
   ```

   - This overwrites the existing content in Quartz with your latest changes.

### Step 3: Commit and Push Changes

1. **Navigate to Quartz Directory**:
   ```powershell
   cd D:\ACC_ID\OneDrive\Desktop\Quartz
   ```

2. **Check Git Status**:
   ```powershell
   git status
   ```

   - Review the changes to ensure everything is as expected.

3. **Add Changes to Git**:
   ```powershell
   git add .
   ```

4. **Commit Changes**:
   ```powershell
   git commit -m "Update notes on [Date or Description]"
   ```

5. **Push to GitHub**:
   ```powershell
   git push
   ```

### Step 4: Automatic Deployment

- **Wait for Deployment**:
  - Depending on your hosting provider:
    - **Cloudflare Pages**: Automatically detects the new commit and rebuilds the site.
    - **GitHub Pages**: GitHub Actions workflow triggers and redeploys the site.

- **Verify Update**:
  - After the deployment process completes, refresh your website to see the updates live.

---

## 8. Additional Configurations

Fine-tune your Quartz site to better suit your needs and preferences.

### Step 1: Updating `quartz.config.ts`

1. **Open Configuration File**:
   - Located at `D:\ACC_ID\OneDrive\Desktop\Quartz\quartz.config.ts`.

2. **Set `baseUrl`**:
   - **For Cloudflare Pages**:
     ```ts
     baseUrl: "your-project-name.pages.dev",
     ```
   - **For Custom Domain**:
     ```ts
     baseUrl: "www.yourcustomdomain.com",
     ```

3. **Customize Theme**:
   - Modify colors, fonts, and other aesthetic settings under the `theme` section.

4. **Configure Plugins**:
   - Enable or disable plugins as needed.
   - For example, enable **full-text search** or **graph view** for enhanced navigation.

5. **Save and Commit Changes**:
   ```powershell
   git add quartz.config.ts
   git commit -m "Update configuration settings"
   git push
   ```

### Step 2: Using Custom Layouts and Components

- **Modify Layouts**:
  - Edit `quartz.layout.ts` to change the overall layout of your site.

- **Add Custom Components**:
  - Create custom React components in the `components` directory and integrate them into your layouts.

- **Update and Deploy**:
  - After making changes, rebuild and deploy your site as usual.

---

## 9. Troubleshooting

### Issue 1: Build Failures

- **Check Error Messages**:
  - Review the output in your terminal or hosting provider's build logs to identify issues.

- **Common Fixes**:
  - Ensure all dependencies are installed: `npm install`.
  - Verify Node.js and npm versions meet requirements.

### Issue 2: Broken Links or Missing Assets

- **Relative Paths**:
  - Ensure all internal links and asset references use correct relative paths.

- **Case Sensitivity**:
  - Remember that some hosting environments are case-sensitive. Ensure file and folder names match exactly.

### Issue 3: Deployment Delays

- **Build Queues**:
  - Hosting providers may have queues. Wait a few minutes and check again.

- **DNS Propagation**:
  - Changes to DNS settings can take up to 24-48 hours to propagate globally.

### Issue 4: Local Development Issues

- **Port Conflicts**:
  - If `localhost:8080` is in use, specify a different port:
    ```powershell
    npx quartz build --serve --port 3000
    ```

---

## 10. Conclusion

Congratulations! You've successfully published your Obsidian vault online using Quartz. With this setup:

- **Easy Content Management**: Continue using Obsidian for seamless note-taking and organization.
- **Effortless Deployment**: Simply push changes to GitHub to update your live site.
- **Customization**: Tailor your site's appearance and functionality to match your personal style and needs.

**Next Steps**:

- Explore more advanced features and plugins offered by Quartz.
- Integrate analytics to track your site's performance.
- Share your knowledge and creations with the world!

**Resources**:

- **Quartz Documentation**: [https://quartz.jzhao.xyz/](https://quartz.jzhao.xyz/)
- **Obsidian Documentation**: [https://help.obsidian.md/](https://help.obsidian.md/)
- **GitHub Guides**: [https://guides.github.com/](https://guides.github.com/)

---

If you encounter any issues or have further questions, feel free to consult the official documentation or seek assistance from the community. Happy publishing!