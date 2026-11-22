# Uploading this project to GitHub

1. Create a new GitHub repository (suggested name: `PH3043-ZBoson-Mass-Reconstruction`) without auto-generating a README.
2. Extract the supplied GitHub-ready ZIP. Open a terminal **inside `PH3043_ZBoson/`**.
3. Run:

```bash
git init
git add .
git commit -m "Initial public PH3043 Z-boson reconstruction project"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/PH3043-ZBoson-Mass-Reconstruction.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your GitHub username. GitHub authentication is required. The upload is **not** performed automatically.

**Before making the repository public:**
- Check that CERN dataset redistribution is compatible with the applicable record terms.
- Review notebooks and plots for anything you do not wish to publish (including execution outputs).
- Edit the README's authorship and citation statements to match your project requirements.
- Optionally keep large raw/derived CSVs out of Git history and instead document their CERN download links; if changing the data policy, edit the README accordingly.
- No software license is automatically added; select one only if desired.

For an existing GitHub repository, do not run `git init` again; copy changed files into the working tree, then commit and push.
