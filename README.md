Laravel Carbon Date/Time Management Project
A comprehensive Laravel 12 project demonstrating practical usage of the Carbon library for date and time manipulation across controllers, blade views, and models.

✨ Features
🔧 Carbon Operations Demonstrated
✅ Basic Date Operations: Current date, yesterday, tomorrow, next week, last month

✅ Date Formatting: Multiple date format examples (Y-m-d, F j Y, m/d/Y, etc.)

✅ Time Zone Management: Working with different time zones (New York, London)

✅ Date Calculations: Age calculation, days between dates, months between dates

✅ Date Comparisons: Past/future checks, weekend detection, date comparisons

✅ Human Readable Formats: diffForHumans() for user-friendly date displays

✅ Date Manipulation: Add/subtract days, weeks, months, years

🏗️ Application Features
✅ CRUD Operations: Full Create, Read, Update, Delete for user profiles

✅ Date-based Scopes: Filter profiles by subscription status, birthdays

✅ Model Accessors: Automatic date formatting and calculations

✅ Form Validation: Date validation with Carbon constraints

✅ Responsive Design: Bootstrap 5 styling

✅ Sample Data: Seeder with realistic date scenarios

🚀 Prerequisites
PHP 8.1 or higher

Composer

MySQL 5.7+ or equivalent database

Laravel 12 requirements

📥 Installation
1. Clone the Repository
bash
git clone https://github.com/yourusername/laravel-carbon-project.git
cd laravel-carbon-project
2. Install Dependencies
bash
composer install
3. Configure Environment
bash
cp .env.example .env
php artisan key:generate
Edit .env file with your database credentials:

env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_carbon
DB_USERNAME=root
DB_PASSWORD=
4. Run Migrations and Seeders
bash
php artisan migrate
php artisan db:seed --class=UserProfileSeeder
5. Start Development Server
bash
php artisan serve
Visit: http://localhost:8000/profiles

📁 Project Structure
text
laravel-carbon-project/
├── app/
│   ├── Http/Controllers/
│   │   └── UserProfileController.php  # Main controller with Carbon examples
│   ├── Models/
│   │   └── UserProfile.php            # Model with Carbon accessors/scopes
│   └── Providers/
├── database/
│   ├── migrations/
│   │   └── create_user_profiles_table.php
│   └── seeders/
│       └── UserProfileSeeder.php
├── resources/
│   └── views/
│       ├── profiles/
│       │   ├── index.blade.php        # Main view with all Carbon examples
│       │   ├── create.blade.php       # Create profile form
│       │   └── calculations.blade.php # Detailed date calculations
│       └── welcome.blade.php
├── routes/
│   └── web.php                        # Route definitions
└── README.md
📅 Carbon Examples
Controller Examples (UserProfileController.php)
php
// Basic operations
$currentDate = Carbon::now()->format('m/d/Y');
$yesterday = Carbon::yesterday()->format('m/d/Y');
$tomorrow = Carbon::tomorrow()->format('m/d/Y');

// Time zones
$newYorkTime = Carbon::now('America/New_York')->format('Y-m-d H:i:s');

// Calculations
$age = Carbon::parse('1990-05-15')->age;
$daysDifference = $startDate->diffInDays($endDate);

// Comparisons
$isPast = Carbon::parse('2023-01-01')->isPast();
$isWeekend = Carbon::now()->isWeekend();
Blade View Examples
blade
{{-- Using now() helper --}}
{{ now()->format('F j, Y') }}

{{-- Using Carbon facade --}}
{{ \Carbon\Carbon::parse($profile->created_at)->diffForHumans() }}

{{-- Conditional formatting --}}
@php
    $expiry = \Carbon\Carbon::parse($profile->subscription_expiry);
    $class = $expiry->isPast() ? 'text-danger' : 'text-success';
@endphp
<span class="{{ $class }}">{{ $expiry->format('M d, Y') }}</span>
Model Examples (UserProfile.php)
php
// Accessors
public function getAgeAttribute()
{
    return Carbon::parse($this->birth_date)->age;
}

public function getSubscriptionStatusAttribute()
{
    $expiry = Carbon::parse($this->subscription_expiry);
    return $expiry->isPast() ? 'Expired' : 'Active';
}

// Scopes
public function scopeActiveSubscription($query)
{
    return $query->where('subscription_expiry', '>', now());
}

public function scopeBirthdaysThisMonth($query)
{
    return $query->whereMonth('birth_date', now()->month);
}
📖 Usage Guide
1. View All Carbon Examples
Navigate to /profiles to see:

Basic date operations

Time zone examples

Date calculations

User profiles with date-based formatting

2. Create New Profile
Visit /profiles/create to add a new user profile with:

Birth date (validated to be in past)

Subscription expiry date (defaults to 30 days from today)

3. View Date Calculations
Click "View Calculations" on any profile to see:

Age calculation

Days until next birthday

Subscription status and expiry countdown

Time since profile creation

4. Sample Data Features
The seeder creates profiles with:

✅ Active subscriptions

⚠️ Expired subscriptions

🎂 Upcoming birthdays

📅 Various creation dates

🔗 API Endpoints
Method	Endpoint	Description
GET	/profiles	View all profiles with Carbon examples
GET	/profiles/create	Create new profile form
POST	/profiles	Store new profile
GET	/profiles/{id}/calculations	View detailed date calculations
🗄️ Database Schema
user_profiles Table
sql
CREATE TABLE user_profiles (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    birth_date DATE NOT NULL,
    subscription_expiry DATE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
Sample Data
php
[
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'birth_date' => '1990-05-15',  // 34 years old
    'subscription_expiry' => Carbon::now()->addDays(15), // Active
],
[
    'name' => 'Bob Wilson',
    'email' => 'bob@example.com',
    'birth_date' => '1978-08-22',  // 45 years old
    'subscription_expiry' => Carbon::now()->subDays(10), // Expired
]
