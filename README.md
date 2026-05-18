# K-Solar v1.2.2 — Database mode, email-confirmation OFF

This build connects to Supabase. Before testing:

In Supabase dashboard:
- Authentication -> Providers -> Email -> Confirm email: OFF -> Save

Without that, signup will fail because Supabase tries to send a verification
email and hits its 2/hour rate limit.

## How signup works
1. User fills the form, clicks Sign Up
2. Supabase creates auth user + profile row in one go
3. Status is 'pending' so login is blocked until admin approves
4. Admin approves in Users tab -> user can sign in

## Default admin
Created manually in Supabase dashboard (Authentication -> Users + matching
profile row with is_admin=true, status='approved').
