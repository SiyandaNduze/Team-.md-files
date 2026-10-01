## 📄 File 4: `docs/team/4-siyanda-backend-devops.md`

```markdown
# Siyanda Nduze — Backend Architect & DevOps Lead
**Student Number:** ST10440706
**Branch:** `feature/backend-api-development`
**Working Directory:** `src/UniCribz.Api/`, `src/UniCribz.Shared/`, `.github/workflows/`

---

## COMPLETE — Every Module Delivered + Production-Hardened

### Repository & Solution
- ✅ GitHub repo with `main` + `develop` branches
- ✅ `UniCribz.sln` with 7 projects
- ✅ All references + NuGet packages (including `Serilog.Sinks.File`, `Serilog.Enrichers.Environment`)
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
| **Observer** | `Observers/*` | ✅ **Fully working** — fires 3 observers on lease termination |
| **Façade** | `Facades/TenantDashboardFacade.cs` | Scaffolded |
| **Repository** | `Data/Repositories/*` | ✅ Complete |

### Middleware — Production Hardened
| Middleware | Purpose | Status |
|------------|---------|--------|
| `SecurityHeadersMiddleware` | CSP, X-Frame-Options, HSTS, Referrer-Policy | ✅ Active |
| `CorrelationIdMiddleware` | X-Correlation-Id on every request, added to log scope | ✅ Active |
| `ErrorHandlingMiddleware` | Global exception handler with correlation ID, hides details in prod | ✅ Active |
| `RequestLoggingMiddleware` | Serilog timing per request | ✅ Active |
| `_LegacyRateLimitingMiddleware` | Custom rate limiter — superseded, kept for reference | 📄 Retained |
| Framework `UseRateLimiter()` | Built-in, 100 req/min per IP, returns 429 with `Retry-After` | ✅ Active |

### Options Pattern (Validated on Startup)
| Options Class | Purpose | Validation |
|---------------|---------|-----------|
| `JwtOptions` | JWT key, issuer, audience, expiry | Key ≥ 32 chars, expiry > 0 |
| `AppOptions` | Base URLs + CORS origins | WebBaseUrl required |
| `DatabaseOptions` | MigrateOnStartup, SeedOnStartup | — |

App will **refuse to start** if any option fails validation.

### Configuration — Environment-Specific
| File | Purpose |
|------|---------|
| `appsettings.json` | Dev defaults |
| `appsettings.Development.json` | Local overrides |
| `appsettings.Staging.json` | `MigrateOnStartup: true`, seed off |
| `appsettings.Production.json` | Empty secrets (populated by Azure env vars) |

Secrets policy: **templates committed with empty strings; real values come from Azure App Service environment variables.**

### Health Checks
| Endpoint | Purpose | Checks |
|----------|---------|--------|
| `GET /health` | Full report | All checks |
| `GET /health/live` | Liveness probe | Self |
| `GET /health/ready` | Readiness probe | PostgreSQL + Redis |

### Security Hardening
- ✅ HTTPS enforced (non-development)
- ✅ HSTS enabled (non-development)
- ✅ Security headers (CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy)
- ✅ Rate limiting (100 req/min/IP)
- ✅ Response compression (Brotli + gzip)
- ✅ Forwarded headers (for Azure reverse proxy)
- ✅ JWT validation with 1-minute clock skew
- ✅ No server-identifying headers (`Server`, `X-Powered-By` removed)

### Resilience
- ✅ EF Core retry on transient failures (3 retries, 5s delay, 30s command timeout)
- ✅ Query splitting for multi-collection includes
- ✅ Serilog rolling file logs (30-day retention)

### Verified Working
- ✅ `dotnet build` → **0 Warnings, 0 Errors**
- ✅ Full `.http` test suite passes (~60 requests)
- ✅ Observer pattern fires 3 observers on lease terminate
- ✅ All error paths return correct status codes (400/401/403/404/429)
- ✅ Correlation IDs propagate through every request
- ✅ Security headers present on every response
- ✅ Health checks return correct status
- ✅ Swagger renders properly in Development

---

## What's Left

### Documentation (Graded Deliverable)

**Create these files:**

| File | Content | Owner |
|------|---------|-------|
| `docs/team/1-samkelsiwe-frontend.md` | This file, saved | Siyanda |
| `docs/team/2-anothile-integration.md` | Same | Siyanda |
| `docs/team/3-siyabonga-database.md` | Same | Siyanda |
| `docs/team/4-siyanda-backend-devops.md` | Same | Siyanda |
| `docs/README-full.md` | Full project README (24 sections) | Siyanda |
| `docs/declaration-of-authenticity.md` | Signed declarations from all 4 members | Siyanda |
| `docs/data-migration-plan.md` | Migration strategy | Siyabonga |
| `docs/research/accommodation-market-research.md` | Market research | Anothile |
| `docs/deployment/deployment-plan.md` | Azure phases | Siyanda |
| `docs/deployment/rollback-procedure.md` | Rollback steps | Siyanda |
| `docs/security/security-architecture.md` | Security layers | Siyanda |
| `docs/security/risk-register.md` | Risk matrix | Siyanda |

**Commit after creating:**
```bash
git add docs/
git commit -m "docs: add team onboarding guides + project documentation"
git push
```

### Git Push
```bash
git add .
git commit -m "feat(api): production-hardening pass — security headers, correlation IDs, options validation, built-in rate limiter"
git push origin feature/backend-api-development
```

Then verify CI on GitHub Actions is green.

### Azure Deployment (Week 3-4)

Do this **after** Samkelsiwe has 2-3 frontend pages working:

1. `az login`
2. `az group create --name unicribz-rg --location southafricanorth`
3. Create App Services (staging + prod) for API + Web
4. Create PostgreSQL Flexible Server
5. Create Redis Cache
6. Create Blob Storage + App Gateway
7. Download 4 publish profiles
8. Add 5 GitHub Secrets
9. Create service principal for blue-green
10. Configure environment variables in App Service:
    - `ConnectionStrings__DefaultConnection`
    - `ConnectionStrings__Redis`
    - `Jwt__Key` (≥ 32 chars)
    - `App__WebBaseUrl`
    - `App__AllowedOrigins__0`
    - `Database__MigrateOnStartup=true`
    - `Database__SeedOnStartup=false`
    - `ASPNETCORE_ENVIRONMENT=Production`
    - SendGrid / Twilio / Stripe / GoogleMaps keys
11. Push code to trigger staging deploy

---

## You Depend On

| From | What | Status |
|------|------|--------|
| **Siyabonga** | All entities + migrations | ✅ Done |
| **Anothile** | Notification strategies, Stripe, Google Maps service | ⏳ Pending |
| **Samkelsiwe** | Frontend UI | ⏳ Pending |

---

## Git Workflow

```bash
git checkout develop && git pull
git checkout feature/backend-api-development
git merge develop
git add . && git commit -m "..."
git push
```

---

## Code Review Reminders

- ✅ `[Authorize]` / `[AllowAnonymous]` on every endpoint
- ✅ FluentValidation on all request DTOs
- ✅ Async/await consistently
- ✅ No hardcoded secrets (use env vars in prod)
- ✅ XML doc comments on public methods
- ✅ Unit tests for business logic
- ✅ Correlation IDs propagated

---

## Quick Reference — Commands

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

# Health check
curl http://localhost:5125/health/ready

# Test suite
# Open src/UniCribz.Api/UniCribz.Api.http in Visual Studio → run all
```

---

## When to Escalate

- Anothile's SendGrid/Twilio strategy not registered → check DI in `ServiceCollectionExtensions.cs`
- Frontend CORS errors → verify `App:AllowedOrigins` in API appsettings
- 429 errors during development → rate limiter active, wait 60s
- Azure deploy fails → check the Actions log for publish profile issues
- Health check `/ready` fails → check Docker containers are running (`docker ps`)