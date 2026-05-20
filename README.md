# K-Solar v1.3.0 — Multi-tenant signup

## What's new
- Personal accounts: sign up with any email, email verify, log in immediately
- Company workers: must use email matching a registered company domain
- New "Companies" admin tab (super admin only)
- Workers still require K-Solar admin approval after email verification

## Before deploying
1. Run `migration-v1.3.sql` in Supabase SQL Editor (idempotent, safe to re-run)
2. Replace index.html and version.json in GitHub
3. Vercel auto-deploys

## After deploying — first thing to do
Log in as super admin, go to Companies tab, register your first company:
- Name: Kalonga SunPower & Electrical Services
- Email Domain: kalongasunpower.mw (or whatever your domain is)
- Address: optional

Then workers can sign up with @kalongasunpower.mw emails.
