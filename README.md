**Stable CI/CD pipeline** with **automated deployment, rollback capabilities, and version control**. 🚀

Index:

1. Separation between Dev and Prod GitHub profiles

2. GitFlow implementation using feature/, release/, hotfix/, main

3. Use of release/* as your CI/CD deployment trigger (pre-prod gate ✅)

4. GitHub Actions defined only in Dev repos

5. Auto-deploy to TMMSoftware (Prod) on successful validation

6. Staged rollout strategy mentioned

7. Hotfix strategy with direct deploy to main + optional merge to release/*

8. Jira integration via branch naming convention

9. Tag-based rollback workflow

10. Mermaid diagram

11. Dev onboarding with Git + GitFlow setup (macOS-focused)


# 🚀 GitFlow CI/CD Workflow Guide for macOS Projects

This document outlines the complete step-by-step workflow for developers working on TMMSoftware products using GitFlow and a CI/CD pipeline.

---

## 🔷 Current CI/CD Workflow

| **GitHub Profile**   | **Repositories** | **Purpose** |
|----------------------|------------------|-------------|
| **TheMikestMike (Dev)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` | Active **development** using GitFlow: `feature`, `release`, `hotfix` branches. Code is tested and integrated here. |
| **TMMSoftware (Prod)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` | **Production-ready repositories.** Only tested, CI/CD-passed code lands here. |
| **GitHub Pages** | `TMMSoftware/Webpage` | Hosts [https://tmmsoftware.github.io/](https://tmmsoftware.github.io/) — reflects the latest deployed site. |

📌 Each project has a **Dev → Prod repo flow**.<br>
✅ Only **code merged into `release/*` in TheMikestMike** and validated via CI/CD gets auto-deployed to the `main` branch of the matching `TMMSoftware` repo.<br>
✅ GitHub Actions live in **Dev repos**, not Prod.<br>
✅ `TMMSoftware` remains the **single source of truth** for production code.<br>

---

### 🔄 GitHub Actions Workflow

Each Dev repo under `TheMikestMike` includes GitHub Actions that:

- ✅ Run **validation jobs** (tests, linting, CI checks) on every push to `release/*`.
- ✅ If successful, **deploy code automatically** to the matching `main` branch in the `TMMSoftware` production repository.
- 🔖 **Tag production-ready releases** (e.g., `v1.0.0-alpha.1`).
- 🌐 Auto-deploy the **Webpage** to GitHub Pages from `TMMSoftware/Webpage`.
- 🛠️ Handle `hotfix/*` branches directly from `main` when emergency production fixes are needed.

---

### 🔧 Feature Overview

| Feature                        | ✅ Status |
|--------------------------------|----------|
| Tests before production        | ✅ Yes (`release/*` branch gated) |
| Auto-deploy only on success    | ✅ Yes (via `if: success()`) |
| Jira integration support       | ✅ Yes (via branch naming: `PRJ-1234`) |
| Full automation & rollback     | ✅ In place (via tags + CI gate) |
| Canary/staged deployment       | ✅ Planned (via clusters/flags) |
| Docker / `.dmg` inclusion      | ⏳ (To be added per repo needs) |

---

### 🛠️ Rollback Plan (Safe & Easy)

1. **Tag every production deployment**  
   ```sh
   git tag v1.0.0-alpha.1 && git push origin --tags
   ```
2. **Revert to a known-good tag in CI/CD**  
   ```sh
   git checkout v1.0.0-alpha.0
   git push origin main  # Reverts production repo
   ```
3. **Optional GitHub Action**  
   - Create a manual job: **Redeploy Tag XYZ** for clean rollback.

---

### 🌱 Branch Naming with Jira Integration

| Branch Type | Format Example | CI/CD Behavior |
|-------------|----------------|----------------|
| `feature/`  | `feature/PRJ-1234-add-signup-form` | 🧪 Lint + unit test (optional) |
| `release/`  | `release/1.0.0-alpha.1`            | ✅ Full test suite + auto-push to prod |
| `hotfix/`   | `hotfix/INC-9876-fix-login`        | ✅ Test + deploy directly to prod |
| `main`      | `main`                             | ✅ Final production truth; accepts merges from `release/*` |

---

### 🚀 CI/CD Automation Summary

- 🧪 Tests always gate production deployments.
- 📦 `release/*` branches are the **deployment trigger**.
- 🔁 `main` reflects only **tested, tagged production code**.
- 🔀 Rollbacks happen by **reverting to tags**.
- 📲 Jira integration shows branch → ticket linkage.

---

### 🔬 Testing Strategy

| When To Test                | ✅ Pros                     | ❌ Cons                          |
|-----------------------------|-----------------------------|----------------------------------|
| On every commit             | Fast feedback               | Noisy for large teams            |
| On PR to `release/*`        | Great collaboration filter  | Adds review overhead             |
| ✅ On push to `release/*`    | Balanced & production-safe  | Slight test redundancy (acceptable) |
| On merge to `main`          | Safety net                  | Too late for real CD enforcement |

---

🔥 **Tests should run on:**
- ✅ Pushes to `release/*`
- ✅ Pull requests to `release/*`
- ✅ Before merging `release/* → main`

💡 **Main is always the source of truth**, but `release/*` is the gatekeeper to production.

---

This workflow guarantees:
- 🔐 Secure, test-gated production releases
- 🧪 Reliable UAT + Canary strategies
- 🔄 Easy rollback & patching
- 📖 Developer onboarding consistency

✅ Last updated: 2025-03

