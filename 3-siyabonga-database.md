## 📄 File 3: `docs/team/3-siyabonga-database.md`

```markdown
# Siyabonga Cebekhulu — Database & Backend Developer
**Student Number:** ST10440807
**Branch:** `feature/database-schema-design`
**Working Directory:** `src/UniCribz.Data/`

---

## Your Work Is 100% Complete

### Delivered by you
| Item | Status |
|------|--------|
| 21 entities with full properties + navigation | ✅ |
| User TPH inheritance (Visitor/Tenant/Admin) | ✅ |
| 8 enums | ✅ |
| 18 EF Core Fluent API configurations | ✅ |
| `UniCribzDbContext` with 22 DbSets + ApplyConfigurationsFromAssembly | ✅ |
| 2 migrations applied (`InitialCreate`, `SecondMigration`) | ✅ |
| 20 tables in PostgreSQL | ✅ |
| Repository pattern (`IRepository<T>` + specialized) | ✅ |
| Data seeder — 3 users, 3 universities, 5 amenities, 1 property, 3 rooms | ✅ |
| BCrypt-hashed demo passwords | ✅ |

**Verified in production:** Every API endpoint returns data from the tables you designed. Migrations applied cleanly. Foreign keys enforced. Seed data present.

### Production-Hardening Applied to Your Layer
| Item | Status |
|------|--------|
| EF Core retry on transient failures (3 retries, 5s delay) | ✅ |
| Query splitting behavior (SplitQuery for multi-collection includes) | ✅ |
| Command timeout (30s) | ✅ |
| Auto-migration on startup (config-controlled via `Database:MigrateOnStartup`) | ✅ |
| Seed-on-startup configurable via `Database:SeedOnStartup` | ✅ |

These were added by Siyanda in the production-hardening pass — your entities are unchanged.

---

## Still To Do (One Item)

### `docs/data-migration-plan.md`

**This is the only remaining item from your role doc (Step 8).**

**File to create:** `docs/data-migration-plan.md`

**Outline:**

```markdown
# Data Migration Plan — UniCribz

## 1. Current State Assessment
- Source systems: spreadsheets, email inboxes, WhatsApp groups
- Data types: tenant records, lease agreements, payments, properties

## 2. Extraction Phase
- Export existing tenant/property data to CSV
- Extract lease PDFs from email folders
- Document data quality issues found

## 3. Cleansing Phase
- Standardize date formats
- Validate email addresses
- Deduplicate tenant records
- Verify property addresses

## 4. Mapping Phase
- Map each source column → target entity field
- Table mapping:
  | Source | Target Entity | Target Column |
  |--------|---------------|---------------|
  | tenant_name | User | FullName |
  | ... | ... | ... |

## 5. Loading Phase
- Load into staging PostgreSQL
- Use EF Core or raw SQL scripts
- Sample script:
  ```sql
  INSERT INTO "Users" (...) VALUES (...);
  ```

## 6. Validation Phase
- Row counts match source
- Foreign key integrity
- Spot-check 10% of records

## 7. Cutover Phase
- Dry run in staging
- Schedule production window
- Rollback plan
- 30-day read-only legacy fallback
```

**Estimated time:** 30 minutes.

When done:
```bash
git add docs/data-migration-plan.md
git commit -m "docs: add data migration plan"
git push
```

---

## Your Technical Deliverables Are Complete

You're done with everything except the one doc above. If you want to keep contributing after, potential extensions:

- Additional repository patterns for other entities
- Soft-delete filters via EF Core global query filters
- Audit fields (CreatedBy, UpdatedBy) with EF Core interceptors
- Additional seed data (test leases, payments, complaints, announcements)

---

## Reference

- **Project doc:** Domain Class Diagram (p.21), ERD (p.44)
- **DB status:** All 20 tables created and verified
- **Migrations:** `20260930143546_InitialCreate`, `20261001083400_SecondMigration`