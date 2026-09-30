## 📄 File 1: `1-samkelsiwe-frontend.md`

```markdown
# Samkelsiwe Hlatshwayo — UI/UX & Frontend Lead
**Student Number:** ST10442364
**Branch:** `feature/frontend-public-website-setup`
**Working Directory:** `src/UniCribz.Web/`

---

## ✅ What Has Already Been Done (By Siyanda)

Everything in this section is **already in place** — you do NOT need to create it.

| Item | Location |
|------|----------|
| `UniCribz.Web` project created | `src/UniCribz.Web/UniCribz.Web.csproj` |
| Project reference to `UniCribz.Shared` added | (auto-configured) |
| All NuGet packages installed | Razor Runtime Compilation, Http, Redis Cache |
| Folder structure created | All folders under `src/UniCribz.Web/` exist |
| Placeholder controller files created | `Controllers/*.cs` |
| Placeholder view files created | `Views/**/*.cshtml` |
| Placeholder ViewModels created | `ViewModels/*.cs` |
| `_Layout.cshtml` replaced with **UniCribz theme** | `Views/Shared/_Layout.cshtml` |
| `site.css` updated with **color scheme** | `wwwroot/css/site.css` |
| `Program.cs` configured (MVC, HttpClient, Session) | `src/UniCribz.Web/Program.cs` |
| `appsettings.json` points to real API URL | `Api:BaseUrl` = `https://localhost:7285` |

**Every placeholder file has a `// TODO` comment telling you what to do.** Open the file in Visual Studio and start coding.

---

## 🎨 Design System (Already Applied)

| Role | Color | Hex |
|------|-------|-----|
| Primary | Dark Blue | `#2C3E50` |
| Secondary | Teal Green | `#18BC9C` |
| Background | Off-White | `#F8F9FA` |
| Text | Dark Grey | `#333333` |

**Bootstrap 5.3** and **Font Awesome 6** are already linked in `_Layout.cshtml`.

**Prototype reference:** https://github.com/SiyandaNduze/UniCribz-Web-app.git

---

## 🌐 Actual Dev URLs (Use These)

| Service | URL |
|---------|-----|
| Web (HTTPS) | `https://localhost:7105` |
| Web (HTTP) | `http://localhost:5294` |
| API (HTTPS) | `https://localhost:7285` |
| API (HTTP) | `http://localhost:5125` |
| Swagger UI | `https://localhost:7285/swagger` |

---

## 🎯 Where YOU Need to Start

### Step 0: Get the project running locally

```bash
git clone https://github.com/YOUR-USERNAME/INSY7315-2026-MOTIVATION.git
cd INSY7315-2026-MOTIVATION
git checkout develop
git pull
git checkout -b feature/frontend-public-website-setup

# Run setup (installs packages, creates folders)
powershell -ExecutionPolicy Bypass -File setup/00-run-all.ps1

# Verify build
dotnet build
```

Open `UniCribz.sln` in Visual Studio 2022.

### Step 1: Build the layout + navigation (FIRST — blocks everything else)

**File:** `src/UniCribz.Web/Views/Shared/_Layout.cshtml`

The layout already has a themed navbar. Your job:
- Update the navbar links to be role-aware using `@if (User.Identity.IsAuthenticated)`:
  - **Anonymous** → "Find Accommodation", "Login", "Register"
  - **Tenant** → "Find Accommodation", "My Dashboard", "Payments", "Maintenance"
  - **Admin** → "Admin Dashboard", "Properties", "Applications", "Reports"
- Add Font Awesome icons to the links
- Add a footer with company info and links

### Step 2: Public property search (SECOND — the visitor's entry point)

**Files:**
- `src/UniCribz.Web/Controllers/PropertyController.cs`
- `src/UniCribz.Web/ViewModels/PropertySearchViewModel.cs`
- `src/UniCribz.Web/Views/Property/Search.cshtml`
- `src/UniCribz.Web/Views/Property/Details.cshtml`

**What to build:**
- Search form with filters: University, price range, gender, room type, amenities
- Property cards grid — image, price, location, distance to university
- Map view embedded (Anothile will provide the map component)
- "Book Viewing" and "Apply" buttons on each property

### Step 3: Application form (multi-step)

**Files:**
- `Controllers/ApplicationController.cs`
- `Views/Application/Create.cshtml`

**What to build:**
- 3-step wizard: Personal Info → Preferences → Upload Documents
- Use Bootstrap tabs or a custom stepper
- On submit: POST to `UniCribz.Api` (endpoint will be provided by Siyanda)

### Step 4: Account pages

**Files:**
- `Controllers/AccountController.cs`
- `Views/Account/Login.cshtml`
- `Views/Account/Register.cshtml`

**What to build:**
- Login form → POST to `https://localhost:7285/api/auth/login`
- Register form → POST to `https://localhost:7285/api/auth/register`
- Store JWT in a cookie / session
- Redirect to role-appropriate dashboard after login

### Step 5: Tenant dashboard

**Files:**
- `Controllers/TenantController.cs`
- `Views/Tenant/Dashboard.cshtml`
- `ViewModels/TenantDashboardViewModel.cs`

**What to build:**
- Dashboard cards: Outstanding Balance, Active Lease, Open Maintenance Requests
- Tables: Payment History, Recent Announcements
- Action buttons: "Upload Proof of Payment", "Log Maintenance Request"

### Step 6: Admin dashboard

**Files:**
- `Controllers/AdminController.cs`
- `Views/Admin/Dashboard.cshtml`
- `ViewModels/AdminDashboardViewModel.cs`

**What to build:**
- Metrics cards: Occupancy Rate, Open Applications, Outstanding Payments
- Chart (Chart.js) for income/occupancy trends
- Data tables for Applications, Maintenance, Complaints

---

## 🔗 What You Depend On (From Teammates)

| From | What | When |
|------|------|------|
| **Siyanda** | Auth API endpoints (`/api/auth/register`, `/api/auth/login`) | Week 1-2 |
| **Siyanda** | Property API endpoints (`/api/properties`, `/api/properties/{id}`) | Week 2 |
| **Siyabonga** | Entity shapes (Property, Room, Application, Lease) so ViewModels match | Week 1 |
| **Anothile** | Google Maps JavaScript component to embed | Week 3 |

**Interim strategy:** Until the API is live, use **mock data** in your ViewModels so you can build the UI in parallel. Replace mocks with real API calls when endpoints are ready.

---

## 🔁 Daily Git Workflow

```bash
# Start of session
git checkout develop
git pull
git checkout feature/frontend-public-website-setup
git merge develop

# During work (commit often)
git add .
git commit -m "feat(web): add property search form"
git push

# When a piece is done → open PR on GitHub: your-branch → develop
```

**Commit conventions:**
```
feat(web):     New UI feature
fix(web):      UI bug fix
style(web):    CSS/layout changes
refactor(web): Reorganize views/controllers
```

---

## ❓ If Something Breaks

1. Delete `bin/` and `obj/` in `src/UniCribz.Web/`, then `dotnet build`
2. Run `dotnet dev-certs https --trust` if you see HTTPS warnings
3. Ask Siyanda if the API isn't reachable

---

## 📚 Reference Material

- **Prototype:** https://github.com/SiyandaNduze/UniCribz-Web-app.git
- **Project doc:** `MOTIVATION-INSY7315.pdf` (color scheme, layout, user stories)
- **Design class diagram:** Page 24 of project doc