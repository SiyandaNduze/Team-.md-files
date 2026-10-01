## 📄 File 1: `docs/team/1-samkelsiwe-frontend.md`

```markdown
# Samkelsiwe Hlatshwayo — UI/UX & Frontend Lead
**Student Number:** ST10442364
**Branch:** `feature/frontend-public-website-setup`
**Working Directory:** `src/UniCribz.Web/`

---

## ✅ What Has Already Been Done

### By Siyanda (Scaffolding)
| Item | Location |
|------|----------|
| `UniCribz.Web` project created | `src/UniCribz.Web/UniCribz.Web.csproj` |
| Project reference to `UniCribz.Shared` | Auto-configured |
| NuGet packages installed | Razor Runtime Compilation, Http, Redis Cache |
| Folder structure created | All folders exist |
| Placeholder controllers + views + ViewModels | All created |
| `_Layout.cshtml` replaced with **UniCribz theme** | `Views/Shared/_Layout.cshtml` |
| `site.css` with **color scheme** | `wwwroot/css/site.css` |
| `Program.cs` configured (MVC, HttpClient, Session) | `src/UniCribz.Web/Program.cs` |
| `appsettings.json` → `Api:BaseUrl` = `http://localhost:5125` | Configured |

### By Siyanda (Backend API — Fully Live)
**The entire API is now complete and tested.** Everything you need is available.

| Module | Endpoints Available |
|--------|---------------------|
| Auth | register, login, me, refresh, logout |
| Property | search, filter, get, create, update, delete, rooms, add room |
| Application | submit, list, get, approve, upload document |
| Viewing | book, list, approve, **available slots** |
| Lease | create, sign, terminate, list, mine |
| Payment | initiate, upload proof, verify, receipt, list |
| Maintenance | submit, list, mine, assign, status |
| Complaint | submit, list, mine, resolve |
| Announcement | public list, admin list, create, delete |
| Notification | list, unread count, mark read |
| Report | occupancy, payments, maintenance, dashboard |

---

## 🎨 Design System (Already Applied)

| Role | Color | Hex |
|------|-------|-----|
| Primary | Dark Blue | `#2C3E50` |
| Secondary | Teal Green | `#18BC9C` |
| Background | Off-White | `#F8F9FA` |
| Text | Dark Grey | `#333333` |

**Bootstrap 5.3** + **Font Awesome 6** linked in `_Layout.cshtml`.

**Prototype:** https://github.com/SiyandaNduze/UniCribz-Web-app.git

---

## 🌐 Actual Dev URLs

| Service | URL |
|---------|-----|
| Web (HTTPS) | `https://localhost:7105` |
| Web (HTTP) | `http://localhost:5294` |
| **API (HTTP)** | `http://localhost:5125` ← **use this** |
| API (HTTPS) | `https://localhost:7285` |
| Swagger UI | `http://localhost:5125/swagger` |
| Health | `http://localhost:5125/health` |

---

## 🎯 Where YOU Need to Start

### Step 0: Get the project running

```bash
git clone https://github.com/YOUR-USERNAME/INSY7315-2026-MOTIVATION.git
cd INSY7315-2026-MOTIVATION
git checkout develop
git pull
git checkout -b feature/frontend-public-website-setup

# Start DB + Redis
docker-compose up -d

# Terminal 1: Run the API
cd src/UniCribz.Api
dotnet run
# → http://localhost:5125/swagger

# Terminal 2: Run the Web app
cd src/UniCribz.Web
dotnet run
```

**Test accounts** (from seeder):
- Admin: `admin@unicribz.co.za` / `Admin@123`
- Tenant: `tenant@unicribz.co.za` / `Admin@123`
- Visitor: `visitor@unicribz.co.za` / `Admin@123`

### Step 1: Role-aware layout + navigation

**File:** `src/UniCribz.Web/Views/Shared/_Layout.cshtml`

- Update navbar links based on `User.Identity.IsAuthenticated` and role
- Anonymous → "Find Accommodation", "Login", "Register"
- Tenant → "Find Accommodation", "Dashboard", "Payments", "Maintenance"
- Admin → "Admin Dashboard", "Properties", "Applications", "Reports"
- Add Font Awesome icons
- Build footer

### Step 2: Public property search

**Files:**
- `Controllers/PropertyController.cs`
- `ViewModels/PropertySearchViewModel.cs`
- `Views/Property/Search.cshtml`
- `Views/Property/Details.cshtml`

**Calls to build:**
- `GET /api/properties` (with `?city=`, `?maxPrice=`)
- `GET /api/properties/{id}`
- `GET /api/properties/{id}/rooms`

**What to build:** Search form + property cards + details page + "Book Viewing" / "Apply" buttons.

### Step 3: Application form (multi-step)

**Files:**
- `Controllers/ApplicationController.cs`
- `Views/Application/Create.cshtml`

**Calls:**
- `POST /api/applications`
- `POST /api/applications/{id}/documents`

**What to build:** 3-step wizard → Personal Info → Preferences → Documents.

### Step 4: Account pages

**Files:**
- `Controllers/AccountController.cs`
- `Views/Account/Login.cshtml`
- `Views/Account/Register.cshtml`

**Calls:**
- `POST /api/auth/login` → returns `{ token, expiresAt, userId, fullName, email, role }`
- `POST /api/auth/register`

**What to build:** Forms → store JWT in cookie/session → redirect by role.

### Step 5: Tenant dashboard

**Files:**
- `Controllers/TenantController.cs`
- `Views/Tenant/Dashboard.cshtml`
- `ViewModels/TenantDashboardViewModel.cs`

**Calls:**
- `GET /api/leases/mine`
- `GET /api/payments/mine`
- `GET /api/maintenance/mine`
- `GET /api/complaints/mine`
- `GET /api/announcements`
- `GET /api/notifications`
- `GET /api/notifications/unread-count`

**What to build:** Cards for Outstanding Balance, Active Lease, Open Maintenance. Tables for Payment History, Announcements. Buttons for "Upload Proof", "Log Maintenance".

### Step 6: Admin dashboard

**Files:**
- `Controllers/AdminController.cs`
- `Views/Admin/Dashboard.cshtml`
- `ViewModels/AdminDashboardViewModel.cs`

**Calls:**
- `GET /api/reports/dashboard` (one call returns everything)
- `GET /api/reports/occupancy`
- `GET /api/reports/payments`
- `GET /api/reports/maintenance`

**What to build:** Metric cards + Chart.js trends + data tables.

---

## 🔗 What You Depend On (Updated)

| From | What | Status |
|------|------|--------|
| **Siyanda** | Auth API endpoints | ✅ Done |
| **Siyanda** | Property API endpoints | ✅ Done |
| **Siyanda** | All other backend endpoints | ✅ Done |
| **Siyabonga** | Entity shapes → ViewModel shapes | ✅ Done |
| **Anothile** | Google Maps JS component | ⏳ Pending |

**You're not blocked on anything.** The API works. Start building.

---

## 🔁 Daily Git Workflow

```bash
git checkout develop && git pull
git checkout feature/frontend-public-website-setup
git merge develop

# work...
git add . && git commit -m "feat(web): add property search form"
git push
```

**Commit conventions:** `feat(web):` | `fix(web):` | `style(web):` | `refactor(web):`

---

## ❓ If Something Breaks

1. `dotnet clean && dotnet build` in `src/UniCribz.Web/`
2. `dotnet dev-certs https --trust` for HTTPS warnings
3. Is the API running? Check `http://localhost:5125/swagger`
4. If API returns 401, your token expired — re-login

---

## 📚 Reference

- **Swagger (test endpoints live):** http://localhost:5125/swagger
- **Prototype:** https://github.com/SiyandaNduze/UniCribz-Web-app.git
- **Project doc:** `MOTIVATION-INSY7315.pdf`