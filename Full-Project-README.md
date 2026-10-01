## 📄 File 5: `README.md` (Root)

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

# 3. Run setup (first time only)
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
| API (Swagger) | `http://localhost:5125/swagger` |
| API (Health) | `http://localhost:5125/health` |
| Web Frontend | `https://localhost:7105` |
| PostgreSQL | `localhost:5432` |
| Redis | `localhost:6379` |

**Test accounts (seeded):**
| Role | Email | Password |
|------|-------|----------|
| Admin | admin@unicribz.co.za | Admin@123 |
| Tenant | tenant@unicribz.co.za | Admin@123 |
| Visitor | visitor@unicribz.co.za | Admin@123 |

---

## 👥 Team & Roles

| Name | Role | Branch | Directory |
|------|------|--------|-----------|
| **Siyanda Nduze** (ST10440706) | Backend Architect & DevOps | `feature/backend-*` | `src/UniCribz.Api/`, `src/UniCribz.Shared/`, `.github/` |
| **Samkelsiwe Hlatshwayo** (ST10442364) | UI/UX & Frontend | `feature/frontend-*` | `src/UniCribz.Web/` |
| **Siyabonga Cebekhulu** (ST10440807) | Database & Backend | `feature/database-*` | `src/UniCribz.Data/` |
| **Anothile Bhengu** (ST10440981) | Integrations & Services | `feature/payment-*`, `feature/google-maps-*` | `src/UniCribz.Api/Notifications/`, `Services/` |

**Per-member instructions:** `docs/team/1-samkelsiwe-frontend.md`, `2-anothile-integration.md`, `3-siyabonga-database.md`, `4-siyanda-backend-devops.md`.

---

## 📁 Full File Structure

```
UniCribz-Student-Accommodation-System/
│
├── .github/workflows/
│   ├── ci.yml                              # Lint, build, test
│   ├── deploy-staging.yml                  # Azure staging (guarded)
│   └── deploy-production.yml               # Blue-green (guarded)
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
│   ├── UniCribz.Api/                           # ASP.NET Core Web API
│   │   ├── Controllers/
│   │   │   ├── AuthController.cs               # register, login, me, refresh, logout
│   │   │   ├── PropertyController.cs           # search, CRUD, rooms
│   │   │   ├── ApplicationController.cs        # submit, approve, upload doc
│   │   │   ├── ViewingController.cs            # book, approve, available slots
│   │   │   ├── LeaseController.cs              # create, sign, terminate
│   │   │   ├── PaymentController.cs            # initiate, upload, verify, receipt
│   │   │   ├── MaintenanceController.cs        # submit, assign, status
│   │   │   ├── ComplaintController.cs          # submit, resolve
│   │   │   ├── AnnouncementController.cs       # create, list, delete
│   │   │   ├── NotificationController.cs       # list, unread, mark read
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
│   │   ├── Observers/                          # ✅ Observer pattern live
│   │   │   ├── IStatusObserver.cs              # Event + interface
│   │   │   ├── RoomStatusUpdater.cs            # Frees room on MOVE_OUT
│   │   │   ├── ReportServiceObserver.cs        # Invalidates reports
│   │   │   └── AdminDashboardObserver.cs       # Invalidates dashboard
│   │   │
│   │   ├── Facades/
│   │   │   └── TenantDashboardFacade.cs        # Placeholder
│   │   │
│   │   ├── DTOs/
│   │   │   ├── Requests/                       # 17 request DTOs
│   │   │   └── Responses/                      # 18 response DTOs
│   │   │
│   │   ├── Middleware/
│   │   │   ├── ErrorHandlingMiddleware.cs
│   │   │   ├── RequestLoggingMiddleware.cs
│   │   │   └── RateLimitingMiddleware.cs
│   │   │
│   │   ├── Validators/
│   │   │   ├── LoginRequestValidator.cs
│   │   │   ├── RegisterRequestValidator.cs
│   │   │   └── CreateApplicationRequestValidator.cs
│   │   │
│   │   ├── Extensions/
│   │   │   ├── ServiceCollectionExtensions.cs
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
│   │   ├── UniCribz.Api.http                   # ~60 tests
│   │   ├── Program.cs
│   │   └── UniCribz.Api.csproj
│   │
│   ├── UniCribz.Data/                          # EF Core Data Layer
│   │   ├── Entities/                           # 21 entities
│   │   ├── Enums/                              # 8 enums
│   │   ├── Context/
│   │   │   └── UniCribzDbContext.cs            # 22 DbSets, TPH
│   │   ├── Configurations/                     # 18 configs
│   │   ├── Repositories/                       # Repository pattern
│   │   ├── Migrations/
│   │   │   ├── 20260930143546_InitialCreate.cs
│   │   │   ├── 20261001083400_SecondMigration.cs
│   │   │   └── UniCribzDbContextModelSnapshot.cs
│   │   ├── Seed/
│   │   │   └── DataSeeder.cs                   # 3 users, 3 unis, 5 amenities, property, 3 rooms
│   │   └── UniCribz.Data.csproj
│   │
│   ├── UniCribz.Shared/
│   │   ├── Constants/
│   │   │   ├── Roles.cs
│   │   │   └── Policies.cs
│   │   ├── Helpers/
│   │   │   ├── PasswordHasher.cs
│   │   │   └── JwtTokenGenerator.cs
│   │   └── UniCribz.Shared.csproj
│   │
│   └── UniCribz.Web/                           # ASP.NET Core MVC (Frontend)
│       ├── Controllers/                        # Mostly placeholders (Sam to build)
│       ├── Views/
│       │   ├── Shared/_Layout.cshtml           # UniCribz theme applied
│       │   ├── Home/                           # Index.cshtml, Privacy.cshtml
│       │   ├── Property/                       # Placeholders
│       │   ├── Application/                    # Placeholder
│       │   ├── Viewing/                        # Placeholder
│       │   ├── Account/                        # Placeholders
│       │   ├── Tenant/                         # Placeholder
│       │   └── Admin/                          # Placeholder
│       ├── ViewModels/                         # 5 placeholders
│       ├── Services/                           # IApiClient, ApiClient placeholders
│       ├── wwwroot/
│       │   ├── css/site.css                    # #2C3E50 / #18BC9C
│       │   ├── js/site.js
│       │   └── lib/                            # bootstrap, jquery
│       ├── appsettings.json                    # Api:BaseUrl → http://localhost:5125
│       ├── Program.cs
│       └── UniCribz.Web.csproj
│
├── tests/
│   ├── UniCribz.Api.Tests/
│   ├── UniCribz.Web.Tests/
│   └── UniCribz.E2E.Tests/
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

### Backend — 100% Complete and Tested

| Area | Status |
|------|--------|
| Solution + 7 projects | ✅ |
| All NuGet packages | ✅ |
| Full folder structure | ✅ |
| API `Program.cs` (JWT, EF, Redis, Swagger, AutoMapper, FluentValidation) | ✅ |
| Data layer (21 entities, 8 enums, 18 EF configs) | ✅ |
| 2 EF migrations applied | ✅ |
| 20 tables in PostgreSQL | ✅ |
| Repository pattern | ✅ |
| Data seeder | ✅ |
| Auth: register, login, me, refresh, logout | ✅ |
| Property + Room: search, CRUD, add room | ✅ |
| Application: submit, approve, upload doc | ✅ |
| Viewing: book, approve, available slots | ✅ |
| Lease: create, sign, terminate | ✅ |
| Payment: initiate, upload, verify, receipt | ✅ |
| Maintenance: submit, assign, complete | ✅ |
| Complaint: submit, resolve | ✅ |
| Announcement: create, list, delete | ✅ |
| Notification: list, unread, mark read | ✅ |
| Report: occupancy, payments, maintenance, dashboard | ✅ |
| **Observer pattern** (fires on lease termination) | ✅ |
| Middleware (error, logging, rate limiting) | ✅ |
| ~60 integration tests in `.http` file | ✅ |

**Build status:** `dotnet build` → **0 Errors, 0 Warnings**

### Remaining Work

| Area | Owner | Status |
|------|-------|--------|
| Frontend UI (all views + controllers) | Samkelsiwe | ⏳ |
| Strategy Pattern (SendGrid/Twilio/InApp) | Anothile | ⏳ |
| Google Maps service implementation | Anothile | ⏳ |
| Stripe payment integration | Anothile | ⏳ |
| `docs/data-migration-plan.md` | Siyabonga | ⏳ |
| Documentation files (`docs/team/*`, `docs/README-full.md`) | Siyanda | ⏳ |
| Azure deployment (Week 3-4) | Siyanda | ⏳ |

---

## 🌿 Branching Strategy

```
main                    ← production-ready, tagged releases only
└── develop             ← integration branch
    ├── feature/frontend-public-website-setup    (Samkelsiwe)
    ├── feature/database-schema-design           (Siyabonga)
    ├── feature/payment-notification-logic       (Anothile)
    └── feature/backend-api-development          (Siyanda)
```

**Commit convention:** `feat:` | `fix:` | `docs:` | `refactor:` | `test:` | `chore:`

**Daily workflow:**
```bash
git checkout develop && git pull
git checkout feature/your-branch
git merge develop
# work...
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
- **Strategy** — Notification channels (Email, SMS, In-App) — `Notifications/*`
- **Observer** — Auto-update on tenant move-out — `Observers/*` ✅ live
- **Façade** — Tenant dashboard aggregation — `Facades/*`
- **Repository** — Data access — `Data/Repositories/*`

---

## 🛠️ Technology Stack

| Layer | Tech |
|-------|------|
| Backend | .NET 8, ASP.NET Core, C# 12 |
| Frontend | ASP.NET Core MVC, Razor, Bootstrap 5.3 |
| Database | PostgreSQL 15 |
| Cache | Redis 7 |
| Storage | Azure Blob Storage (staged) |
| Auth | JWT Bearer + RBAC |
| CI/CD | GitHub Actions |
| Cloud | Microsoft Azure (deferred) |
| Testing | xUnit, Moq, FluentAssertions, Playwright |

---

## 📚 Full Documentation

- **Full project README:** [`docs/README-full.md`](docs/README-full.md)
- **Team instructions:** `docs/team/*.md`
- **Setup scripts:** `setup/README.md`
- **Prototype:** https://github.com/SiyandaNduze/UniCribz-Web-app.git

---

## ❓ Troubleshooting

| Issue | Fix |
|-------|-----|
| `dotnet: command not found` | Install .NET 8 SDK |
| Build fails after clone | Run `setup/00-run-all.ps1` |
| `dotnet --version` shows 10.x | Repo `global.json` pins 8.0 |
| Docker containers not starting | Open Docker Desktop first |
| API can't connect to DB | `docker-compose up -d` and wait 12s |
| HTTPS cert warnings | `dotnet dev-certs https --trust` |
| Frontend can't reach API | Verify `Api:BaseUrl` in `src/UniCribz.Web/appsettings.json` |
| Azure deploy fails | Add the 5 secrets to GitHub |

---

## 📝 Declaration

See `docs/declaration-of-authenticity.md` for the signed declarations from all 4 team members.

---

**Last Updated:** 1 October 2026
**Maintained by:** UniCribz Development Team