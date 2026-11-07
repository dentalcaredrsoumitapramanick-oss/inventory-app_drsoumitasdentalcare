# Dental Care Inventory — setup 

React + Vite web app with Supabase cloud authentication and PostgreSQL storage.

## Features
- Doctor and receptionist accounts (new accounts default to receptionist)
- Permanent inventory item, stock batch and movement records
- Purchases with supplier, batch/lot, purchase date and expiry date
- Usage deduction using FEFO (first-expiring, first-out) batch order
- Low-stock dashboard alerts and 30-day expiry watch
- Search and permanent movement history
- Supabase Row Level Security policies

## Deploy / run
1. Create a project at https://supabase.com.
2. In Supabase SQL Editor, run all of `supabase/schema.sql`.
3. Copy `.env.example` to `.env` and fill in the Supabase Project URL and anon/public key from Project Settings → API.
4. Install Node.js 20+, then run:

   ```bash
   npm install
   npm run dev
   ```

5. To build for hosting: `npm run build`. Deploy the `dist` folder to Vercel, Netlify, or another static host; configure the same environment variables there.

## Set up Doctor and Receptionist
1. Register the first user in the app.
2. Copy that user's UUID from Supabase → Authentication → Users.
3. Run this in SQL Editor, replacing the UUID:

   ```sql
   update public.profiles set role='doctor' where id='AUTH_USER_UUID';
   ```

4. Other staff members register normally and receive the receptionist role by default.

## Security and production notes
- Use only the Supabase anon/public key in the browser. Never expose the service-role key.
- RLS is enabled on all tables. Doctor can delete records; receptionist can create and read stock records.
- Enable email confirmation and MFA for the Doctor account; configure backups/PITR in Supabase.
- Dashboard alerts are shown when staff open the app. Automatic email/SMS alerts need a scheduled Supabase Edge Function and a notification provider; they are not configured in this starter.
- Avoid entering patient-identifying or clinical information in the optional reference field.
