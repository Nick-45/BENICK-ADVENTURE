Benick Adventure - Booking System Documentation
Overview
Benick Adventure is a comprehensive booking system for a Kenyan adventure tourism company, featuring client-facing booking interfaces and an administrative dashboard. The system is built with HTML, CSS, JavaScript, and Firebase for backend services.

System Components
1. Client-Facing Pages
index.html: Homepage with featured packages

about.html: Company information page

tours.html: Safari package listings

roadtrips.html: Road trip offerings

vacations.html: Beach holiday packages

booking.html: Booking form with dynamic pricing

contact.html: Contact information form

2. Admin Dashboard
admin.html: Secure dashboard for managing bookings

Firebase Realtime Database: Backend data storage

Authentication System: Secure admin login

Technical Stack
Frontend
HTML5

CSS3 (with custom styling)

JavaScript (ES6)

Firebase SDK (v9+)

Backend
Firebase Realtime Database

Firebase Authentication

Firebase Hosting

Third-Party Integrations
EmailJS (for email confirmations)

Google Fonts (Poppins)

Font Awesome (icons)

Installation & Setup
Prerequisites
Firebase project with Realtime Database enabled

Firebase Authentication enabled (Email/Password)

Firebase Hosting configured

EmailJS account (for booking confirmations)

Setup Instructions
Clone the repository

bash
git clone https://github.com/your-repo/benick-adventure.git
cd benick-adventure
Configure Firebase

Replace Firebase config in all HTML files:

javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  databaseURL: "YOUR_DATABASE_URL",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
Set up EmailJS

Replace EmailJS credentials in booking.html:

javascript
emailjs.init("YOUR_EMAILJS_USER_ID");
Deploy to Firebase Hosting

bash
firebase init
firebase deploy
Database Structure
Collections
bookings

bookingRef (string)

fullName (string)

email (string)

package (string)

travelDate (string)

numPeople (number)

totalAmount (number)

status (string: pending/confirmed/cancelled)

bookingDate (timestamp)

packages

name (string)

price (number)

description (string)

imageUrl (string)

adminUsers

uid (string)

email (string)

isAdmin (boolean)

Security Rules
See the complete security rules in the Firebase Rules section below.

Usage Guide
For Customers
Browse available packages

Select desired package

Fill out booking form

Receive email confirmation

Manage bookings through contact options

For Administrators
Log in with admin credentials

View all bookings in dashboard

Filter by status (pending/confirmed/cancelled)

Update booking status

View booking statistics

Firebase Rules
json
{
  "rules_version": "2",
  "rules": {
    "bookings": {
      ".read": "auth != null",
      ".write": "auth != null",
      "$bookingId": {
        ".validate": "newData.hasChildren(['bookingRef', 'fullName', 'email', 'package', 'status'])",
        "bookingRef": { ".validate": "newData.isString() && newData.val().length > 0" },
        "fullName": { ".validate": "newData.isString() && newData.val().length > 0" },
        "email": { ".validate": "newData.isString() && newData.val().matches(/^[^@]+@[^@]+\\.[^@]+$/)" },
        "status": { ".validate": "newData.isString() && ['pending', 'confirmed', 'cancelled'].indexOf(newData.val()) != -1" },
        "totalAmount": { ".validate": "newData.isNumber() && newData.val() > 0" },
        "numPeople": { ".validate": "newData.isNumber() && newData.val() > 0" }
      }
    },
    "adminUsers": {
      ".read": "auth != null && auth.token.email.matches(/@benickadventure\\.com$/)",
      ".write": "auth != null && auth.token.email.matches(/@benickadventure\\.com$/)"
    },
    "packages": {
      ".read": true,
      ".write": "auth != null && auth.token.email.matches(/@benickadventure\\.com$/)",
      "$packageId": {
        ".validate": "newData.hasChildren(['name', 'price', 'description'])",
        "name": { ".validate": "newData.isString() && newData.val().length > 0" },
        "price": { ".validate": "newData.isNumber() && newData.val() > 0" }
      }
    },
    "users": {
      "$userId": {
        ".read": "auth != null && auth.uid === $userId",
        ".write": "auth != null && auth.uid === $userId",
        ".validate": "newData.hasChildren(['email', 'name'])",
        "isAdmin": {
          ".write": "auth != null && root.child('adminUsers').child(auth.uid).exists()"
        }
      }
    }
  }
}
Testing
Unit Tests
Form validation

Pricing calculations

Authentication flows

Integration Tests
Booking submission

Database writes

Email notifications

Troubleshooting
Common Issues
Firebase not initializing

Verify API keys

Check internet connection

Ensure Firebase services are enabled

Email not sending

Check EmailJS configuration

Verify email template exists

Check spam folder

Authentication failures

Verify admin email format

Check Firebase Authentication logs

Ensure password meets complexity requirements

Maintenance
Regular Tasks
Backup database weekly

Review security rules quarterly

Update package information as needed

Monitor booking trends

License
Proprietary software - © Benick Adventure 2023

Contact
For technical support: techsupport@benickadventure.com
For booking inquiries: bookings@benickadventure.com
