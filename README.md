<!-- ############################################# -->
<!-- Links -->

<!--
  Shields: https://shields.io/
  UTF8 Art: http://aa.en.utf8art.com/
  ASCII Art: https://www.asciiart.eu/
  ASCII Banners: https://patorjk.com/software/taag/
  Unicode Characters: https://www.amp-what.com/
  Braille Art: https://emojicombos.com/
  Spaces: https://jkorpela.fi/chars/spaces.html
  Color for SVG artworks: #6C757D / #F8F9FA
-- >

<!-- ############################################# -->
<!-- Header -->

<h1 align="center">
  <img width="150" src="./media/art-header.svg" alt="art-header">
  <img width="355" src="./media/header.svg" alt="header">
</h1>

This repository acts as a **shared submodule**, which is used by multiple repositories and acts as a central hub for the original localization files which can be found in the `locale` branch. Changes to this repository will affect all other repositories using it.

<!-- ############################################# -->
<!-- Main Area -->

## **Usage**

### **Adding the Submodule to Your Project**

To use the repository as a submodule in your own project:

```sh
# add the locale branch as a submodule
git submodule add -b locale https://github.com/nlobby4/fm-localization.git path/to/save/in
```

Then, to initialize and fetch the content of the submodule:

```sh
# initialize and fetch submodule content
git submodule update --init --recursive
# NOTE: if you clone a repository with submodules 
#   without using --recurse-submodules, you 
#   will also need to execute this command
#   to fetch the submodule content
```

If you need to manually update the submodule contents:

```sh
# fetches latest commit and merges changes
git submodule update --remote --merge
```

## **Automate Submodule Updates**

If you want to **automatically fetch updates** for this submodule when you commit changes the `main` branch of your project, you can use **GitHub Actions**.

### **1. Create the Workflow**

Create the following file in your project and push it to your repository:

```yaml
# .github/workflows/fm-localization.yml

name: fm-localization

on:
  push:
    branches:
      - main # when you push to your main branch
  workflow_dispatch: # manual trigger from GitHub Actions UI

jobs:
  update-submodule:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Parent Repository
        uses: actions/checkout@v4
        with:
          submodules: true # Fetches submodule content
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Check for Submodule Updates
        run: |
          # fetch the latest submodule commit
          git submodule update --remote --merge --recursive
          
          # check if submodule has changed
          git diff --exit-code || echo "Submodule has changes"
        
      - name: Update Submodule and Push Changes
        if: success() # Only runs if submodule has changes
        run: |
          git submodule update --remote --merge
          git add .
          git commit -m "Auto-update submodule to latest commit" || exit 0
          git push origin main
```

---

### **How It Works**

- **Triggered by Push:** The workflow is triggered by pushes to the `main` branch of the parent repository, and the submodule changes are handled within the workflow. The workflow checks for updates on the `locale` branch of the submodule.
- **Manual Trigger:** You can manually trigger the workflow from the GitHub Actions UI or by running the manual command if needed.

---

<!-- ############################################# -->
<!-- References -->
