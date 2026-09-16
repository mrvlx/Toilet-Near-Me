# Toilet Near Me - Setup Guide

## 🚀 Quick Start

### 1. Supabase Setup

1. **Create a Supabase Project**
   - Go to [supabase.com](https://supabase.com)
   - Create a new project

2. **Run Database Setup**
   - Go to SQL Editor in Supabase Dashboard
   - Copy and paste the contents of `supabase-setup.sql`
   - Run the script

3. **Create Admin User**
   - Sign up via the app with email: `admin1@gmail.com`
   - Go to SQL Editor and run:
     ```sql
     UPDATE profiles SET role = 'admin' WHERE email = 'admin1@gmail.com';
     ```

4. **Setup Storage Bucket**
   - Go to Storage in Supabase Dashboard
   - Create a new bucket named: `toilet-images`
   - Make it public
   - Add these policies:
     ```sql
     -- Public read access
     CREATE POLICY "Public Access"
       ON storage.objects FOR SELECT
       USING (bucket_id = 'toilet-images');

     -- Authenticated upload
     CREATE POLICY "Authenticated Upload"
       ON storage.objects FOR INSERT
       WITH CHECK (
         bucket_id = 'toilet-images' AND
         auth.role() = 'authenticated'
       );
     ```

5. **Get Your Credentials**
   - Go to Settings → API
   - Copy your Project URL and anon/public key

6. **Update Config**
   - Open `js/config.js`
   - Replace `YOUR_SUPABASE_URL` and `YOUR_SUPABASE_ANON_KEY` with your credentials

### 2. Running the App

Simply open `index.html` in a browser, or use a local server:

```bash
# Using Python
python -m http.server 8000

# Using Node.js (npx)
npx serve

# Using PHP
php -S localhost:8000
```

Then visit `http://localhost:8000` in your browser.

## 📋 Features

### Guest Users
- ✅ View Surabaya map with approved toilets
- ✅ GPS location detection
- ✅ Search toilets
- ✅ Filter by facilities (Gratis, Air, Wastafel, Difabel, Wanita, Pria)
- ✅ View toilet details

### Registered Users
- ✅ Login/Register with email verification
- ✅ Report new toilets
- ✅ Upload multiple photos (max 2MB each)
- ✅ Select location using draggable map pin
- ✅ View own reports
- ✅ Cancel pending reports
- ✅ Edit profile (change password)

### Admin Users
- ✅ Access admin dashboard
- ✅ Review pending reports
- ✅ Approve reports (creates public toilet)
- ✅ Reject reports with mandatory reason
- ✅ View all reports by status

## 🎨 Design

Neo-Brutalist UI with:
- Bold black borders (3px)
- High contrast colors
- Offset shadows (4px 4px 0 #111111)
- Primary green: #087F5B
- Background: #F5F0E8

## 📁 File Structure

```
/workspace
├── index.html          # Main HTML file
├── css/
│   └── style.css       # Neo-brutalist styles
├── js/
│   ├── config.js       # Supabase configuration
│   └── app.js          # Main application logic
├── admin/              # (Future admin pages)
├── supabase-setup.sql  # Database setup script
└── README.md           # This file
```

## 🔐 Default Admin Credentials

- Email: `admin1@gmail.com`
- Password: `adminada4`

**Note:** You must first sign up with this email via the app, then update the role to 'admin' in the database.

## 🗺️ Map Features

- Leaflet.js with OpenStreetMap
- Centered on Surabaya (-7.2575, 112.7521)
- User location marker (blue)
- Toilet markers (green with 🚽 icon)
- Draggable pin for report location selection

## 📱 Responsive Design

- Mobile: Bottom sheet for toilet details
- Desktop: Side panel for toilet details
- Fully responsive layout

## 🔄 Real-time Updates

Reports are loaded in real-time from Supabase. When an admin approves a report:
1. Toilet is created in `toilets` table
2. Images are copied from `report_images` to `toilet_images`
3. Report status is updated to 'approved'
4. New toilet appears on the map immediately

## ⚠️ Important Notes

1. **Email Verification**: Users must verify their email before logging in
2. **Storage**: Make sure the `toilet-images` bucket is public
3. **RLS Policies**: Row Level Security is enabled for all tables
4. **GPS**: Browser permission required for location features
5. **Outside Surabaya**: Warning banner shown if user is outside Surabaya bounds

## 🐛 Troubleshooting

**Map not loading?**
- Check browser console for errors
- Ensure Leaflet CSS/JS are loading

**Can't login?**
- Verify email address
- Check Supabase Auth settings
- Ensure email confirmation is enabled

**Photos not uploading?**
- Check storage bucket exists and is public
- Verify RLS policies for storage
- Check file size (max 2MB)

**Toilets not showing?**
- Check if they have 'approved' status
- Verify RLS policies allow public read
- Check browser console for errors

## 📝 Next Steps

After setup:
1. Test user registration
2. Submit a test report
3. Login as admin and approve the report
4. Verify the toilet appears on the map
5. Test all filters and search

Enjoy building! 🚽✨
