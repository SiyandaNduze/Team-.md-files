## 📄 File 2: `docs/team/2-anothile-integration.md`

```markdown
# Anothile Bhengu — Integration & Services Specialist
**Student Number:** ST10440981
**Branch:** `feature/payment-notification-logic`
**Working Directory:** `src/UniCribz.Api/Notifications/`, `src/UniCribz.Api/Services/`

---

## What Has Already Been Done

### By Siyanda (Scaffolding)
| Item | Location |
|------|----------|
| All packages installed | SendGrid, Twilio, Stripe.net, RestSharp |
| Notification folder structure | `src/UniCribz.Api/Notifications/` |
| Placeholder files (6) | INotificationStrategy + 3 strategies + manager |
| Placeholder services | PaymentService, GoogleMapsService, NotificationService |
| Placeholder interfaces | IPaymentService, IGoogleMapsService, INotificationService |
| `appsettings.json` with all API key sections | SendGrid, Twilio, Stripe, GoogleMaps |
| DI registration slot in `ServiceCollectionExtensions.cs` | Ready for your code |

### By Siyanda (Backend — Production-Ready)
| Item | Status |
|------|--------|
| `Payment` entity with all fields (StripePaymentIntentId, ProofOfPaymentUrl, etc.) | ✅ |
| `Notification` entity with Channel, Severity, RelatedEntityType | ✅ |
| `PaymentController` with initiate, upload proof, verify, receipt endpoints | ✅ |
| `NotificationController` with list, unread count, mark read | ✅ |
| `PaymentService` — local flow works; you plug in Stripe | ✅ |
| `NotificationService` — basic query; you plug in Strategy + sends | ✅ |
| GoogleMaps endpoints `GET /api/maps/geocode` and `/nearby` | ✅ (registered, service pending) |
| Health checks split (live/ready) | ✅ |
| Rate limiting (100 req/min/IP via framework) | ✅ |
| Correlation IDs on all responses | ✅ |
| Security headers + HSTS | ✅ |
| Validated Options pattern for SendGrid/Twilio/Stripe keys | ✅ |

**Important:** The `GoogleMapsApi` (v6) package doesn't exist. Use **`RestSharp` v112** (already installed).

---

## Where YOU Need to Start

### Step 0: Get the project running

```bash
git checkout develop && git pull
git checkout -b feature/payment-notification-logic
docker-compose up -d

cd src/UniCribz.Api
dotnet run
# → http://localhost:5125/swagger
```

### Step 1: Strategy Pattern — Notifications (START HERE)

**Project doc requirement — page 47.**

**Files to implement in order:**

1. **`INotificationStrategy.cs`**
```csharp
public interface INotificationStrategy
{
    Task SendAsync(string recipient, string message);
}
```

2. **`EmailNotificationStrategy.cs`** — SendGrid
   - Inject `IConfiguration`, `ILogger<EmailNotificationStrategy>`
   - Read `SendGrid:ApiKey` and `SendGrid:FromEmail`
   - `SendAsync` → `SendGridClient.SendEmailAsync(msg)`

3. **`SMSNotificationStrategy.cs`** — Twilio
   - Read `Twilio:AccountSid`, `AuthToken`, `FromNumber`
   - `MessageResource.CreateAsync(to, from, body)`

4. **`InAppNotificationStrategy.cs`** — Save to DB
   - Inject `UniCribzDbContext`
   - Create `Notification` entity, `SaveChangesAsync`

5. **`NotificationManager.cs`**
```csharp
public class NotificationManager : INotificationManager
{
    private readonly IEnumerable<INotificationStrategy> _strategies;
    public NotificationManager(IEnumerable<INotificationStrategy> strategies) => _strategies = strategies;

    public async Task NotifyAsync(string recipient, string message)
    {
        foreach (var strategy in _strategies)
            await strategy.SendAsync(recipient, message);
    }
}
```

6. **Register in `ServiceCollectionExtensions.cs`**
```csharp
services.AddScoped<INotificationStrategy, EmailNotificationStrategy>();
services.AddScoped<INotificationStrategy, SMSNotificationStrategy>();
services.AddScoped<INotificationStrategy, InAppNotificationStrategy>();
services.AddScoped<INotificationManager, NotificationManager>();
```

### Step 2: Payment Service — Stripe integration

**Files:**
- `Services/Interfaces/IPaymentService.cs` (already declares the methods)
- `Services/PaymentService.cs` — you extend the existing local flow
- `Controllers/PaymentController.cs` — endpoint exists, you plug the service

**What to add:**
1. Configure Stripe in `Program.cs`: `StripeConfiguration.ApiKey = config["Stripe:SecretKey"];`
2. `PaymentService.InitiateAsync`: after creating the local `Payment`, also call `Stripe.Checkout.Session.CreateAsync(...)` and return the redirect URL
3. Add webhook handler: `POST /api/payments/webhook`
   - Verify signature via `Stripe:WebhookSecret`
   - Update `Payment.Status` → `Paid` or `Failed`
   - Trigger `INotificationManager`

**Testable locally:** Swagger at `http://localhost:5125/swagger` → `POST /api/payments`

### Step 3: Google Maps Service — RestSharp

**Files:**
- `Services/Interfaces/IGoogleMapsService.cs` (already declares geocode + nearby)
- `Services/GoogleMapsService.cs` — implement with RestSharp
- `Controllers/GoogleMapsController.cs` — endpoints exist

**Implementation:**
```csharp
public async Task<GeocodeResponse?> GeocodeAsync(string address)
{
    var client = new RestClient("https://maps.googleapis.com/maps/api/geocode/json");
    var request = new RestRequest();
    request.AddQueryParameter("address", address);
    request.AddQueryParameter("key", _config["GoogleMaps:ApiKey"]);

    var response = await client.GetAsync<GeocodeResponse>(request);
    // parse lat/lng, return
}
```

**Register:** `services.AddScoped<IGoogleMapsService, GoogleMapsService>();`

**Test endpoint:** `GET /api/maps/geocode?address=UJ%20Auckland%20Park`

### Step 4: Notification Service — Wrapper

**File:** `Services/NotificationService.cs`

Wrap `INotificationManager` with business logic:
- Which user gets what notification (e.g., on lease sign, on payment verified)
- Call from other services when events happen

### Step 5: Research Documentation

**File to create:** `docs/research/accommodation-market-research.md`

- Study the South African student housing market
- Analyze 3-5 competitors (DigsConnect, StudentDigz, etc.)
- Cite sources (IJR, Sabinet, IOL Property)

---

## What You Depend On

| From | What | Status |
|------|------|--------|
| **Siyabonga** | Payment + Notification entities | ✅ Done |
| **Siyanda** | `Program.cs` with Stripe/SendGrid/Twilio config | ✅ Done |
| **Siyanda** | Payment controller + service skeleton | ✅ Done |
| **Siyanda** | DI registration slot ready | ✅ Done |
| **Samkelsiwe** | UI triggers (Pay Now, notification bell) | ⏳ Pending |

**You're unblocked — start with Step 1.**

---

## Important Notes

- **Error format:** If a notification send fails, the whole `NotifyAsync` call will throw. Wrap each strategy call in a try/catch so one failure (e.g., SendGrid down) doesn't block SMS + InApp.
- **Correlation IDs:** Every API response has an `X-Correlation-Id`. Log it with every send so support can trace notification failures back to a specific request.
- **Rate limiting:** The framework limits each IP to 100 req/min. If you test SendGrid in a loop, you'll get 429s. Space your tests out.
- **Health checks:** `GET /health/ready` verifies Redis + Postgres. If your notification service depends on Redis, this is your smoke test.

---

## API Keys — Where They Go

| Key | Location | Source |
|-----|----------|--------|
| SendGrid | `SendGrid:ApiKey` | https://signup.sendgrid.com (free) |
| Twilio | `Twilio:AccountSid`, `AuthToken`, `FromNumber` | https://twilio.com/try-twilio |
| Stripe | `Stripe:SecretKey`, `PublishableKey`, `WebhookSecret` | https://dashboard.stripe.com/test/apikeys |
| Google Maps | `GoogleMaps:ApiKey` | https://console.cloud.google.com/apis/credentials |

**Never commit real keys.** Use `dotnet user-secrets`:
```bash
cd src/UniCribz.Api
dotnet user-secrets init
dotnet user-secrets set "SendGrid:ApiKey" "SG.xxxxx"
dotnet user-secrets set "Twilio:AccountSid" "ACxxxxx"
dotnet user-secrets set "Stripe:SecretKey" "sk_test_xxxxx"
dotnet user-secrets set "GoogleMaps:ApiKey" "AIzaxxxxx"
```

In Azure production, keys are set as **App Service environment variables** (never in files).

---

## Daily Git Workflow

```bash
git checkout develop && git pull
git checkout feature/payment-notification-logic
git merge develop
git add . && git commit -m "feat(api): implement EmailNotificationStrategy"
git push
```

---

## If Something Breaks

- RestSharp not restoring? `dotnet nuget locals all --clear && dotnet restore`
- SendGrid/Twilio key invalid? Test at their dashboard
- Stripe webhook fails? `Stripe:WebhookSecret` must match dashboard
- Getting `429 Too Many Requests`? Rate limiter — wait 60s