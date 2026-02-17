# 🚀 Project Handoff Guide: sudokuhub.live

Welcome to your new Sudoku platform! This guide will walk you through the steps to get the project running on your own infrastructure.

## 📋 Prerequisites

- **Node.js 18+** installed.
- A **Vercel** account (for hosting).
- A **Supabase** account (for database & auth).

---

## 🛠️ Step 1: Supabase Setup (Database & Auth)

1. **Create a new project** on [Supabase](https://supabase.com).
2. **Setup Database**:
   - Go to the **SQL Editor** in your Supabase dashboard.
   - Create a "New Query".
   - Copy the contents of the `supabase.sql` file (found in the project root) and paste them into the editor.
   - Click **Run**. This will create the `profiles`, `levels_completed`, `purchases`, and `chat_messages` tables, along with the necessary security policies (RLS).
3. **Authentication**:
   - Go to **Authentication > Providers** and ensure "Email" is enabled.
   - (Optional) Configure SMTP if you want to send real recovery/confirmation emails.

---

## 🚀 Step 2: Vercel Deployment

1. **Import the repository** from GitHub to Vercel.
2. **Configure Environment Variables**:
   In the Vercel project settings, add the following variables:
   - `NEXT_PUBLIC_SUPABASE_URL`: Your Supabase Project URL.
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`: Your Supabase Project Anon Key.
   
   *You can find these in Supabase under **Project Settings > API**.*

3. **Deploy**: Vercel will automatically build and deploy the project.

---

## 💰 Step 3: Monetization & Payments

Currently, the project uses a **simulated payment system** integrated with Supabase:
- When a user "buys" a pack, the transaction is recorded in the `purchases` table.
- Credits are automatically added to the user's profile in the `profiles` table.
- **To implement real payments (Stripe/PayPal)**: You will need to replace the logic in `components/PaymentPage.tsx` with your preferred payment gateway API.

---

## 📊 Step 4: Admin Dashboard

- **Access**: Double-click the logo on the landing page to enter the hidden admin panel.
- **Security**: The admin panel currently displays data fetched from Supabase. Ensure you manage your Supabase access tokens securely.

---

## 📂 Project Structure Refresher

- `/app`: Next.js App Router (Main logic in `AppClient.tsx`).
- `/components`: UI elements (Modals, Pages, PWA components).
- `/services`: Business logic (Supabase client, Sudoku generation).
- `supabase.sql`: Database schema and RLS policies.

---

**Congratulations!** Your project is ready for users. For further customization (colors, app name), use the Admin Dashboard within the app.
