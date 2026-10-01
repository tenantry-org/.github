<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/tenantry-org/.github/master/profile/tenantry-logo-white.svg">
    <img alt="Tenantry" src="https://raw.githubusercontent.com/tenantry-org/.github/master/profile/tenantry-logo.svg" width="300">
  </picture>
</p>

<p align="center">
  Modern multi-tenancy for .NET
  <br>
  <a href="https://tenantry.dev/docs/core/getting-started">Get started</a> ·
  <a href="https://tenantry.dev/docs">Docs</a> ·
  <a href="https://www.nuget.org/packages/Tenantry.Core">NuGet</a> ·
  <a href="https://tenantry.dev/#pricing">Tenantry Pro</a>
</p>

Tenantry isolates each tenant's data in a shared database, or gives every tenant a database of its own. It does this without a base class on your entities, without a custom DbContext and without taking over your request pipeline.

```csharp
builder.Services.AddTenantry<Guid>(tenant => tenant
    .ResolveFromClaim("tenant_id")   // where the tenant comes from
    .UseStore<TenantStore>());       // where tenants are defined

builder.Services.AddDbContext<AppDbContext>(options => options
    .UseSqlServer(connectionString)
    .UseTenantry());                 // how data is isolated
```

- **Unopinionated.** Use a `Guid`, `int`, `string` or your own type as the tenant key. Resolve it from a claim, header, subdomain, route or query string, and keep your tenants wherever you like.
- **One call per DbContext.** `UseTenantry()` isolates any DbContext, pooled or not, including pooled contexts with a database per tenant.
- **Fails closed.** With no tenant, queries return no rows. Writes to another tenant's data are rejected before they are saved, and the stored tenant id is part of every `UPDATE` and `DELETE`.
- **Beyond HTTP.** Workers, console apps and desktop apps get the same isolation inside tenant scopes, and `Tenantry.Core` depends on nothing but the DI abstractions.

Tenantry is open source under Apache 2.0. [Tenantry Pro](https://tenantry.dev/#pricing) adds what running many tenant databases takes: provisioning, schema per tenant, migrations across every tenant database, and tenant context in Hangfire, MassTransit, Quartz.NET and Rebus.
