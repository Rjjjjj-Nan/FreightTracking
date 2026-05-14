# Freight & Logistics Management System

## Overview
A Flask and MySQL freight logistics system with role-based access for admins, couriers, and customers. The app supports parcel tracking, courier management, dashboard analytics, and parcel condition history.

## Main Features

### Core Features
- **Admin Dashboard** - Dashboard analytics, manage parcels and couriers
- **Courier Dashboard** - View assigned deliveries and update parcel conditions
- **Customer Portal** - Registration, login, book parcels, track shipments
- **Parcel Management** - Full CRUD operations with courier assignment
- **Analytics** - Charts for parcel status, trends, and courier performance
- **Audit Logging** - Track all parcel conditions and system actions
- **Security** - bcrypt password hashing and session-based authentication

### Email Feature
- **Booking Confirmations** - Automatic confirmation emails after parcel booking
- **Email Details** - Tracking number, sender/receiver info, cost breakdown
- **Configurable** - Supports Gmail, Outlook, Yahoo, SendGrid, AWS SES

### Courier Management System
- **Courier Ratings & Reviews** - Display 1-5 star ratings on admin dashboard
- **Courier Performance Reports** - Leaderboard with delivery rates and metrics
- **Ratings & Reviews Report** - Detailed customer feedback with filtering and sorting
- **Courier Information Management** - Edit courier details and contact info
- **Courier Status Control** - Activate/deactivate couriers with preserved data
- **Detailed Courier Profiles** - View complete statistics, recent parcels, and reviews
- **Real-time Search** - Filter couriers by name, email, or phone

## Tech Stack
- Backend: Flask, Python
- Database: MySQL
- Frontend: HTML, CSS, Bootstrap, Chart.js
- Authentication: bcrypt
- Environment management: python-dotenv

## Requirements
- Python 3.8 or newer
- MySQL 8.0 or newer
- Windows PowerShell or Command Prompt

## How To Run

### 1. Open the project folder
Open a terminal in this directory:
```bash
cd freight-logistics
```

### 2. Create and activate a virtual environment
On Windows PowerShell:
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

If PowerShell blocks script execution, allow it for the current session:
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Create the database
Import the schema into MySQL:
```bash
mysql -u root -p < database_schema.sql
```

You can also run the SQL in MySQL Workbench or another MySQL client using [database_schema.sql](database_schema.sql).

### 5. Configure the environment
Create or edit the `.env` file in the project root:
```env
SECRET_KEY=your-secret-key
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=yourpassword
DB_NAME=freight_logistics
```

### 6. Start the app
Run the Flask app directly:
```bash
python app.py
```

Open:
```text
http://127.0.0.1:5000
```

## Default Login Accounts

| Role | Username | Password |
| --- | --- | --- |
| Admin | admin | admin123 |
| Courier | courier1 | courier123 |

## What The App Does
Admins can review dashboard analytics, manage parcels, assign couriers, and monitor delivery performance. Couriers can see assigned parcels, open parcel details, and update parcel conditions. Customers can register, log in, and use the customer-facing parcel pages.

## Database Notes
The project uses MySQL tables for users, parcels, parcel assignments, parcel conditions, and audit logs. Connection settings are loaded from `.env` when `app.py` starts.

## Project Files
The main runtime files are `app.py`, `database_schema.sql`, `requirements.txt`, `templates/`, and `static/`. A zipped backup copy is also present in the repository as `freight-logistics.zip`.

## Important Notes
- The app is started with `python app.py`; no separate runtime script is required.
- Helper setup scripts were removed from the working tree, so the README now reflects the current app-first workflow.
- If MySQL cannot connect, verify the `.env` values and confirm the database exists before starting the app.

## GitHub Deployment (Render)

This repository now includes a GitHub Actions workflow at `.github/workflows/main.yml` that:
- Runs CI checks on every push to `main`
- Triggers deployment to Render after CI passes

### 1. Create a Render Web Service
Use this repository as the source in Render and configure:
- Build Command: `pip install -r requirements.txt`
- Start Command: `gunicorn app:app --bind 0.0.0.0:$PORT`

### 2. Add Render Environment Variables
In Render dashboard, add your runtime variables:
- `SECRET_KEY`
- `DB_HOST`
- `DB_USER`
- `DB_PASSWORD`
- `DB_NAME`
- `MAIL_SERVER`
- `MAIL_PORT`
- `MAIL_USE_TLS`
- `MAIL_USERNAME`
- `MAIL_PASSWORD`
- `MAIL_DEFAULT_SENDER`

### 3. Add GitHub Secret
In GitHub repository settings, add:
- Secret Name: `RENDER_DEPLOY_HOOK_URL`
- Value: Render Deploy Hook URL from your Render service

After this is set, each push to `main` will automatically trigger a fresh deployment.

## Shipping Options & Pricing

The system provides four shipping methods with transparent pricing:

### Shipping Methods

| Shipping Method | Base Cost | Weight Surcharge | Use Case |
|---|---|---|---|
| **Standard** | ₱100 | ₱10/kg | Regular deliveries, flexible timeline |
| **Express** | ₱200 | ₱10/kg | Time-sensitive deliveries, 1-2 day delivery |
| **Super** | ₱300 | ₱10/kg | Urgent deliveries, priority handling |
| **International** | ₱500 | ₱10/kg | Cross-border shipments, customs handling |

### Pricing Calculation
- **Total Cost** = Base Cost + (Weight in kg × ₱10)
- Example: 3kg Standard = ₱100 + (3 × ₱10) = ₱130

### How to Book
1. Log in as a customer
2. Go to "Book a Parcel"
3. Fill in sender/receiver details
4. Select shipping method based on your needs
5. Enter parcel weight (in kg)
6. Cost is calculated automatically
7. Confirm and receive booking confirmation email with tracking number

## Testing Checklist

- [ ] Database created and seeded with 2000 parcels
- [ ] Admin login works (admin/admin123)
- [ ] Courier login works (courier1/admin123)
- [ ] Landing page displays with marketing content
- [ ] Admin dashboard loads with all charts
- [ ] Can view and filter parcels
- [ ] Can edit parcel details
- [ ] Can assign parcels to couriers
- [ ] Courier can view assigned parcels
- [ ] Courier can update parcel condition
- [ ] Condition history displays correctly
- [ ] Auto-cancellation scheduler works
- [ ] Analytics update in real-time

## Troubleshooting

### Database Connection Error
```
Error connecting to MySQL
```
**Solution**: Check `.env` file for correct credentials and ensure MySQL is running.

### Port Already in Use
```
Address already in use
```
**Solution**: Change port in `app.py` (last line) or kill the process using port 5000.

### Module Not Found
```
ModuleNotFoundError: No module named 'flask'
```
**Solution**: Ensure virtual environment is activated and dependencies installed.

### Seed Script Fails
```
ERROR: No couriers found
```
**Solution**: Ensure database schema is imported before running seed script.

## Email Feature Configuration

### Overview
The application sends automatic booking confirmation emails when users book parcels. Each email includes the tracking number, parcel details, cost breakdown, and tracking link.

### Setup Steps

#### 1. Configure .env File
Add these email configuration variables to your `.env` file:

```env
# Email Configuration (Gmail SMTP)
MAIL_SERVER=smtp.gmail.com
MAIL_PORT=587
MAIL_USE_TLS=True
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-specific-password
MAIL_DEFAULT_SENDER=noreply@freight-logistics.com
```

#### 2. Gmail Setup (if using Gmail)
1. Go to [Google Account Security](https://myaccount.google.com/security)
2. Enable "2-Step Verification"
3. Generate "App Password" for Mail
4. Copy the 16-character app password
5. Paste into `MAIL_PASSWORD` in `.env`

#### 3. Alternative Email Providers
- **Outlook/Hotmail**: `smtp-mail.outlook.com`, port `587`
- **Yahoo Mail**: `smtp.mail.yahoo.com`, port `587` (requires app password)
- **SendGrid**: Use SendGrid SMTP credentials
- **AWS SES**: Use your AWS SES SMTP credentials

#### 4. Test Email Feature
1. Log in as a customer
2. Navigate to "Book a Parcel"
3. Fill in all parcel details and submit
4. Check your email inbox (also check spam folder)
5. You should receive a confirmation email with tracking number and details

### Email Troubleshooting

**Email Not Sending:**
- Verify MAIL_USERNAME and MAIL_PASSWORD in .env
- For Gmail, ensure you used App Password (not regular password)
- Check Flask console for error messages
- Ensure port 587 is not blocked by firewall

**Email Goes to Spam:**
- Add sender address to your contacts
- Mark email as "Not Spam" in your email client

## Courier Management System

### Admin Dashboard Enhancements
The admin dashboard now displays:
- **Quick Action Buttons** - "Manage Couriers", "Performance", "Ratings"
- **Courier Performance Table** - Shows ratings, delivery rates, and revenue
- **Key Metrics** - View courier names, contact info, parcel counts, and star ratings

### Courier Management Page (`/admin/couriers`)

**Access**: Admin Dashboard → "Manage Couriers" button

**Features:**
- View all couriers with active/inactive status
- Search couriers by name, email, or phone
- Quick statistics cards:
  - Total couriers count
  - Active couriers count
  - Inactive couriers count
- View, Edit, Deactivate/Reactivate actions

### Courier Details Page

**Access**: Courier Management → Click eye icon (View)

**View:**
- Complete courier profile with member since date
- Performance metrics:
  - Total parcels assigned
  - Successfully delivered count
  - In transit and pending counts
  - Delivery rate percentage
- Customer satisfaction:
  - Average star rating (1-5)
  - Total customer reviews
  - Average delivery time in days
  - Total revenue generated
- Last 20 parcels assigned to courier
- Last 10 customer reviews with ratings

### Edit Courier Information

**Access**: Courier Detail → Edit button

**Edit Fields:**
- Full name
- Email address
- Phone number

### Deactivate/Reactivate Couriers

**Access**: Courier Management or Detail page → Lock/Unlock icons

**Why Deactivate:**
- Remove underperforming couriers temporarily
- Suspend without deleting courier data
- Preserve all historical information
- Reactivate anytime to restore access

**Effect of Deactivation:**
- Courier cannot accept new parcel assignments
- Cannot see assigned parcels
- All data preserved for future reactivation

### Performance Report (`/admin/reports/courier-performance`)

**Access**: Admin Dashboard → "Performance" button

**Displays:**
- Courier performance leaderboard sorted by rating and delivery rate
- Quick statistics:
  - Total active couriers
  - Average delivery rate across all couriers
  - Average customer rating
  - Total system revenue
- Detailed performance table with:
  - Rank/position
  - Courier name and status
  - Parcels assigned and delivered
  - Delivery rate percentage (with progress bar)
  - Average star rating and review count
  - Average delivery time
  - Total revenue generated
- Top performers sections:
  - Top 3 highest rated couriers
  - Top 3 highest delivery rate couriers
  - Top 3 revenue earning couriers

### Ratings & Reviews Report (`/admin/reports/courier-ratings`)

**Access**: Admin Dashboard → "Ratings" button

**Filter Options:**
- By specific courier
- By minimum star rating (1-5 stars)

**Sort Options:**
- Newest first (default)
- Oldest first
- Highest rating
- Lowest rating

**View:**
- Full customer reviews with ratings
- Courier name and customer name
- Star rating (visual display)
- Review date and time
- Related parcel tracking number
- Sender and receiver information

**Summary Statistics:**
- Total ratings received
- Average rating across results
- Count of 5-star reviews
- Count of 1-2 star reviews

## Future Enhancements

- Mobile app (React Native/Flutter)
- Real-time notifications (Firebase Cloud Messaging)
- GPS tracking with map integration
- Email notifications for status changes
- Advanced route optimization
- Payment gateway integration
- Advanced reporting and exports
- Machine learning for delivery time predictions

## Support

For issues or questions, check the logs in the terminal or enable Flask debug mode in `.env`:
```
FLASK_DEBUG=True
```

## License

This project is for educational purposes.

---

**Version**: 1.0  
**Last Updated**: 2026 
**Status**: Production Ready
