## 📄 File 4: `docs/team/4-siyanda-backend-devops.md`

```markdown
# Siyanda Nduze — Backend Architect & DevOps Lead
**Student Number:** ST10440706
**Branch:** `feature/backend-api-development`
**Working Directory:** `src/UniCribz.Api/`, `src/UniCribz.Shared/`, `.github/workflows/`

---

## ✅ COMPLETE — Every Module Delivered

### Repository & Solution
- ✅ GitHub repo with `main` + `develop` branches
- ✅ `UniCribz.sln` with **7 projects**
- ✅ All references + NuGet packages
- ✅ Root configs (`.gitignore`, `.editorconfig`, `Directory.Build.props`, `global.json`, `docker-compose.yml`)

### API Layer — 100% Complete
| Module | Endpoints | Status |
|--------|-----------|--------|
| **Auth** | register, login, me, refresh, logout | ✅ |
| **Property** | search, filter, CRUD, rooms, add room | ✅ |
| **Application** | submit, list, get, approve, upload doc | ✅ |
| **Viewing** | book, list, approve, available slots | ✅ |
| **Lease** | create, sign, terminate, list, mine | ✅ |
| **Payment** | initiate, upload proof, verify, receipt, list | ✅ |
| **Maintenance** | submit, list, mine, assign, status | ✅ |
| **Complaint** | submit, list, mine, resolve | ✅ |
| **Announcement** | public list, admin list, create, delete | ✅ |
| **Notification** | list, unread count, mark read | ✅ |
| **Report** | occupancy, payments, maintenance, dashboard | ✅ |
| **GoogleMaps** | geocode, nearby (endpoints live; service stubbed for Anothile) | ✅ |

### Design Patterns Implemented
| Pattern | File Location | Status |
|---------|---------------|--------|
| **Strategy** | `Notifications/*` | Scaffolded (Anothile to complete) |
| **Observer** | `Observers/*` | ✅ **Fully working** — fires on lease termination |
| **Façade** | `Facades/TenantDashboardFacade.cs` | Scaffolded |
| **Repository** | `Data/Repositories/*` | ✅ Complete |

### Middleware — 100% Complete
- ✅ `ErrorHandlingMiddleware` — global exception handler returns JSON
- ✅ `RequestLoggingMiddleware` — Serilog timing per request
- ✅ `RateLimitingMiddleware` — 60 req/min per IP, returns 429

### Data Layer Integration
- ✅ DbContext wired into DI
- ✅ Seed data runs on startup (idempotent)
- ✅ 22 DbSets accessible

### Shared Utilities
- ✅ `PasswordHasher` (BCrypt wrapper)
- ✅ `JwtTokenGenerator` (primitive-only signature, no Data ref)
- ✅ `Roles.cs`, `Policies.cs`

### Verified Working
- ✅ `dotnet build` → **0 Warnings, 0 Errors**
- ✅ Full `.http` test suite passes (~60 requests)
- ✅ Observer pattern fires 3 observers on lease terminate
- ✅ All error paths return correct status codes (400/401/403/404)
- ✅ DateTime Kind fix in ViewingService
- ✅ Rate limiting validated

---

## ⏳ What's Left

### Documentation (Graded Deliverable)

**Create these files:**

| File | Content |
|------|---------|
| `docs/team/1-samkelsiwe-frontend.md` | Sam's role doc (already drafted — save it) |
| `docs/team/2-anothile-integration.md` | Anothile's role doc |
| `docs/team/3-siyabonga-database.md` | Siyabonga's role doc |
| `docs/team/4-siyanda-backend-devops.md` | This file |
| `docs/README-full.md` | Full project README (the big one) |
| `docs/declaration-of-authenticity.md` | Signed declarations from all 4 members |
| `docs/data-migration-plan.md` | Assign to Siyabonga (his Step 8) |
| `docs/deployment/deployment-plan.md` | Azure deployment phases |
| `docs/deployment/rollback-procedure.md` | Rollback steps |
| `docs/security/security-architecture.md` | Security layers |
| `docs/security/risk-register.md` | Risk matrix |

**Commit after creating:**
```bash
git add docs/
git commit -m "docs: add team onboarding guides + project documentation"
git push
```

### Git Push
```bash
git add .
git commit -m "feat(api): complete backend — all modules, observer pattern, middleware"
git push origin feature/backend-api-development
```

Then verify CI on GitHub Actions is green.

### Azure Deployment (Week 3-4)
Do this **after** Samkelsiwe has 2-3 frontend pages working:
1. `az login`
2. `az group create --name unicribz-rg --location southafricanort`
3. Create App Services (staging + prod) for API + Web
4. Create PostgreSQL Flexible Server
5. Create Redis Cache
6. Create Blob Storage + App Gateway
7. Download 4 publish profiles
8. Add 5 GitHub Secrets
9. Create service principal for blue-green
10. Push code to trigger staging deploy

---

## 🔗 You Depend On

| From | What | Status |
|------|------|--------|
| **Siyabonga** | All entities + migrations | ✅ Done |
| **Anothile** | Notification strategies, Stripe, Google Maps service | ⏳ Pending |
| **Samkelsiwe** | Frontend UI | ⏳ Pending |

---

## 🔁 Git Workflow

```bash
git checkout develop && git pull
git checkout feature/backend-api-development
git merge develop
git add . && git commit -m "..."
git push
```

---

## 📋 Code Review Reminders

- ✅ `[Authorize]` / `[AllowAnonymous]` on every endpoint
- ✅ FluentValidation on all request DTOs
- ✅ Async/await consistently
- ✅ No hardcoded secrets
- ✅ XML doc comments on public methods
- ✅ Unit tests for business logic

---

## ❓ Quick Reference — Commands

```bash
# Start local env
docker-compose up -d

# Run API
cd src/UniCribz.Api && dotnet run

# Run Web
cd src/UniCribz.Web && dotnet run

# Create migration
dotnet ef migrations add NAME --project src/UniCribz.Data --startup-project src/UniCribz.Api

# Apply migration
dotnet ef database update --project src/UniCribz.Data --startup-project src/UniCribz.Api

# Clean rebuild
dotnet clean && dotnet restore && dotnet build

# Test suite
# Open src/UniCribz.Api/UniCribz.Api.http in Visual Studio → run all
```

---

## 📞 When to Escalate

- Anothile's SendGrid/Twilio strategy not registered → check DI
- Frontend CORS errors → verify `App:WebBaseUrl` in API appsettings
- Azure deploy fails → check the Actions log for publish profile issues