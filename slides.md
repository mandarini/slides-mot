---
theme: seriph
background: https://images.unsplash.com/photo-1518709268805-4e9042af2176?ixlib=rb-4.0.3&auto=format&fit=crop&w=2426&q=80
class: "text-center"
highlighter: shiki
lineNumbers: false
info: |
  ## Stop Mocking Your Backend
  Testing with Real Supabase Backends
drawings:
  persist: false
css: unocss
wakeLock: "build"
---

# Stop Mocking Your Backend

## Katerina Skroumpelou

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    MOT Athens <carbon:arrow-right class="inline"/>
  </span>
</div>

<p><em>Follow along: slides-mot.netlify.app</em></p>

<div class="abs-br m-6 text-xl">
  <logos-supabase-icon />
</div>

<!--
Welcome everyone! Today I want to change how you think about testing. We've all been told to mock our backends, but what if I told you there's a better way? With Supabase, you can test against the exact same stack you use in production - locally, for free, in seconds.
-->

---
layout: center
class: text-center
---

# About Me

<div class="grid grid-cols-2 gap-12 items-center">
<div>

# Katerina Skroumpelou

- Software Engineer at **Supabase**
- Loves cats and chocolate
- Loves to be on stage
- **GDE** for Angular & Maps

## psyber.city - @psyber.city

</div>
<div>
<img src="/images/katsup.jpg" class="rounded-full w-64 mx-auto shadow-lg">
</div>
</div>

<!--
Quick intro - I'm Katerina, a Software Engineer at Supabase. I love cats and chocolate, I love being on stage talking to all of you, and I'm a Google Developer Expert for Angular and Maps. Today I want to show you how to stop mocking and start testing with real backends.
-->

---
layout: center
class: text-center
---

<img src="/images/cats.jpg" class="max-w-[90vw] max-h-[80vh] object-contain mx-auto" alt="My cats">

---
layout: center
class: text-center
---

# Build in a Weekend. Scale to Millions.

> "Supabase is open source. We choose open source tools which are scalable and make them simple to use."

### What is Supabase?

<div class="grid grid-cols-2 gap-8 mt-8 text-left">
<div>

- **The open-source Firebase alternative** built on Postgres
- Everything you need to build and scale apps

</div>
<div>

- Fully integrated, yet modular
- **No lock-in** — open source all the way down

</div>
</div>

<!--
Before we dive into testing, let me quickly introduce Supabase for those who haven't used it yet.

Supabase is an open-source backend platform. Think of it as 'Firebase, but with Postgres.' It gives you everything you need to build a product: database, authentication, file storage, edge functions, realtime subscriptions.

The key difference? It's built on Postgres — the world's most advanced open-source database with 30+ years of active development. And because it's open source, there's no vendor lock-in. You can self-host or migrate anytime.
-->

---

# What's in Supabase?

| Feature | Description |
|---------|-------------|
| **Database** | Full Postgres with 50+ extensions (pgvector, PostGIS, etc.) |
| **Auth** | Email/password, OAuth, Magic Links, Enterprise SSO |
| **Storage** | S3-compatible file storage with RLS |
| **Edge Functions** | Server-side TypeScript/Deno at the edge |
| **Realtime** | WebSockets for live updates & multiplayer |
| **Auto-generated APIs** | REST and GraphQL from your schema |

<!--
Here's what you get with Supabase:

A full Postgres database — not a simplified version. Full postgres-level access with extensions like pgvector for AI, PostGIS for geospatial.

Authentication — complete auth system with social logins, magic links, even enterprise SSO.

Storage — S3-compatible file storage that integrates with your database security.

Edge Functions — run server-side code in Deno at the edge.

Realtime — WebSocket connections for live updates.

Auto-generated APIs — PostgREST turns your schema into REST APIs automatically. No backend code needed for CRUD.

The important thing for today's talk: all of this runs locally. The same Docker images. The same services. That's what makes testing against the real backend possible.
-->

---

# The Architecture

<div class="text-sm">

```
┌─────────────────────────────────────────────────────────────┐
│                     Your Application                        │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Kong API Gateway                         │
└─────────────────────────────────────────────────────────────┘
        │           │           │           │           │
        ▼           ▼           ▼           ▼           ▼
   ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
   │PostgREST│ │ GoTrue  │ │Realtime │ │ Storage │ │Functions│
   │  (API)  │ │ (Auth)  │ │  (WS)   │ │ (Files) │ │ (Edge)  │
   └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘ └────┬────┘
        │           │           │           │           │
        └───────────┴───────────┴─────┬─────┴───────────┘
                                      ▼
                    ┌─────────────────────────────────┐
                    │           Postgres              │
                    │    (The heart of Supabase)      │
                    └─────────────────────────────────┘
```

</div>

**All open source. All running locally.**

<!--
This is the Supabase architecture. Notice something important: everything flows through Postgres.

Kong — the API gateway that routes requests
PostgREST — auto-generates REST APIs by reading your Postgres schema
GoTrue — handles authentication, stores users in auth.users (a Postgres table!)
Realtime — watches Postgres WAL for changes, broadcasts via WebSockets
Storage — stores file metadata in Postgres with RLS
Edge Functions — Deno runtime for custom logic

Why does this matter for testing? Because everything is Postgres-centric, when you test Postgres, you test everything. And these exact same Docker images run in production. Same code. Same behavior. That's production parity you can trust.
-->

---
layout: center
---

# Who Uses Supabase?

<div class="grid grid-cols-2 gap-12">
<div>

### From indie hackers to Fortune 500

- **5,000,000+** developers
- **93,000+** GitHub stars
- **16,000,000+** databases created
- **90,000+** databases launched daily
- More than half of every **Y Combinator** batch

</div>
<div>

### Notable users

Udio, Lovable, GitHub, Resend, Kayhan Space, Deriv, Mozilla, PWC, 1Password

</div>
</div>

<!--
Supabase isn't just for side projects. Look at these numbers:

5 million developers
93,000 GitHub stars — one of the fastest growing open source projects
16 million databases created

Companies like Udio (AI music), Lovable (AI dev tools), Mozilla, 1Password, PWC — they all use Supabase in production.

And here's a fun fact: more than half of every Y Combinator batch builds on Supabase. These are companies that need to move fast AND scale.

So when I tell you to test against a real Supabase backend, I'm talking about the same infrastructure running millions of requests for real companies.
-->

---
layout: center
class: text-center
---

# The Problem with Mocks

> "Testing is a critical part of database development, especially when working with features like Row Level Security (RLS) policies."

<!--
Let's be honest - mocks are lies we tell ourselves. They make tests pass, but they don't catch real bugs.
-->

---
layout: center
class: text-center
---

# Why Mocks Fall Short

<div class="text-left max-w-2xl mx-auto text-xl space-y-4">

- Mock behavior may not match reality
- Doesn't catch integration issues
- Requires maintenance when API changes
- **Doesn't test RLS policies**
- **Doesn't test database constraints**

</div>

<!--
Here's what mocks CAN'T test:

Row Level Security - The cornerstone of Supabase security. RLS policies are SQL rules that run at the database level. You literally cannot mock this in JavaScript.

Database constraints - Foreign keys, unique constraints, check constraints - these are enforced by Postgres, not your mock.

Real auth flows - JWT validation, session management, token refresh - mocks just return what you tell them to.

I've seen production bugs that passed all mock tests because the mock didn't match reality. That's time and money wasted.
-->

---

# The Supabase Solution

> "The Supabase CLI enables you to run the entire Supabase stack locally, on your machine or in a CI environment."

### Two commands to get started:

```bash
supabase init    # Create a new local project
supabase start   # Launch the Supabase services
```

<!--
This is the magic of Supabase - two commands and you have the ENTIRE production stack running locally. Not a simplified version. Not a mock. The actual services.

Why is this possible? Because Supabase is built on open source tools:
- Postgres - The world's most advanced open source database (30+ years of development)
- PostgREST - Auto-generates REST APIs from your database
- GoTrue - JWT-based authentication
- Realtime - WebSocket server for live updates
- Kong - API gateway

These same Docker images run in production. Same code. Same behavior. That's production parity you can trust.
-->

---

# What You Get Locally

```
Started supabase local development setup.

         API URL: http://localhost:54321
          DB URL: postgresql://postgres:postgres@localhost:54322/postgres
      Studio URL: http://localhost:54323
     Mailpit URL: http://localhost:54324
   Publishable Key: sb_publishable_...
        Secret Key: sb_secret_...
```

<!--
Look at what you get! This isn't a toy - it's a full backend. The keys work exactly like in production. Your client code doesn't change between local and production - just the URL.
-->

---

# Full Stack Included

<div class="grid grid-cols-2 gap-12 text-xl">
<div>

- **Postgres** - Real database, not a mock
- **Auth (GoTrue)** - Complete authentication
- **Storage** - File storage with RLS
- **Realtime** - WebSocket connections

</div>
<div>

- **Edge Functions** - Deno runtime
- **Studio** - Visual dashboard
- **Mailpit** - Email testing

</div>
</div>

<!--
A real Postgres database - with pgvector, PostGIS, and 50+ extensions available
Real authentication - sign up, sign in, OAuth, magic links - it all works
Mailpit - catches all emails locally so you can test password resets, magic links, confirmations
Studio - a full visual dashboard to inspect your data, run SQL, manage users

This is the same architecture used by companies running millions of requests. You're testing against production-grade infrastructure.
-->

---

# Why Supabase Architecture Matters

```
Your App → Kong API Gateway → Services → Postgres
                ↓
         ┌──────┴──────┐
         │  PostgREST  │ ← Auto-generated REST API
         │  GoTrue     │ ← Authentication
         │  Realtime   │ ← WebSockets
         │  Storage    │ ← S3-compatible files
         │  Functions  │ ← Edge Functions (Deno)
         └─────────────┘
```

**Everything flows through Postgres** = Test Postgres, test everything

<!--
Let me explain WHY Supabase testing works so well. It's the architecture.

Everything flows through Postgres. This is intentional. Supabase isn't trying to be clever - it's using the most battle-tested database in existence.

PostgREST reads your schema and generates REST APIs automatically
GoTrue stores users in auth.users - a Postgres table you can query
Storage stores file metadata in Postgres with RLS
Realtime watches Postgres WAL for changes

Because everything is Postgres, you test everything by testing Postgres. And Postgres has pgTAP - a mature testing framework.

This is why the local stack works identically to production - it's the same database, same services, same Docker images.
-->

---

# Local Development Benefits

<div class="text-xl space-y-6">

1. **Faster development** - Make changes and see results instantly

2. **Offline work** - Continue development without internet

3. **Cost-effective** - Local development is free

</div>

<!--
Speed - No network latency. Queries return in milliseconds. Hot reload your migrations.

Offline - On a plane? No wifi? Still productive. Your whole backend runs locally.

Free - No usage limits locally. Spin up 10 projects, run thousands of tests, costs nothing.
-->

---

# Local Development Benefits (cont.)

<div class="text-xl space-y-6">

4. **Enhanced privacy** - Sensitive data stays on your machine

5. **Easy testing** - Experiment without affecting production
   - Reset with `supabase db reset`

</div>

<!--
Privacy - Building something with sensitive data? It never leaves your machine during development.

Experimentation - Try crazy things. Break stuff. Reset with supabase db reset. No consequences.

The key insight: the local environment IS the production environment, just running on your machine. When you deploy, you're deploying the same thing you tested.
-->

---

# Two Testing Approaches

| Approach | Tool | Isolation | Speed |
|----------|------|-----------|-------|
| **Database-level** | pgTAP | Transactions (auto-rollback) | Milliseconds |
| **Application-level** | Vitest/Jest | Unique IDs per test | Fast |

<div class="mt-8">

**Use both for complete coverage:**
- pgTAP for security logic (RLS, permissions)
- Application tests for end-to-end flows

</div>

<!--
Supabase gives you TWO complementary ways to test:

pgTAP (Database-level) - Tests run INSIDE Postgres. They're wrapped in transactions that automatically rollback. This means:
- Tests run in milliseconds
- Perfect isolation - no cleanup needed
- Direct access to test RLS policies

Application tests (Vitest/Jest) - Tests run from your app code, calling the real API. Use unique IDs for isolation.

Use pgTAP for security logic (RLS, permissions). Use application tests for end-to-end flows. Together, they give you complete coverage.
-->

---

# pgTAP - What Is It?

> "pgTAP is a unit testing framework for Postgres that allows testing:
> - Database structure: tables, columns, constraints
> - Row Level Security (RLS) policies
> - Functions and procedures
> - Data integrity"

<div class="mt-8">

**Think of it as Jest/Vitest but for SQL**

</div>

<!--
pgTAP has been around since 2008. It's battle-tested, mature, and specifically designed for Postgres.

Think of it as Jest/Vitest but for SQL. You write tests in SQL, they run in Postgres, and you get TAP output (Test Anything Protocol).

Why this matters for Supabase: RLS policies are the security layer. They determine who can see what data. You CANNOT properly test RLS from outside the database - you need to test at the database level.

pgTAP lets you:
- Create test users
- Switch user contexts (simulate being different users)
- Assert what each user can/cannot see
- All in milliseconds, with automatic cleanup
-->

---

# Row Level Security - The Killer Feature

> "Row Level Security (RLS) is incredibly powerful and flexible, allowing you to write complex SQL rules that fit your unique business needs."

### Why RLS changes everything:

- Security at the **database level**, not application level
- Works with **any client** - REST, GraphQL, direct SQL
- **Defense in depth** - even if your API is compromised
- **You can't mock it** - it's SQL evaluated by Postgres

<!--
This is why testing with a real backend matters. RLS is the crown jewel of Supabase security.

Traditional approach: Your API checks permissions, then queries the database. Problem: Every API endpoint is a potential security hole.

Supabase approach: RLS policies enforce permissions AT THE DATABASE. The API doesn't need to know about permissions - Postgres handles it.

The policy using ((select auth.uid()) = user_id) means: 'Only return rows where the user_id matches the authenticated user's ID.' This runs on EVERY query. You can't bypass it.

This is why mocks fail. A mock doesn't evaluate auth.uid(). It doesn't enforce the policy. It just returns fake data. You need the real database to test real security.
-->

---

# Example Schema - Todos with RLS

```sql
-- Create a simple todos table
create table todos (
  id uuid primary key default gen_random_uuid(),
  task text not null,
  user_id uuid references auth.users not null,
  completed boolean default false
);

-- Enable RLS
alter table todos enable row level security;

-- Create a policy
create policy "Users can only access their own todos"
  on todos for all
  to authenticated
  using ((select auth.uid()) = user_id);
```

<!--
Let's look at a real example. This is a todos table with RLS.

Notice: alter table todos enable row level security. This is CRITICAL. Without it, the table is public.

The policy says: 'For ALL operations (select, insert, update, delete), only allow access to authenticated users, and only for rows where the user_id equals the current user's ID.'

auth.uid() is a Supabase helper function that extracts the user ID from the JWT. It's evaluated at query time.

Key insight: We wrap it in (select auth.uid()) instead of just auth.uid(). This is a performance optimization from the docs - it caches the result per statement instead of evaluating per row.
-->

---

# Creating a pgTAP Test

```bash
# Create a new test file
supabase test new todos_rls
```

Creates: `supabase/tests/todos_rls.test.sql`

<div class="mt-8">

**Pro tip:** Create a `000-setup.test.sql` file for shared extensions and helpers

</div>

<!--
The CLI makes it easy. supabase test new scaffolds a test file for you.

Tests go in supabase/tests/. They're SQL files that run in order (alphabetically).

Pro tip from the docs: Create a 000-setup.test.sql file that installs shared extensions and test helpers. It runs first because of the naming convention.
-->

---

# pgTAP Test - Full Example

```sql {all|1-3|5-8|10-14|16-17|19-23|25-29|31}
begin;
create extension if not exists pgtap with schema extensions;
select plan(4);

-- Setup test users
insert into auth.users (id, email) values
  ('123e4567-e89b-12d3-a456-426614174000', 'user1@test.com'),
  ('987fcdeb-51a2-43d7-9012-345678901234', 'user2@test.com');

-- Create test todos
insert into public.todos (task, user_id) values
  ('User 1 Task 1', '123e4567-e89b-12d3-a456-426614174000'),
  ('User 1 Task 2', '123e4567-e89b-12d3-a456-426614174000'),
  ('User 2 Task 1', '987fcdeb-51a2-43d7-9012-345678901234');

-- Test as User 1
set local role authenticated;
set local request.jwt.claim.sub = '123e4567-e89b-12d3-a456-426614174000';

-- Test 1: User 1 should only see their own todos
select results_eq(
  'select count(*) from todos',
  ARRAY[2::bigint],
  'User 1 should only see their 2 todos'
);

-- Switch to User 2
set local request.jwt.claim.sub = '987fcdeb-51a2-43d7-9012-345678901234';

select * from finish();
rollback;
```

<!--
Let's break this down - this is a complete RLS test:

1. begin; - Start a transaction. Everything happens in isolation.

2. select plan(4); - Tell pgTAP we're running 4 tests.

3. Insert test users - We're inserting directly into auth.users. Only possible in tests!

4. set local role authenticated; - This is KEY. We're simulating an authenticated user.

5. set local request.jwt.claim.sub = '...' - We're setting which user we're pretending to be. This is how auth.uid() knows who's making the request.

6. Test assertions - results_eq checks query results, lives_ok checks SQL doesn't error, results_ne checks inequality.

7. rollback; - Undo everything. The database is clean for the next test.

This runs in MILLISECONDS. No network. No cleanup. Just pure SQL speed.
-->

---

# Running pgTAP Tests

```bash
supabase test db
```

Output:

```
supabase/tests/todos_rls.test.sql .. ok
All tests successful.
Files=1, Tests=4,  0 wallclock secs ( 0.01 usr +  0.00 sys =  0.01 CPU)
Result: PASS
```

**Key point:** Tests wrapped in transactions = automatic rollback!

<!--
One command: supabase test db. That's it.

Look at that output - 0.01 seconds for 4 tests. These aren't slow integration tests. This is the speed of unit tests with the accuracy of integration tests.

The rollback at the end means:
- No test data pollutes your database
- Tests can run in any order
- Tests are completely isolated
- You can run the same test 1000 times, always the same result

Compare this to application tests that need database cleanup, unique IDs, and careful ordering. pgTAP is surgical.
-->

---

# Application-Level Testing

> "Testing through application code provides end-to-end verification. Unlike database-level testing with pgTAP, application-level tests cannot use transactions for isolation."

### Solution: Unique IDs per test suite

Each test suite owns its own "slice" of the database

<!--
pgTAP is great for RLS, but sometimes you need to test the full flow - from your JavaScript code through the API to the database and back.

The challenge: Application tests can't use transactions for isolation. They make HTTP requests that each run in their own database transaction.

The solution from the docs: Unique identifiers per test suite. Generate a unique ID at the start of your test, use it in all test data. Tests don't conflict because each suite has its own namespace.

Think of it as each test suite owning its own 'slice' of the database.
-->

---

# Application Test - Setup

```typescript
import { createClient } from '@supabase/supabase-js'
import { beforeAll, describe, expect, it } from 'vitest'
import crypto from 'crypto'

describe('Todos RLS', () => {
  // Generate unique IDs for this test suite
  const USER_1_ID = crypto.randomUUID()
  const USER_2_ID = crypto.randomUUID()

  const supabase = createClient(
    process.env.SUPABASE_URL!,
    process.env.SUPABASE_PUBLISHABLE_KEY!  // or SUPABASE_ANON_KEY
  )
```

---

# Application Test - beforeAll

```typescript
  beforeAll(async () => {
    const adminSupabase = createClient(
      process.env.SUPABASE_URL!,
      process.env.SUPABASE_SECRET_KEY!  // or SERVICE_ROLE_KEY
    )

    // Create test users with unique IDs
    await adminSupabase.auth.admin.createUser({
      id: USER_1_ID,
      email: `user1-${USER_1_ID}@test.com`,
      password: 'password123',
      email_confirm: true,
    })

    // Create initial todos
    await adminSupabase.from('todos').insert([
      { task: 'User 1 Task 1', user_id: USER_1_ID },
      { task: 'User 1 Task 2', user_id: USER_1_ID },
    ])
  })
```

---

# Application Test - Real Auth

```typescript
  it('should allow User 1 to only see their own todos', async () => {
    // Real authentication
    await supabase.auth.signInWithPassword({
      email: `user1-${USER_1_ID}@test.com`,
      password: 'password123',
    })

    // Real query against real database
    const { data: todos } = await supabase.from('todos').select('*')

    expect(todos).toHaveLength(2)
    todos?.forEach((todo) => {
      expect(todo.user_id).toBe(USER_1_ID)
    })
  })
```

---

# Application Test - Negative Case

```typescript
  it('should prevent User 2 from modifying User 1 todos', async () => {
    await supabase.auth.signInWithPassword({
      email: `user2-${USER_2_ID}@test.com`,
      password: 'password123',
    })

    // Attempt to update - will be a no-op due to RLS
    await supabase.from('todos')
      .update({ task: 'Hacked!' })
      .eq('user_id', USER_1_ID)

    // Verify as User 1
    await supabase.auth.signInWithPassword({
      email: `user1-${USER_1_ID}@test.com`,
      password: 'password123',
    })

    const { data: todos } = await supabase.from('todos').select('*')
    todos?.forEach((todo) => {
      expect(todo.task).not.toBe('Hacked!')
    })
  })
})
```

---

# Test Isolation Strategies

> "Application-level tests should not rely on a clean database state, as resetting the database before each test can be slow and makes tests difficult to parallelize."

### Three strategies:

1. **Unique Identifiers** - Generate unique IDs for each test suite
2. **Cleanup After Tests** - Clean up in `afterAll` or `afterEach` hooks
3. **Isolated Data Sets** - Use prefixes or namespaces

---

# CI/CD Integration

```yaml
name: Database Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Supabase CLI
        uses: supabase/setup-cli@v1
      - name: Start Supabase
        run: supabase start
      - name: Run Tests
        run: supabase test db
```

---

# Best Practices: Test Data

### Test Data Setup
- Use `begin` and `rollback` for isolation
- Create realistic test data
- Test with different user roles

---

# Best Practices: RLS & CI/CD

<div class="grid grid-cols-2 gap-12">
<div>

### RLS Policy Testing
- Test CRUD operations
- Test with anon AND authenticated
- **Always test negative cases**

</div>
<div>

### CI/CD Integration
- Run tests on every PR
- Keep tests fast using transactions

</div>
</div>

---

# Advanced - Test Helpers

```sql
-- Install test helpers
select dbdev.install('basejump-supabase_test_helpers');
create extension if not exists "basejump-supabase_test_helpers";

-- Simplified user management
select tests.create_supabase_user('user1@test.com');
select tests.authenticate_as('user1@test.com');
select tests.get_supabase_uid('user1@test.com');

-- Verify RLS across entire schema
select tests.rls_enabled('public');
```

---

# Comparison Table

| Aspect | Mocks | Real Backend |
|--------|-------|--------------|
| RLS Testing | No | **Yes** |
| Database Constraints | No | **Yes** |
| Auth Flows | Simulated | **Real** |
| Integration Issues | Missed | **Caught** |
| Maintenance | High | **Low** |
| Production Parity | No | **Yes** |
| Speed | Fast | **Also Fast** |

---
layout: center
---

# Key Takeaways

<div class="text-left max-w-2xl">

1. **Supabase makes real backend testing practical**
   - Full stack runs locally with `supabase start`
   - Fast and free

2. **Two complementary approaches**
   - pgTAP for database-level (milliseconds, transactional)
   - Application tests for end-to-end

3. **CI/CD ready out of the box**
   - GitHub Actions support via `supabase/setup-cli`

4. **Stop mocking, start testing for real**

</div>

---

# Resources

<div class="grid grid-cols-2 gap-8">
<div>

### Official Documentation
- [Testing Overview](https://supabase.com/docs/guides/local-development/testing/overview)
- [Advanced pgTAP Testing](https://supabase.com/docs/guides/local-development/testing/pgtap-extended)
- [Local Development](https://supabase.com/docs/guides/local-development)
- [CLI Reference](https://supabase.com/docs/reference/cli/supabase-test)

</div>
<div>

### Community Resources
- [pgTAP Documentation](https://pgtap.org)
- [Test Helpers](https://github.com/usebasejump/supabase-test-helpers)
- [Example Repository](https://github.com/usebasejump/basejump/tree/main/supabase/tests/database)

</div>
</div>

---
layout: center
class: text-center
---

# The Demo

## Cookie Catcher Game

<div class="text-6xl mb-8">🎮</div>

<div class="bg-gray-800 text-white p-6 rounded-lg mt-8">

**Rules:**

- Catch cookies = **1 point**
- Catch cats = **3 points**
- Compete on **live leaderboard!**

</div>

<div class="mt-8 text-gray-600">
<em>Live link will be shared later</em>
</div>

<!--
Now let me show you a live demo of a real-time multiplayer game built with Supabase. This demonstrates the same infrastructure we've been testing - Realtime, Auth, and Row Level Security working together.
-->

---
layout: center
class: text-center
---

# Let's Play Together!

## Cookie Catcher - Live Demo

<div class="text-6xl mb-8">🎮</div>

<div class="flex items-center justify-center gap-12">
  <img src="/images/game-qr.png" class="w-64 h-64" alt="QR Code">
  <h2 class="text-4xl font-bold"><a href="https://ngdemo-sb.netlify.app" target="_blank">ngdemo-sb.netlify.app</a></h2>
</div>

<!--
This is Cookie Catcher - a real-time multiplayer game where everyone in the room can join on their phones right now. You catch falling cookies and cats, compete on live leaderboards, and see each other's cursors moving in real-time. Go ahead, scan this QR code and join the game!

3 top winners get supabase swag
-->

---
layout: center
class: text-center
---

# thankz!

<div class="text-6xl mb-8">🍪</div>

<div class="text-xl space-y-4">

## [ngdemo-sb.netlify.app](https://ngdemo-sb.netlify.app)

**i can has teh codez:** [github.com/mandarini/ac-demo-sb](https://github.com/mandarini/ac-demo-sb)

</div>

<div class="mt-12 text-lg">

**Connect with me:**

- @psybercity
- [psyber.city](https://psyber.city)
- [github.com/mandarini](https://github.com/mandarini)

</div>

<!--
That's how you stop mocking and start testing with real Supabase backends. The combination of pgTAP for database-level tests and application tests with real auth gives you complete coverage with production parity. Thank you for your attention - I'd love to answer any questions you have!
-->

<style>
* {
  font-family: 'Inter', 'Segoe UI', 'Roboto', 'Helvetica Neue', sans-serif;
}

h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>
