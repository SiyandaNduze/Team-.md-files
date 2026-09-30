## 📄 File 3: `3-siyabonga-database.md`

```markdown
# Siyabonga Cebekhulu — Database & Backend Developer
**Student Number:** ST10440807
**Branch:** `feature/database-schema-design`
**Working Directory:** `src/UniCribz.Data/`

---

## ✅ What Has Already Been Done (By Siyanda)

| Item | Location |
|------|----------|
| `UniCribz.Data` project created | `src/UniCribz.Data/UniCribz.Data.csproj` |
| EF Core + Npgsql packages installed | Microsoft.EntityFrameworkCore 8.0, Npgsql 8.0 |
| Referenced from `UniCribz.Api` | ✅ (API can use your DbContext) |
| All folders created | Entities, Enums, Context, Configurations, Repositories, Migrations, Seed |
| **21 placeholder entity files created** | `src/UniCribz.Data/Entities/*.cs` |
| **8 placeholder enum files created** | `src/UniCribz.Data/Enums/*.cs` |
| **6 EF configuration files created** | `src/UniCribz.Data/Configurations/*.cs` |
| **4 repository pattern files created** | `src/UniCribz.Data/Repositories/*.cs` |
| `DataSeeder.cs` placeholder created | `src/UniCribz.Data/Seed/DataSeeder.cs` |
| `UniCribzDbContext.cs` scaffolded | `src/UniCribz.Data/Context/UniCribzDbContext.cs` |
| `docker-compose.yml` ready for local PostgreSQL + Redis | Root of repo |
| Connection string placeholder in `appsettings.json` | `Host=localhost;Port=5432;Database=UniCribzDb` |

**Every placeholder has a `// TODO` comment.** Just open in Visual Studio and fill in.

---

## 🎯 Where YOU Need to Start

### Step 0: Get the DB running

```bash
git checkout develop
git pull
git checkout -b feature/database-schema-design

# Start PostgreSQL + Redis locally
docker-compose up -d

# Verify
docker ps
# You should see unicribz-postgres and unicribz-redis
```

### Step 1: Enums (start here — 10 minutes)

**Files in `src/UniCribz.Data/Enums/`:**

```csharp
// UserRole.cs
public enum UserRole { Visitor, Tenant, Admin }

// RoomStatus.cs
public enum RoomStatus { Available, Reserved, Occupied, UnderMaintenance }

// ApplicationStatus.cs
public enum ApplicationStatus { Submitted, UnderReview, Approved, Rejected, RoomAllocated }

// PaymentStatus.cs
public enum PaymentStatus { Pending, Verified, Paid, Failed }

// MaintenanceStatus.cs
public enum MaintenanceStatus { Submitted, Assigned, InProgress, OnHold, Completed, Reopened }

// ComplaintStatus.cs
public enum ComplaintStatus { Submitted, UnderReview, Resolved }

// LeaseStatus.cs
public enum LeaseStatus { PendingSignature, Active, Terminated }

// ViewingStatus.cs
public enum ViewingStatus { Requested, Approved, Rejected, Rescheduled, Completed }
```

### Step 2: Entities (the main task — 3-4 hours)

**Reference:** Domain Class Diagram (page 21), ERD (page 44) of the project document.

**Order to implement (bottom-up to avoid dangling references):**

1. `University.cs`
2. `User.cs` (base class — Visitor, Tenant, Admin inherit)
3. `Visitor.cs`, `Tenant.cs`, `Admin.cs` (inherit from User)
4. `Amenity.cs`
5. `Property.cs`
6. `PropertyImage.cs`
7. `Room.cs`
8. `Application.cs`
9. `ApplicationDocument.cs`
10. `Viewing.cs`
11. `Lease.cs`
12. `Payment.cs`
13. `MaintenanceRequest.cs`
14. `MaintenancePhoto.cs`
15. `Complaint.cs`
16. `Announcement.cs`
17. `Notification.cs`
18. `Document.cs`
19. `InventoryItem.cs`

**All entities should:**
- Use `Guid` for primary keys (named `Id`)
- Use `DateTime` for timestamps (named `CreatedAt`, `UpdatedAt`)
- Include navigation properties for relationships
- Use nullable reference types (`string?` for optional fields)

**Example — `User.cs`:**
```csharp
namespace UniCribz.Data.Entities;

public class User
{
    public Guid Id { get; set; } = Guid.NewGuid();
    public string FullName { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
    public string PasswordHash { get; set; } = string.Empty;
    public string PhoneNumber { get; set; } = string.Empty;
    public UserRole Role { get; set; }
    public Guid? UniversityId { get; set; }
    public University? University { get; set; }
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
    public DateTime? UpdatedAt { get; set; }

    // Navigation
    public Tenant? Tenant { get; set; }
    public Admin? Admin { get; set; }
}
```

### Step 3: Add DbSets to Context

**File:** `src/UniCribz.Data/Context/UniCribzDbContext.cs`

Uncomment/add DbSets as you create entities:
```csharp
public DbSet<User> Users => Set<User>();
public DbSet<University> Universities => Set<University>();
public DbSet<Property> Properties => Set<Property>();
public DbSet<Room> Rooms => Set<Room>();
public DbSet<Application> Applications => Set<Application>();
public DbSet<Lease> Leases => Set<Lease>();
public DbSet<Payment> Payments => Set<Payment>();
public DbSet<MaintenanceRequest> MaintenanceRequests => Set<MaintenanceRequest>();
public DbSet<Complaint> Complaints => Set<Complaint>();
public DbSet<Announcement> Announcements => Set<Announcement>();
public DbSet<Notification> Notifications => Set<Notification>();
public DbSet<Amenity> Amenities => Set<Amenity>();
public DbSet<Viewing> Viewings => Set<Viewing>();
```

### Step 4: EF Configurations (Fluent API)

**Files in `src/UniCribz.Data/Configurations/`:**

Each configuration implements `IEntityTypeConfiguration<T>`:
```csharp
public class UserConfiguration : IEntityTypeConfiguration<User>
{
    public void Configure(EntityTypeBuilder<User> builder)
    {
        builder.HasKey(u => u.Id);
        builder.Property(u => u.Email).IsRequired().HasMaxLength(255);
        builder.HasIndex(u => u.Email).IsUnique();
        builder.HasOne(u => u.University)
               .WithMany()
               .HasForeignKey(u => u.UniversityId);
    }
}
```

### Step 5: Create the first migration

**Once 5+ entities are done:**
```bash
cd src/UniCribz.Api
dotnet ef migrations add InitialCreate --project ../UniCribz.Data
dotnet ef database update --project ../UniCribz.Data
```

**Verify** — check tables are created:
```bash
docker exec -it unicribz-postgres psql -U postgres -d UniCribzDb -c "\dt"
```

### Step 6: Repository Pattern

**Files in `src/UniCribz.Data/Repositories/`:**

```csharp
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(Guid id);
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(Guid id);
}
```

Implement `Repository<T>` using `UniCribzDbContext`. Register in `ServiceCollectionExtensions.cs`:
```csharp
services.AddScoped(typeof(IRepository<>), typeof(Repository<>));
```

### Step 7: Data Seeder

**File:** `src/UniCribz.Data/Seed/DataSeeder.cs`

Seed:
- 1 admin (`admin@unicribz.co.za` / `Admin@123`)
- 3 universities (UJ, Wits, TUT)
- 5 amenities (Wi-Fi, Laundry, Parking, Study Room, Security)
- 1 sample property with 3 rooms

Called from `Program.cs` on startup (only if DB is empty).

### Step 8: Data Migration Plan document

**File to create:** `docs/data-migration-plan.md`

Outline: extract from spreadsheets → clean → map to new schema → load to staging → validate → cutover.

---

## 🔗 What You Depend On

| From | What | When |
|------|------|------|
| **Siyanda** | PostgreSQL docker-compose running | ✅ Done |
| **Siyanda** | Connection string configured | ✅ Done |
| **Anothile** | Payment/Notification data requirements | Week 1 |

**You are the critical path** — Siyanda can't build API controllers without your entities. Prioritize `User`, `Property`, `Room`, `Application`, `Lease`.

---

## 🔁 Daily Git Workflow

```bash
git checkout develop
git pull
git checkout feature/database-schema-design
git merge develop

git add .
git commit -m "feat(data): add User, Tenant, Admin entities with inheritance"
git push
```

---

## ❓ If Something Breaks

- `docker-compose up -d` fails? Make sure Docker Desktop is running
- `dotnet ef` not found? `dotnet tool install --global dotnet-ef`
- Migration fails? Check the exact error — usually a missing `using` or wrong type
- DB connection refused? Check the container is healthy: `docker ps`