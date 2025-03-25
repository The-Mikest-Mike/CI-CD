**Stable CI/CD pipeline** with **automated deployment, rollback capabilities, and version control**. 🚀

## 🔷 Current CI/CD Workflow

| **GitHub Profile**   | **Repositories** | **Purpose** |
|----------------------|-----------------|-------------|
| **TheMikestMike (Dev)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` | Active **development & testing**. New features & bug fixes happen here. |
| **TMMSoftware (Prod)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` (or with a `-prod` suffix) | **Production-ready repositories.** Only tested, stable code lands here. |
| **GitHub Pages** | `TMMSoftware/Webpage` | Hosts [https://tmmsoftware.github.io/](https://tmmsoftware.github.io/) |

📌 Each project has a **dev repo** in `TheMikestMike` and a **prod repo** in `TMMSoftware`.<br>
✅ Once code is validated, it moves to `TMMSoftware` & gets published.<br>
✅ GitHub Actions automate the movement from `TheMikestMike` → `TMMSoftware`.<br>
✅ `TMMSoftware` has a **centralized workflow** for all repositories.<br>

---

### 🔄 GitHub Actions Workflow

Every repository in **TheMikestMike (Dev)** has a GitHub Action that:

- ✅ Runs **tests** in release branches (**pre-prod step**).
- 🚀 If tests pass, the code is **pushed** to the corresponding **TMMSoftware (Prod)** repository.
- 🔖 **Auto-generates version tags** based on release status:  
  - Example: `1.0.0-alpha.1 → 1.0.0-beta.0` when ready for beta.
- 🌐 **Deploys** the `Webpage` repository to **GitHub Pages** at [https://tmmsoftware.github.io/](https://tmmsoftware.github.io/).
- 🔬 **Unit & API tests** run in `TheMikestMike` before merging to `TMMSoftware`.
- 🏗️ **Pre-release testing** is triggered in the `release/*` branch before deployment.

---

### 🔧 Feature Overview

| Feature                        | ✅ Status |
|--------------------------------|----------|
| Tests before production        | ✅ Yes (via `release/*`) |
| Auto-deploy only on success    | ✅ Yes (`GitHub Actions if: success()`) |
| Jira integration support       | ✅ Yes (via branch names & commits) |
| Full automation & rollback     | ✅ Ready to build |
| Docker / `.dmg` inclusion      | ⏳ (when needed) |

---

### 🛠️ Rollback Plan (Safe & Easy)

1. **Tag every production deployment**  
   ```sh
   git tag 1.0.0-alpha.1 && git push origin --tags
   ```
2. **Revert to a known-good tag in CI/CD**  
   ```sh
   git checkout 1.0.0-alpha.0  # Switch to stable version
   git tag 1.0.0-alpha.0 && git push origin --tags  # Re-deploy
   ```
3. **Optional GitHub Action**  
   - Set up a **manual workflow** to "Redeploy Tag XYZ" for fast rollback.

---

### 🌱 Branch Naming with Jira Integration

| Branch Type | Format Example | CI/CD Behavior |
|------------|---------------|---------------|
| `feature/` | `feature/PRJ-1234-add-signup-form` | Run lint + unit tests |
| `release/` | `release/1.0.0-alpha.1` | ✅ Run full test suite (gate for prod) |
| `hotfix/` | `hotfix/INC-9876-invalidPageMatch` | ✅ Run tests + auto-push to prod |
| `main` | `main` | ✅ Deploy on successful merge |
| `master` | _(Avoid both `main` & `master`)_ | ❌ Naming confusion |

---

### 🚀 CI/CD Automation Tasks

- 🛠️ **Tests gate all production deploys.**
- 🔄 **Every push to `release/*` triggers tests.**
- ✅ **Successful `release/* → main` merge auto-deploys to `TMMSoftware`.**
- 🔀 **Rollback = revert the commit or redeploy previous tag.**
- 📌 **Jira integration** keeps active branches & PRs linked to tickets.

---

### 🔬 Testing Strategy

| Option                     | ✅ Pros | ❌ Cons |
|----------------------------|--------|---------|
| On every commit            | Fast feedback loop | Can get noisy & redundant |
| On every PR to `release/*` | Focused validation, good for teamwork | Adds review overhead |
| **On every push to `release/*` ✅** | Best balance – ensures everything in `release/*` is test-validated | None if tests are quick |
| On merge to `main` | Final safety net | Too late if you're auto-deploying |


graph TD

  subgraph Developer Workflow
    A1([🧑‍💻 feature/PRJ-1234-login-fix]) --> B1([📦 release/1.0.0-alpha.1])
  end

  subgraph CI/CD Pipeline
    B1 --> C1([✅ Run Tests])
    C1 -->|Pass| D1([🚀 Deploy to Prod GitHub Repo])
    C1 -->|Fail| E1([🛑 Stop Deployment])
  end

  subgraph Production
    D1 --> F1([🔵 main (production)])
    F1 --> G1([🏷️ Tag v1.0.0-alpha.1])
  end

  subgraph Emergency Patch
    H1([🛠 hotfix/INC-567-crash]) --> F1
    H1 --> B1
  end


🔥 **Tests should run on:**
- ✅ **Pushes to `release/*`**
- ✅ **Before merging `release/*` → `main`**
- ✅ **Pull Requests when growing the team and collaborating with others**

How to Read It:
feature/* branches → go into release/* (CI runs here)

If tests pass ✅ → code is auto-deployed to prod repo

Then, release/* is merged into main → tagged with official version

Hotfixes go straight from hotfix/* → main → release/* if needed

