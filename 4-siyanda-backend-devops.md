## 📄 File 4: `4-siyanda-backend-devops.md`

```markdown
# Siyanda Nduze — Backend Architect & DevOps Lead
**Student Number:** ST10440706
**Branch:** `feature/backend-api-development`
**Working Directory:** `src/UniCribz.Api/`, `src/UniCribz.Shared/`, `.github/workflows/`

---

## ✅ What Has Already Been Done (By You)

Everything below is **already in the repository**:

### Repository & Solution
- ✅ GitHub repo created and configured
- ✅ `UniCribz.sln` with **7 projects** (API, Data, Shared, Web, 3 test projects)
- ✅ All project references wired up
- ✅ `global.json` pinning .NET 8
- ✅ `.gitignore`, `.gitattributes`, `.editorconfig`, `Directory.Build.props`
- ✅ `docker-compose.yml` (PostgreSQL + Redis)
- ✅ `main` and `develop` branches created

### Dependencies
- ✅ **26 NuGet packages** installed in `UniCribz.Api` (JWT, EF Core, Redis, Azure, Serilog, Stripe, SendGrid, Twilio, RestSharp, etc.)
- ✅ **5 packages** in `UniCribz.Data`
- ✅ **3 packages** in `UniCribz.Web`
- ✅ **Test frameworks** installed (xUnit, Moq, FluentAssertions, Testcontainers, Playwright)

### Folder Structure
- ✅ **46 folders** created across all projects
- ✅ **141 placeholder files** created with `// TODO` comments
- ✅ Every team member's area is scaffolded and ready

### API Configuration
- ✅ `Program.cs` fully configured: JWT, EF Core, Redis, CORS, Swagger, Serilog, Health Checks
- ✅ `appsettings.json` with all connection strings and API key placeholders
- ✅ `launchSettings.json` verified (HTTPS: 7285, HTTP: 5125)
- ✅ `ServiceCollectionExtensions.cs` placeholder ready to register services

### Data Layer Scaffolding
- ✅ `UniCribzDbContext.cs` with `ApplyConfigurationsFromAssembly`
- ✅ All entity/enum/configuration/repository placeholders created

### Frontend Scaffolding
- ✅ `_Layout.cshtml` with UniCribz theme applied
- ✅ `site.css` with color scheme
- ✅ Web `Program.cs` with HttpClient → API at `https://localhost:7285`

### CI/CD & Automation
- ✅ `.github/workflows/ci.yml` — runs on every push/PR
- ✅ `.github/workflows/deploy-staging.yml` — guarded on Azure secrets (silently skips if not configured)
- ✅ `.github/workflows/deploy-production.yml` — blue-green, guarded
- ✅ `setup/` folder with 5 automation scripts for team onboarding

### Verification
- ✅ `dotnet build` → **0 Warnings, 0 Errors**
- ✅ `dotnet test` → **3 tests pass** (trivial templates)

**You are the ONLY person who needs to implement anything in the API layer core.** Everything is scaffolded for you.

---

## 🎯 Where YOU Need to Start

### Step 0: Start a work session

```bash
git checkout develop
git pull
git checkout feature/backend-api-development
dotnet build
```

### Step 1: Shared constants (5 minutes)

**Files:**
- `src/UniCribz.Shared/Constants/Roles.cs`
```csharp
public static class Roles
{
    public const string Admin = "Admin";
    public const string Tenant = "Tenant";
    public const string Visitor = "Visitor";
}
```

- `src/UniCribz.Shared/Constants/Policies.cs`
```csharp
public static class Policies
{
    public const string AdminOnly = "AdminOnly";
    public const string TenantOnly = "TenantOnly";
    public const string TenantOrAdmin = "TenantOrAdmin";
}
```

### Step 2: Helpers (30 minutes)

**Files:**
- `src/UniCribz.Shared/Helpers/PasswordHasher.cs` — wrap `BCrypt.Net.BCrypt`
  ```csharp
  public static class PasswordHasher
  {
      public static string Hash(string password) => BCrypt.Net.BCrypt.HashPassword(password);
      public static bool Verify(string password, string hash) => BCrypt.Net.BCrypt.Verify(password, hash);
  }
  ```

- `src/UniCribz.Shared/Helpers/JwtTokenGenerator.cs` — generate JWT with claims:
  ```csharp
  public static string GenerateToken(User user, IConfiguration config)
  {
      var claims = new[]
      {
          new Claim(ClaimTypes.NameIdentifier, user.Id.ToString()),
          new Claim(ClaimTypes.Email, user.Email),
          new Claim(ClaimTypes.Role, user.Role.ToString())
      };
      // ... standard JWT generation
  }
  ```

### Step 3: Auth service + controller (2-3 hours)

**Files (in order):**
1. `src/UniCribz.Api/DTOs/Requests/RegisterRequest.cs`
2. `src/UniCribz.Api/DTOs/Requests/LoginRequest.cs`
3. `src/UniCribz.Api/DTOs/Responses/AuthResponse.cs`
4. `src/UniCribz.Api/Services/Interfaces/IUserService.cs`
5. `src/UniCribz.Api/Services/UserService.cs`
6. `src/UniCribz.Api/Controllers/AuthController.cs`

**Endpoints to implement:**
- `POST /api/auth/register` → hash password, save User, return JWT
- `POST /api/auth/login` → verify password, return JWT + user info
- `POST /api/auth/refresh` → issue new JWT from refresh token

### Step 4: Register services in DI

**File:** `src/UniCribz.Api/Extensions/ServiceCollectionExtensions.cs`

Uncomment and add:
```csharp
services.AddScoped<IUserService, UserService>();
services.AddScoped<IPropertyService, PropertyService>();
// ... as you implement each service
```

### Step 5: Property + Room endpoints (3-4 hours)

**Files:**
- `DTOs/Requests/CreatePropertyRequest.cs`
- `DTOs/Responses/PropertyResponse.cs`
- `Services/Interfaces/IPropertyService.cs`
- `Services/PropertyService.cs`
- `Controllers/PropertyController.cs`
- `Controllers/RoomController.cs`

**Endpoints:**
- `GET /api/properties` (public — search + filter)
- `GET /api/properties/{id}` (public)
- `POST /api/properties` (Admin only)
- `PUT /api/properties/{id}` (Admin only)
- `DELETE /api/properties/{id}` (Admin only)
- `GET /api/properties/{id}/rooms` (public)

### Step 6: Application + Viewing endpoints

Similar pattern. Reference the API endpoints table in the main README (page 21 of your full README).

### Step 7: Middleware (1 hour)

**Files:**
- `src/UniCribz.Api/Middleware/ErrorHandlingMiddleware.cs`
  - Catch all exceptions → return JSON `{ error: message, statusCode }`
  - Log via Serilog
- `src/UniCribz.Api/Middleware/RequestLoggingMiddleware.cs`

Register in `Program.cs`:
```csharp
app.UseMiddleware<ErrorHandlingMiddleware>();
app.UseMiddleware<RequestLoggingMiddleware>();
```

### Step 8: Validators (FluentValidation)

**Files in `src/UniCribz.Api/Validators/`:**
- `RegisterRequestValidator.cs` — email format, password length, phone format
- `LoginRequestValidator.cs`
- `CreateApplicationRequestValidator.cs`

Register in `Program.cs`:
```csharp
builder.Services.AddValidatorsFromAssemblyContaining<RegisterRequestValidator>();
```

### Step 9: AutoMapper profile

**File:** `src/UniCribz.Api/Mappings/MappingProfile.cs`

```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<User, AuthResponse>();
        CreateMap<Property, PropertyResponse>();
        // ...
    }
}
```

Register:
```csharp
builder.Services.AddAutoMapper(typeof(MappingProfile));
```

### Step 10: Register IRepository + DbContext

Already in `Program.cs`, but confirm the repository is registered:
```csharp
services.AddScoped(typeof(IRepository<>), typeof(Repository<>));
```
(Wait for Siyabonga to complete the repository pattern.)

### Step 11: Run and test

```bash
cd src/UniCribz.Api
dotnet run
```

Open `https://localhost:7285/swagger` → you should see your Auth endpoints.

Test with curl or Postman:
```bash
curl -X POST https://localhost:7285/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@test.com","password":"Test@123","fullName":"Test User"}'
```

### Step 12: Verify CI passes

Push your branch. Go to GitHub Actions. CI should run and pass (or fail on `dotnet format` — ignore that, it's `continue-on-error: true`).

---

## 🌐 Azure Setup (Do This ONLY When Ready — Week 3-4)

**Don't do this yet.** Wait until:
- ✅ Auth + Property + Application work locally
- ✅ Database migrations are complete
- ✅ Frontend has 2-3 working pages

### When Ready — Follow This Order

1. **Install Azure CLI:** https://learn.microsoft.com/cli/azure/install-azure-cli
2. **Login:** `az login`
3. **Create resource group:** `az group create --name unicribz-rg --location southafricanorth`
4. **Create App Services:** staging + production for API and Web (commands in the main README)
5. **Create PostgreSQL:** `az postgres flexible-server create ...`
6. **Create Redis:** `az redis create ...`
7. **Download publish profiles** for each App Service
8. **Add GitHub Secrets:** Settings → Secrets → Actions → New repository secret
   - `AZURE_STAGING_API_PUBLISH_PROFILE`
   - `AZURE_STAGING_WEB_PUBLISH_PROFILE`
   - `AZURE_PROD_API_PUBLISH_PROFILE`
   - `AZURE_PROD_WEB_PUBLISH_PROFILE`
   - `AZURE_CREDENTIALS`
9. **Create service principal:**
   ```bash
   az ad sp create-for-rbac --name "unicribz-github-actions" --role contributor \
     --scopes /subscriptions/YOUR-SUB-ID/resourceGroups/unicribz-rg --sdk-auth
   ```
   Copy output → `AZURE_CREDENTIALS` secret

Once secrets are set, the deploy workflows activate automatically.

---

## 🔗 What You Depend On

| From | What | When |
|------|------|------|
| **Siyabonga** | `User`, `Property`, `Room`, `Application` entities | Week 1 |
| **Siyabonga** | `UniCribzDbContext` with DbSets | Week 1 |
| **Siyabonga** | Repository pattern implementation | Week 2 |
| **Anothile** | `INotificationManager` for triggering notifications | Week 2 |
| **Samkelsiwe** | ViewModels/DTOs to define shape of API responses | Week 2 |

---

## 🔁 Daily Git Workflow

```bash
git checkout develop
git pull
git checkout feature/backend-api-development
git merge develop

git add .
git commit -m "feat(api): implement AuthController with JWT + BCrypt"
git push
```

---

## 📋 Code Review Reminders

When reviewing teammate PRs:
- ✅ All endpoints have `[Authorize]` or explicitly `[AllowAnonymous]`
- ✅ Input validation via FluentValidation
- ✅ Async/await used consistently
- ✅ No hardcoded secrets
- ✅ XML doc comments on public methods
- ✅ Unit tests added for business logic

---

## ❓ Quick Reference — Commands

```bash
# Start local env
docker-compose up -d

# Run API
cd src/UniCribz.Api && dotnet run

# Run Web (separate terminal)
cd src/UniCribz.Web && dotnet run

# Create migration
dotnet ef migrations add NAME --project src/UniCribz.Data --startup-project src/UniCribz.Api

# Apply migration
dotnet ef database update --project src/UniCribz.Data --startup-project src/UniCribz.Api

# Clean rebuild
dotnet clean && dotnet restore && dotnet build

# Trust HTTPS dev cert
dotnet dev-certs https --trust
```

---

## 📞 When to Escalate

- Siyabonga's entities aren't ready → use mock DTOs, mock the service layer temporarily
- CI failing on something you didn't touch → check the log, may be a teammate's PR
- Azure deployment fails → check the Actions log, usually a publish profile issue