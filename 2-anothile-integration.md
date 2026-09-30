## 📄 File 2: `2-anothile-integration.md`

```markdown
# Anothile Bhengu — Integration & Services Specialist
**Student Number:** ST10440981
**Branch:** `feature/payment-notification-logic`
**Working Directory:** `src/UniCribz.Api/Notifications/` and `src/UniCribz.Api/Services/`

---

## ✅ What Has Already Been Done (By Siyanda)

| Item | Location |
|------|----------|
| All NuGet packages installed | SendGrid, Twilio, Stripe.net, RestSharp |
| Notification folder structure created | `src/UniCribz.Api/Notifications/` |
| Placeholder notification files created | 6 files (interfaces + strategies) |
| Placeholder service files created | `Services/{Payment,GoogleMaps,Notification}Service.cs` |
| Placeholder controllers created | `Controllers/{Payment,GoogleMaps,Notification}Controller.cs` |
| Placeholder interfaces created | `Services/Interfaces/I{...}Service.cs` |
| `appsettings.json` has keys for all APIs | SendGrid, Twilio, Stripe, GoogleMaps sections |
| `ServiceCollectionExtensions.cs` ready to register your services | `src/UniCribz.Api/Extensions/ServiceCollectionExtensions.cs` |

**Important note:** The package `GoogleMapsApi` (v6) does NOT exist. We use **`RestSharp` v112** instead — you'll call the Google Maps REST API directly with `RestClient`.

---

## 🎯 Where YOU Need to Start

### Step 0: Get the project running

```bash
git checkout develop
git pull
git checkout -b feature/payment-notification-logic
powershell -ExecutionPolicy Bypass -File setup/00-run-all.ps1
dotnet build
```

### Step 1: Strategy Pattern — Notifications (START HERE, 4 files)

**The Strategy Pattern is a key architectural requirement in the project doc (page 47).**

**Files to implement (in order):**

1. **`src/UniCribz.Api/Notifications/INotificationStrategy.cs`**
```csharp
public interface INotificationStrategy
{
    Task SendAsync(string recipient, string message);
}
```

2. **`EmailNotificationStrategy.cs`** — implement using SendGrid
   - Inject `IConfiguration` to read `SendGrid:ApiKey`
   - Inject `ILogger<EmailNotificationStrategy>`
   - `SendAsync` creates a `SendGridMessage`, sends via `SendGridClient`

3. **`SMSNotificationStrategy.cs`** — implement using Twilio
   - Read `Twilio:AccountSid`, `Twilio:AuthToken`, `Twilio:FromNumber` from config
   - Use `Twilio.Rest.Api.V2010.Account.MessageResource.CreateAsync(...)`

4. **`InAppNotificationStrategy.cs`** — save to DB
   - Inject `UniCribzDbContext`
   - Create a `Notification` entity, save to database
   - (Wait for Siyabonga to finish the `Notification` entity)

5. **`INotificationManager.cs`** and **`NotificationManager.cs`**
```csharp
public class NotificationManager : INotificationManager
{
    private readonly IEnumerable<INotificationStrategy> _strategies;
    
    public NotificationManager(IEnumerable<INotificationStrategy> strategies)
    {
        _strategies = strategies;
    }
    
    public async Task NotifyAsync(string recipient, string message)
    {
        foreach (var strategy in _strategies)
        {
            await strategy.SendAsync(recipient, message);
        }
    }
}
```

6. **Register in `ServiceCollectionExtensions.cs`:**
```csharp
services.AddScoped<INotificationStrategy, EmailNotificationStrategy>();
services.AddScoped<INotificationStrategy, SMSNotificationStrategy>();
services.AddScoped<INotificationStrategy, InAppNotificationStrategy>();
services.AddScoped<INotificationManager, NotificationManager>();
```

### Step 2: Payment Service (Stripe integration)

**Files:**
- `src/UniCribz.Api/Services/Interfaces/IPaymentService.cs`
- `src/UniCribz.Api/Services/PaymentService.cs`
- `src/UniCribz.Api/Controllers/PaymentController.cs`

**Implementation plan:**
1. Configure Stripe in `Program.cs` (already loads `Stripe:SecretKey` from config)
2. `PaymentService.InitiatePaymentAsync()`:
   - Creates a `Payment` entity with status `Pending`
   - Calls `Stripe.Checkout.Session.CreateAsync(...)` to get a payment URL
   - Returns the URL
3. `PaymentService.HandleWebhookAsync()`:
   - Verifies webhook signature
   - Updates `Payment.Status` to `Verified` or `Failed`
   - Triggers a notification via `INotificationManager`
4. `PaymentController`:
   - `POST /api/payments` → initiate
   - `POST /api/payments/webhook` → Stripe callback
   - `POST /api/payments/{id}/proof` → upload proof of payment (uses Blob Storage)

### Step 3: Google Maps Service (RestSharp)

**Files:**
- `src/UniCribz.Api/Services/Interfaces/IGoogleMapsService.cs`
- `src/UniCribz.Api/Services/GoogleMapsService.cs`
- `src/UniCribz.Api/Controllers/GoogleMapsController.cs`

**Implementation plan (using RestSharp):**
```csharp
public async Task<(double Lat, double Lng)?> GeocodeAsync(string address)
{
    var client = new RestClient("https://maps.googleapis.com/maps/api/geocode/json");
    var request = new RestRequest();
    request.AddQueryParameter("address", address);
    request.AddQueryParameter("key", _config["GoogleMaps:ApiKey"]);
    
    var response = await client.GetAsync<GeocodeResponse>(request);
    // Parse and return lat/lng
}
```

**Endpoints to expose:**
- `GET /api/maps/geocode?address=...` — returns `{lat, lng}`
- `GET /api/maps/nearby?lat=...&lng=...` — returns nearby facilities

Samkelsiwe will consume these endpoints to render markers on the property map.

### Step 4: Notification Service (wrapper)

**File:** `src/UniCribz.Api/Services/NotificationService.cs`

Wrap `INotificationManager` in a service that:
- Handles business logic (which user gets what notification)
- Logs notifications
- Saves in-app notifications to DB via `InAppNotificationStrategy`

### Step 5: Research documentation (Task 1.1.1)

**File to create:** `docs/research/accommodation-market-research.md`

- Study the South African student housing market
- Analyze 3-5 competitors (e.g., DigsConnect, StudentDigz, NSFAS-affiliated platforms)
- Validate business hypotheses from the project doc
- Cite sources (IJR, Sabinet, IOL Property — already listed in the project doc)

---

## 🔗 What You Depend On

| From | What | When |
|------|------|------|
| **Siyabonga** | `Payment` and `Notification` entities defined | Week 1 |
| **Siyanda** | `Program.cs` with Stripe/SendGrid/Twilio configuration | ✅ Done |
| **Siyanda** | Base controller patterns and API conventions | Week 1 |
| **Samkelsiwe** | UI triggers (a "Pay Now" button, notification bell icon) | Week 3 |

---

## 🔑 API Keys — Where They Go

| Key | Location | How to Get |
|-----|----------|-----------|
| SendGrid | `SendGrid:ApiKey` in `appsettings.Development.json` | https://signup.sendgrid.com (free tier) |
| Twilio | `Twilio:AccountSid`, `Twilio:AuthToken`, `Twilio:FromNumber` | https://www.twilio.com/try-twilio (free trial) |
| Stripe | `Stripe:SecretKey`, `Stripe:PublishableKey`, `Stripe:WebhookSecret` | https://dashboard.stripe.com/test/apikeys |
| Google Maps | `GoogleMaps:ApiKey` | https://console.cloud.google.com/apis/credentials |

**Never commit real keys.** Use `dotnet user-secrets` for local dev:
```bash
cd src/UniCribz.Api
dotnet user-secrets init
dotnet user-secrets set "SendGrid:ApiKey" "SG.xxxxx"
dotnet user-secrets set "Twilio:AccountSid" "ACxxxxx"
dotnet user-secrets set "Stripe:SecretKey" "sk_test_xxxxx"
dotnet user-secrets set "GoogleMaps:ApiKey" "AIzaxxxxx"
```

---

## 🔁 Daily Git Workflow

```bash
git checkout develop
git pull
git checkout feature/payment-notification-logic
git merge develop

git add .
git commit -m "feat(api): implement EmailNotificationStrategy with SendGrid"
git push
```

---

## ❓ If Something Breaks

- RestSharp not restoring? `dotnet nuget locals all --clear && dotnet restore`
- SendGrid/Twilio API key invalid? Test at their dashboard first
- Stripe webhook signature failing? Make sure `Stripe:WebhookSecret` matches the dashboard