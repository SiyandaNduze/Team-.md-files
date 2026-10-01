## 📄 File 5: `README.md` (Root of Repo)

Replace your repo's root `README.md` with this (short, links to full docs):

```markdown
# UniCribz — Student Accommodation Management System

> **Module:** INSY7315 | **Assessment:** Task 1 | **Group:** MOTIVATION | **Programme:** BCAD3
> **Submission:** 17 August 2026

A cloud-native student accommodation platform for South Africa that eliminates fragmented manual processes (spreadsheets, paper, WhatsApp) and provides a unified digital experience for visitors, tenants, and admins.

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/YOUR-USERNAME/INSY7315-2026-MOTIVATION.git
cd INSY7315-2026-MOTIVATION

# 2. Verify .NET 8
dotnet --list-sdks     # Should show 8.0.x

# 3. Run setup
powershell -ExecutionPolicy Bypass -File setup/00-run-all.ps1

# 4. Start local dependencies
docker-compose up -d

# 5. Build & run
dotnet build
cd src/UniCribz.Api && dotnet run
# (New terminal)
cd src/UniCribz.Web && dotnet run
```

**Dev URLs:**
| Service | URL |
|---------|-----|
| API (Swagger) | `https://localhost:7285/swagger` |
| API (Health) | `https://localhost:7285/health` |
| Web Frontend | `https://localhost:7105` |

---

## 👥 Team & Roles

| Name | Role | Branch Prefix | Working Directory |
|------|------|---------------|-------------------|
| **Siyanda Nduze** (ST10440706) | Backend Architect & DevOps | `feature/backend-*`, `feature/ci-cd-*` | `src/UniCribz.Api/`, `src/UniCribz.Shared/`, `.github/` |
| **Samkelsiwe Hlatshwayo** (ST10442364) | UI/UX & Frontend | `feature/frontend-*` | `src/UniCribz.Web/` |
| **Siyabonga Cebekhulu** (ST10440807) | Database & Backend | `feature/database-*` | `src/UniCribz.Data/` |
| **Anothile Bhengu** (ST10440981) | Integrations & Services | `feature/payment-*`, `feature/google-maps-*` | `src/UniCribz.Api/Notifications/`, `Services/` |

**Per-member instructions:** See `docs/team/1-samkelsiwe-frontend.md`, `2-anothile-integration.md`, `3-siyabonga-database.md`, `4-siyanda-backend-devops.md`.

---

## Full File Structure

```
UniCribz-Student-Accommodation-System/
│
├── .github/
│   └── workflows/
│       ├── ci.yml                              # Lint, build, test on every push
│       ├── deploy-staging.yml                  # Deploy to Azure staging (guarded)
│       └── deploy-production.yml               # Blue-green deploy (guarded)
│
├── docs/
│   ├── diagrams/
│   ├── deployment/
│   ├── security/
│   └── team/
│
├── infrastructure/
│   ├── arm-templates/
│   ├── bicep/
│   └── scripts/
│
├── setup/
│   ├── 00-run-all.ps1
│   ├── 01-add-references.ps1
│   ├── 02-add-nuget-packages.ps1
│   ├── 03-create-folders.ps1
│   ├── 04-create-all-classes.ps1
│   └── README.md
│
├── src/
│   │
│   ├── UniCribz.Api/                           # ASP.NET Core Web API (Backend)
│   │   ├── Controllers/
│   │   │   ├── AuthController.cs               # register, login, me, refresh, logout
│   │   │   ├── PropertyController.cs           # search, get, create, update, delete, rooms
│   │   │   ├── ApplicationController.cs        # submit, list, get, approve, upload doc
│   │   │   ├── ViewingController.cs            # book, list, approve, available slots
│   │   │   ├── LeaseController.cs              # create, sign, terminate, list, mine
│   │   │   ├── PaymentController.cs            # initiate, upload proof, verify, receipt
│   │   │   ├── MaintenanceController.cs        # submit, list, mine, assign, status
│   │   │   ├── ComplaintController.cs          # submit, list, mine, resolve
│   │   │   ├── AnnouncementController.cs       # public list, admin list, create, delete
│   │   │   ├── NotificationController.cs       # list, unread count, mark read
│   │   │   ├── ReportController.cs             # occupancy, payments, maintenance, dashboard
│   │   │   └── GoogleMapsController.cs         # geocode, nearby
│   │   │
│   │   ├── Services/
│   │   │   ├── Interfaces/
│   │   │   │   ├── IUserService.cs
│   │   │   │   ├── IPropertyService.cs
│   │   │   │   ├── IApplicationService.cs
│   │   │   │   ├── IViewingService.cs
│   │   │   │   ├── ILeaseService.cs
│   │   │   │   ├── IPaymentService.cs
│   │   │   │   ├── IMaintenanceService.cs
│   │   │   │   ├── IComplaintService.cs
│   │   │   │   ├── IAnnouncementService.cs
│   │   │   │   ├── INotificationService.cs
│   │   │   │   ├── IReportService.cs
│   │   │   │   └── IGoogleMapsService.cs
│   │   │   ├── UserService.cs
│   │   │   ├── PropertyService.cs
│   │   │   ├── ApplicationService.cs
│   │   │   ├── ViewingService.cs
│   │   │   ├── LeaseService.cs
│   │   │   ├── PaymentService.cs
│   │   │   ├── MaintenanceService.cs
│   │   │   ├── ComplaintService.cs
│   │   │   ├── AnnouncementService.cs
│   │   │   ├── NotificationService.cs
│   │   │   ├── ReportService.cs
│   │   │   └── GoogleMapsService.cs            # (pending: Anothile)
│   │   │
│   │   ├── Notifications/                      # (pending: Anothile)
│   │   │   ├── INotificationStrategy.cs
│   │   │   ├── INotificationManager.cs
│   │   │   ├── NotificationManager.cs
│   │   │   ├── EmailNotificationStrategy.cs
│   │   │   ├── SMSNotificationStrategy.cs
│   │   │   └── InAppNotificationStrategy.cs
│   │   │
│   │   ├── Observers/                          # (placeholders)
│   │   │   ├── IStatusObserver.cs
│   │   │   ├── RoomStatusUpdater.cs
│   │   │   ├── ReportServiceObserver.cs
│   │   │   └── AdminDashboardObserver.cs
│   │   │
│   │   ├── Facades/
│   │   │   └── TenantDashboardFacade.cs
│   │   │
│   │   ├── DTOs/
│   │   │   ├── Requests/
│   │   │   │   ├── LoginRequest.cs
│   │   │   │   ├── RegisterRequest.cs
│   │   │   │   ├── CreatePropertyRequest.cs
│   │   │   │   ├── CreateRoomRequest.cs
│   │   │   │   ├── CreateApplicationRequest.cs
│   │   │   │   ├── BookViewingRequest.cs
│   │   │   │   ├── CreateLeaseRequest.cs
│   │   │   │   ├── InitiatePaymentRequest.cs
│   │   │   │   ├── UploadProofOfPaymentRequest.cs
│   │   │   │   ├── VerifyPaymentRequest.cs
│   │   │   │   ├── CreateMaintenanceRequest.cs
│   │   │   │   ├── AssignMaintenanceRequest.cs
│   │   │   │   ├── UpdateMaintenanceStatusRequest.cs
│   │   │   │   ├── CreateComplaintRequest.cs
│   │   │   │   ├── UpdateComplaintStatusRequest.cs
│   │   │   │   ├── CreateAnnouncementRequest.cs
│   │   │   │   └── UploadDocumentRequest.cs
│   │   │   │
│   │   │   └── Responses/
│   │   │       ├── AuthResponse.cs
│   │   │       ├── PropertyResponse.cs
│   │   │       ├── RoomResponse.cs
│   │   │       ├── ApplicationResponse.cs
│   │   │       ├── ViewingResponse.cs
│   │   │       ├── SlotResponse.cs
│   │   │       ├── LeaseResponse.cs
│   │   │       ├── PaymentResponse.cs
│   │   │       ├── MaintenanceResponse.cs
│   │   │       ├── MaintenanceReportResponse.cs
│   │   │       ├── ComplaintResponse.cs
│   │   │       ├── AnnouncementResponse.cs
│   │   │       ├── NotificationResponse.cs
│   │   │       ├── OccupancyReportResponse.cs
│   │   │       ├── PaymentReportResponse.cs
│   │   │       ├── DashboardReportResponse.cs
│   │   │       ├── DashboardResponse.cs
│   │   │       └── GeocodeResponse.cs
│   │   │
│   │   ├── Middleware/
│   │   │   ├── ErrorHandlingMiddleware.cs      # global exception → JSON
│   │   │   ├── RequestLoggingMiddleware.cs     # Serilog timing
│   │   │   └── RateLimitingMiddleware.cs       # 60 req/min per IP
│   │   │
│   │   ├── Validators/
│   │   │   ├── LoginRequestValidator.cs
│   │   │   ├── RegisterRequestValidator.cs
│   │   │   └── CreateApplicationRequestValidator.cs
│   │   │
│   │   ├── Extensions/
│   │   │   ├── ServiceCollectionExtensions.cs  # DI: all services registered
│   │   │   └── ClaimsPrincipalExtensions.cs
│   │   │
│   │   ├── Mappings/
│   │   │   └── MappingProfile.cs
│   │   │
│   │   ├── Properties/
│   │   │   └── launchSettings.json             # HTTP: 5125, HTTPS: 7285
│   │   │
│   │   ├── appsettings.json
│   │   ├── appsettings.Development.json
│   │   ├── UniCribz.Api.http                   # ~60 integration tests
│   │   ├── Program.cs
│   │   └── UniCribz.Api.csproj
│   │
│   ├── UniCribz.Data/                          # EF Core Data Layer
│   │   ├── Entities/
│   │   │   ├── User.cs                         # abstract, TPH
│   │   │   ├── Visitor.cs
│   │   │   ├── Tenant.cs
│   │   │   ├── Admin.cs
│   │   │   ├── University.cs
│   │   │   ├── Property.cs
│   │   │   ├── PropertyImage.cs
│   │   │   ├── Amenity.cs
│   │   │   ├── Room.cs
│   │   │   ├── Application.cs
│   │   │   ├── ApplicationDocument.cs
│   │   │   ├── Viewing.cs
│   │   │   ├── Lease.cs
│   │   │   ├── Payment.cs
│   │   │   ├── MaintenanceRequest.cs
│   │   │   ├── MaintenancePhoto.cs
│   │   │   ├── Complaint.cs
│   │   │   ├── Announcement.cs
│   │   │   ├── Notification.cs
│   │   │   ├── Document.cs
│   │   │   └── InventoryItem.cs
│   │   │
│   │   ├── Enums/
│   │   │   ├── UserRole.cs
│   │   │   ├── RoomStatus.cs
│   │   │   ├── ApplicationStatus.cs
│   │   │   ├── PaymentStatus.cs
│   │   │   ├── MaintenanceStatus.cs
│   │   │   ├── ComplaintStatus.cs
│   │   │   ├── LeaseStatus.cs
│   │   │   └── ViewingStatus.cs
│   │   │
│   │   ├── Context/
│   │   │   └── UniCribzDbContext.cs            # 22 DbSets, TPH, ApplyConfigurationsFromAssembly
│   │   │
│   │   ├── Configurations/
│   │   │   ├── UserConfiguration.cs
│   │   │   ├── PropertyConfiguration.cs
│   │   │   ├── PropertyImageConfiguration.cs
│   │   │   ├── RoomConfiguration.cs
│   │   │   ├── AmenityConfiguration.cs
│   │   │   ├── UniversityConfiguration.cs
│   │   │   ├── ApplicationConfiguration.cs
│   │   │   ├── ApplicationDocumentConfiguration.cs
│   │   │   ├── ViewingConfiguration.cs
│   │   │   ├── LeaseConfiguration.cs
│   │   │   ├── PaymentConfiguration.cs
│   │   │   ├── MaintenanceRequestConfiguration.cs
│   │   │   ├── MaintenancePhotoConfiguration.cs
│   │   │   ├── ComplaintConfiguration.cs
│   │   │   ├── AnnouncementConfiguration.cs
│   │   │   ├── NotificationConfiguration.cs
│   │   │   ├── DocumentConfiguration.cs
│   │   │   └── InventoryItemConfiguration.cs
│   │   │
│   │   ├── Repositories/
│   │   │   ├── IRepository.cs
│   │   │   ├── Repository.cs
│   │   │   ├── IUserRepository.cs
│   │   │   ├── UserRepository.cs
│   │   │   ├── IPropertyRepository.cs
│   │   │   └── PropertyRepository.cs
│   │   │
│   │   ├── Migrations/
│   │   │   ├── 20260930143546_InitialCreate.cs
│   │   │   ├── 20261001083400_SecondMigration.cs
│   │   │   └── UniCribzDbContextModelSnapshot.cs
│   │   │
│   │   ├── Seed/
│   │   │   └── DataSeeder.cs                   # 3 users, 3 universities, 5 amenities, 1 property, 3 rooms
│   │   │
│   │   └── UniCribz.Data.csproj
│   │
│   ├── UniCribz.Shared/                        # Shared Models & Utilities
│   │   ├── Constants/
│   │   │   ├── Roles.cs
│   │   │   └── Policies.cs
│   │   ├── Helpers/
│   │   │   ├── PasswordHasher.cs               # BCrypt wrapper
│   │   │   └── JwtTokenGenerator.cs            # primitive-only signature (no Data ref)
│   │   └── UniCribz.Shared.csproj
│   │
│   └── UniCribz.Web/                           # ASP.NET Core MVC (Frontend)
│       ├── Controllers/
│       │   ├── HomeController.cs
│       │   ├── PropertyController.cs           # (placeholder)
│       │   ├── ApplicationController.cs        # (placeholder)
│       │   ├── ViewingController.cs            # (placeholder)
│       │   ├── AccountController.cs            # (placeholder)
│       │   ├── TenantController.cs             # (placeholder)
│       │   └── AdminController.cs              # (placeholder)
│       ├── Views/
│       │   ├── Shared/
│       │   │   └── _Layout.cshtml              # UniCribz theme applied
│       │   ├── Home/
│       │   │   ├── Index.cshtml
│       │   │   └── Privacy.cshtml
│       │   ├── Property/
│       │   │   ├── Search.cshtml               # (placeholder)
│       │   │   └── Details.cshtml              # (placeholder)
│       │   ├── Application/
│       │   │   └── Create.cshtml               # (placeholder)
│       │   ├── Viewing/
│       │   │   └── Book.cshtml                 # (placeholder)
│       │   ├── Account/
│       │   │   ├── Login.cshtml                # (placeholder)
│       │   │   └── Register.cshtml             # (placeholder)
│       │   ├── Tenant/
│       │   │   └── Dashboard.cshtml            # (placeholder)
│       │   └── Admin/
│       │       └── Dashboard.cshtml            # (placeholder)
│       ├── ViewModels/
│       │   ├── PropertySearchViewModel.cs
│       │   ├── PropertyDetailsViewModel.cs
│       │   ├── TenantDashboardViewModel.cs
│       │   ├── AdminDashboardViewModel.cs
│       │   └── LoginViewModel.cs
│       ├── Services/
│       │   ├── IApiClient.cs
│       │   └── ApiClient.cs
│       ├── wwwroot/
│       │   ├── css/
│       │   │   └── site.css                    # #2C3E50 / #18BC9C theme
│       │   ├── js/
│       │   │   └── site.js
│       │   ├── lib/                            # bootstrap, jquery (default template)
│       │   └── favicon.ico
│       ├── appsettings.json                    # Api:BaseUrl → http://localhost:5125
│       ├── Program.cs
│       └── UniCribz.Web.csproj
│
├── tests/
│   ├── UniCribz.Api.Tests/
│   │   ├── UnitTests/
│   │   │   ├── Services/
│   │   │   ├── Notifications/
│   │   │   └── Observers/
│   │   ├── IntegrationTests/
│   │   │   └── Controllers/
│   │   └── UniCribz.Api.Tests.csproj
│   ├── UniCribz.Web.Tests/
│   │   └── UniCribz.Web.Tests.csproj
│   └── UniCribz.E2E.Tests/
│       ├── Playwright/
│       └── UniCribz.E2E.Tests.csproj
│
├── .editorconfig
├── .gitattributes
├── .gitignore
├── Directory.Build.props
├── UniCribz.sln                                # 7 projects
├── global.json                                 # pins .NET 8
├── docker-compose.yml                          # PostgreSQL 15 + Redis 7
├── Dockerfile
├── README.md
└── LICENSE
```
---

## ✅ Current Project Status

### Done (Infrastructure & Scaffolding)

| Area | Status |
|------|--------|
| Solution + 7 projects | ✅ Created |
| All NuGet packages | ✅ Installed |
| Folder structure (46 folders) | ✅ Created |
| Placeholder classes (141 files) | ✅ Created |
| API `Program.cs` (JWT, EF, Redis, Swagger) | ✅ Configured |
| Web `Program.cs` (MVC, HttpClient) | ✅ Configured |
| `appsettings.json` (both projects) | ✅ Configured with URLs |
| `_Layout.cshtml` theme + site.css | ✅ Applied |
| `UniCribzDbContext` scaffold | ✅ Created |
| `docker-compose.yml` (Postgres + Redis) | ✅ Ready |
| GitHub Actions (CI, staging, prod) | ✅ Created (deploys guarded on Azure) |
| `setup/` automation scripts | ✅ Created |

**Build status:** `dotnet build` → ✅ 0 Errors, 0 Warnings
**Test status:** `dotnet test` → ✅ 3 tests pass

### Not Yet Done (Your Work)

| Area | Status |
|------|--------|
| Entity properties + relationships | ⏳ Siyabonga |
| EF Core migration #1 | ⏳ Siyabonga |
| Enums filled in | ⏳ Siyabonga |
| Auth endpoints (register + login) | ⏳ Siyanda |
| Property endpoints | ⏳ Siyanda |
| JWT generation & validation | ⏳ Siyanda |
| Notification Strategy Pattern | ⏳ Anothile |
| Payment integration (Stripe) | ⏳ Anothile |
| Google Maps geocoding | ⏳ Anothile |
| All Razor views | ⏳ Samkelsiwe |
| Frontend controllers | ⏳ Samkelsiwe |
| Azure infrastructure | ⏳ Siyanda (Week 3-4) |
| GitHub secrets (Azure) | ⏳ Siyanda (Week 3-4) |

---

## 🌿 Branching Strategy

```
main                    ← production-ready, tagged releases only
└── develop             ← integration branch, deploys to staging
    ├── feature/frontend-public-website-setup    (Samkelsiwe)
    ├── feature/database-schema-design           (Siyabonga)
    ├── feature/payment-notification-logic       (Anothile)
    └── feature/backend-api-development          (Siyanda)
```

**Branch protection (setup on GitHub):**
- `main`: require PR + 1 approval + CI pass
- `develop`: require PR + 1 approval + CI pass

**Commit convention:**
```
feat:       New feature
feat(web):  Feature scoped to a subproject
fix:        Bug fix
docs:       Documentation
refactor:   Code restructuring
test:       Adding tests
chore:      Maintenance
```

**Daily workflow:**
```bash
git checkout develop
git pull
git checkout feature/your-branch
git merge develop
# ... work ...
git add . && git commit -m "feat: ..."
git push
```

---

## 🧱 Architecture

Layered (N-Tier) with clear separation:

```
┌──────────────────────────────────────┐
│  PRESENTATION (UniCribz.Web)         │
│  ASP.NET Core MVC + Razor + Bootstrap│
└────────────────┬─────────────────────┘
                 │ HTTPS / REST
┌────────────────▼─────────────────────┐
│  APPLICATION (UniCribz.Api)          │
│  ASP.NET Core Web API + JWT + RBAC   │
└────────────────┬─────────────────────┘
                 │ EF Core / SDK
┌────────────────▼─────────────────────┐
│  DATA (UniCribz.Data)                │
│  PostgreSQL + Redis + Blob Storage   │
└──────────────────────────────────────┘
```

**Design patterns used:**
- Strategy (Notification channels)
- Observer (Room status on tenant move-out)
- Façade (Tenant dashboard aggregation)
- Repository (Data access)

---

## 🛠️ Technology Stack

| Layer | Tech |
|-------|------|
| Backend | .NET 8, ASP.NET Core, C# 12 |
| Frontend | ASP.NET Core MVC, Razor, Bootstrap 5.3 |
| Database | PostgreSQL 15 (Azure Flexible Server) |
| Cache | Redis 7 (Azure Cache) |
| Storage | Azure Blob Storage |
| Auth | JWT Bearer + RBAC |
| CI/CD | GitHub Actions |
| Cloud | Microsoft Azure |
| Testing | xUnit, Moq, FluentAssertions, Playwright |

---

## 📚 Full Documentation

- **Complete project README:** [`docs/README-full.md`](docs/README-full.md) — 24 sections covering architecture, security, deployment, costs, change management
- **Team instructions:** `docs/team/*.md`
- **Setup scripts:** `setup/README.md`
- **Prototype:** https://github.com/SiyandaNduze/UniCribz-Web-app.git

---

## ❓ Troubleshooting

| Issue | Fix |
|-------|-----|
| `dotnet: command not found` | Install .NET 8 SDK |
| Build fails after clone | Run `setup/00-run-all.ps1` |
| `dotnet --version` shows 10.x | Repo has `global.json` pinning 8.0 |
| Docker containers not starting | Open Docker Desktop first |
| HTTPS cert warnings | `dotnet dev-certs https --trust` |
| CI fails on `dotnet format` | It's `continue-on-error` — ignore |
| Azure deploy fails | Add the 4 publish profiles + credentials to GitHub secrets |

---

## 📝 Declaration

See `docs/declaration-of-authenticity.md` for the signed declarations from all 4 team members.

---

**Last Updated:** September 2026
**Maintained by:** UniCribz Development Team
```

---

## 📌 How to Use These Files

1. **Create the folders:**
   ```powershell
   New-Item -ItemType Directory -Path "docs/team" -Force
   New-Item -ItemType Directory -Path "docs/research" -Force
   New-Item -ItemType Directory -Path "docs/deployment" -Force
   New-Item -ItemType Directory -Path "docs/security" -Force
   ```

2. **Save each file** at:
   - `docs/team/1-samkelsiwe-frontend.md`
   - `docs/team/2-anothile-integration.md`
   - `docs/team/3-siyabonga-database.md`
   - `docs/team/4-siyanda-backend-devops.md`
   - `README.md` (replace root)

3. **Move the full README you already have** to `docs/README-full.md` so nothing is lost.

4. **Commit:**
   ```powershell
   git add docs/ README.md
   git commit -m "docs: add per-member onboarding guides and updated root README"
   git push
   ```

5. **Send each teammate the link to their specific file** on GitHub:
   - Samkelsiwe → `docs/team/1-samkelsiwe-frontend.md`
   - Anothile → `docs/team/2-anothile-integration.md`
   - Siyabonga → `docs/team/3-siyabonga-database.md`
   - You → keep `docs/team/4-siyanda-backend-devops.md` for reference