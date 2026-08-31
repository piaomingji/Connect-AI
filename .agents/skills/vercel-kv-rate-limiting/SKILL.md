---
name: vercel-kv-rate-limiting
description: Guide and patterns for implementing IP and User-based rate limiting (permanent or time-based reset) in Next.js applications using @vercel/kv or Upstash Redis.
---

# Vercel KV Rate Limiting Integration Guide

This skill provides step-by-step instructions and reusable code patterns to implement IP-based and/or User-based rate limiting in Next.js API Routes using Vercel KV (Upstash Redis).

## 1. Prerequisites

Ensure `@vercel/kv` is installed in the target project:
```bash
npm install @vercel/kv
```

Make sure the following environment variables are set in Vercel (or locally in `.env.local` for development):
- `KV_REST_API_URL` or `REDIS_REST_API_URL`
- `KV_REST_API_TOKEN` or `REDIS_REST_API_TOKEN`

---

## 2. KV Client Initialization

Add this snippet to initialize the KV client in your API Route (e.g., `app/api/generate/route.ts`):

```typescript
import { createClient } from '@vercel/kv';

const kv = createClient({
  url: process.env.KV_REST_API_URL || process.env.REDIS_REST_API_URL || "",
  token: process.env.KV_REST_API_TOKEN || process.env.REDIS_REST_API_TOKEN || "",
});
```

---

## 3. Rate Limit Patterns

### Pattern A: 72-Hour Reset Rate Limiting (With Redis TTL)

Use this pattern when you want to grant a set number of free generations (e.g., 5 times) that automatically reset 72 hours after the first generation.

#### 1. Rate Limit Verification (Before executing heavy operations)
```typescript
const ip = req.headers.get("x-forwarded-for")?.split(",")[0].trim() || req.ip || "127.0.0.1";
const ipKey = `project-name:ip:${ip}`;
const googleKey = finalUserIdentifier ? `project-name:google:${finalUserIdentifier}` : null;

const currentIpCount = (await kv.get<number>(ipKey)) || 0;
const currentGoogleCount = googleKey ? ((await kv.get<number>(googleKey)) || 0) : 0;

if (currentIpCount >= 5 || currentGoogleCount >= 5) {
  return NextResponse.json(
    { error: '無料体験枠（5回）をすべて消費しました。72時間後にリセットされます。引き続きご利用いただくには有料プランをご検討ください。' },
    { status: 429 }
  );
}
```

#### 2. Increment Counter with Expiration (After successful generation)
```typescript
// Increment IP count with 72-hour TTL
const ipKey = `project-name:ip:${ip}`;
const currentIpCount = (await kv.get<number>(ipKey)) || 0;
if (currentIpCount === 0) {
  await kv.set(ipKey, 1, { ex: 72 * 60 * 60 });
} else {
  const ttl = await kv.ttl(ipKey);
  await kv.set(ipKey, currentIpCount + 1, ttl > 0 ? { ex: ttl } : { ex: 72 * 60 * 60 });
}

// Increment Google Account count with 72-hour TTL
if (finalUserIdentifier) {
  const googleKey = `project-name:google:${finalUserIdentifier}`;
  const currentGoogleCount = (await kv.get<number>(googleKey)) || 0;
  if (currentGoogleCount === 0) {
    await kv.set(googleKey, 1, { ex: 72 * 60 * 60 });
  } else {
    const ttl = await kv.ttl(googleKey);
    await kv.set(googleKey, currentGoogleCount + 1, ttl > 0 ? { ex: ttl } : { ex: 72 * 60 * 60 });
  }
}
```

### Pattern B: Permanent (No Reset) Rate Limiting

Use this pattern when you want to enforce a lifetime-only trial (e.g., 3 times total) without reset.

#### 1. Rate Limit Verification
```typescript
const ipKey = `project-name:ip:${ip}`;
const count = (await kv.get<number>(ipKey)) || 0;

if (count >= 3) {
  return NextResponse.json(
    { error: "無料お試しの制限回数（3回）を超過しました。引き続き生成するには、有料プランへのご加入をお願いいたします。" },
    { status: 403 }
  );
}
```

#### 2. Increment Counter permanently
```typescript
const ipKey = `project-name:ip:${ip}`;
await kv.set(ipKey, count + 1);
```
