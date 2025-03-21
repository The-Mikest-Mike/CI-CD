## 🔷 Current CI/CD Workflow

| **GitHub Profile**   | **Repositories** | **Purpose** |
|----------------------|-----------------|-------------|
| **TheMikestMike (Dev)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` | Active **development & testing**. New features & bug fixes happen here. |
| **TMMSoftware (Prod)** | `WebQuiz-Devnet`, `Gesture-Control-System`, `File-Organizer`, `Webpage` (or with a `-prod` suffix) | **Production-ready repositories.** Only tested, stable code lands here. |
| **GitHub Pages** | `TMMSoftware/Webpage` | Hosts [https://tmmsoftware.github.io/](https://tmmsoftware.github.io/) |


📌 Each project has a dev repo in TheMikestMike and a prod repo in TMMSoftware.<br>
✅ Once code is validated, it moves to TMMSoftware & gets published.<br>
✅ GitHub Actions automate the movement from TheMikestMike → TMMSoftware.<br>
✅ Prod Github has configured a central workflow that applies to all repositories.<br>


