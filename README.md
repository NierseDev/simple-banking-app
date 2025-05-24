# Simple Banking App

A user-friendly and responsive Flask-based banking application designed for deployment on PythonAnywhere. This application allows users to create accounts, perform simulated money transfers between accounts, view transaction history, and securely manage their credentials.

## Features

- **User Authentication**
  - Secure login with username/password
  - Registration of new users
  - Password recovery mechanism (email-based)

- **Account Management**
  - Display of account balance
  - View recent transaction history (last 10 transactions)

- **Fund Transfer**
  - Transfer money to other registered users
  - Confirmation screen before completing transfers
  - Transaction history updated after transfers

- **User Role Management**
  - Regular user accounts
  - Admin users with account approval capabilities
  - Manager users who can manage admin accounts

- **Location Data Integration**
  - Philippine Standard Geographic Code (PSGC) API integration
  - Hierarchical location data selection (Region, Province, City, Barangay)
  - Form fields pre-populated with location data

- **Admin Features**
  - User account approval workflow
  - Account activation/deactivation
  - Deposit funds to user accounts
  - Create new accounts
  - Edit user information

- **Manager Features**
  - Create and manage admin accounts
  - View admin transaction logs
  - Monitor all system transfers

- **Security**
  - Password hashing with bcrypt for secure storage
  - Secure session management
  - Token-based password reset
  - API rate limiting to prevent abuse
  - CSRF protection for all forms

## Getting Started

### Prerequisites
- Python 3.7+
- pip (Python package manager)
- MySQL Server 5.7+ or MariaDB 10.2+

### Database Setup

1. Install MySQL Server or MariaDB if you haven't already:
   ```
   # For Ubuntu/Debian
   sudo apt update
   sudo apt install mysql-server
   
   # For macOS with Homebrew
   brew install mysql
   
   # For Windows
   # Download and install from the official website
   ```

2. Create a database user and set privileges:
   ```
   mysql -u root -p
   
   # In MySQL prompt
   CREATE USER 'bankapp'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON *.* TO 'bankapp'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

3. Update the `.env` file with your MySQL credentials:
   ```
   DATABASE_URL=mysql+pymysql://bankapp:your_password@localhost/simple_banking
   MYSQL_USER=bankapp
   MYSQL_PASSWORD=your_password
   MYSQL_HOST=localhost
   MYSQL_PORT=3306
   MYSQL_DATABASE=simple_banking
   ```

4. Initialize the database:
   ```
   python init_db.py
   ```

### Installation

1. Clone the repository:
   ```
   git clone https://github.com/lanlanjr/simple-banking-app.git
   cd simple-banking-app
   
   # Set up your own repository
   # First, create a new repository named 'simple-banking-app-v2' on GitHub
   
   # Then configure your local repository
   git remote remove origin
   git remote add origin https://github.com/yourusername/simple-banking-app-v2.git
   git branch -M main
   git push -u origin main
   
   # Note: Replace 'yourusername' with your actual GitHub username
   ```

2. Install the required packages:
   ```
   pip install -r requirements.txt
   ```

3. Run the application:
   ```
   python app.py
   ```

4. Access the application at `http://localhost:5000`

## Deploying to PythonAnywhere

1. Create a PythonAnywhere account at [www.pythonanywhere.com](https://www.pythonanywhere.com)

2. Upload your code using Git:
   ```
   git clone https://github.com/yourusername/simple-banking-app-v2.git
   ```

3. Install requirements:
   ```
   cd simple-banking-app-v2
   pip install -r requirements.txt
   ```

4. Set up MySQL database on PythonAnywhere:
   - Go to the Databases tab in your PythonAnywhere dashboard
   - Create a new MySQL database
   - Note the database name, username, and password
   - Update your .env file with these credentials

5. Initialize your database on PythonAnywhere:
   ```
   python init_db.py
   ```

6. Configure a new web app via the PythonAnywhere dashboard:
   - Select "Manual configuration"
   - Choose Python 3.8
   - Set source code directory to `/home/yourusername/simple-banking-app-v2`
   - Set working directory to `/home/yourusername/simple-banking-app-v2`
   - Set WSGI configuration file to point to your Flask app

7. Add environment variables in the PythonAnywhere dashboard for security

## Usage

### Registration
- Navigate to the registration page
- Enter username, email, and password
- Confirm your password
- Submit the form to create your account (pending admin approval)

### Login
- Enter your username and password
- Click "Sign In"

### Account Overview
- View your current balance
- See your recent transaction history

### Transfer Funds
- Navigate to the Transfer page
- Enter recipient's username or account number
- Enter the amount to transfer
- Confirm the transfer details on the confirmation screen
- Complete the transfer

### Password Reset
- Click "Forgot your password?" on the login page
- Enter your registered email address
- Follow the link in the email (simulated in this demo)
- Create a new password

### Admin Features
- Approve new user registrations
- Activate/deactivate user accounts
- Create new user accounts
- Make over-the-counter deposits to user accounts
- Edit user details including location information

### Manager Features
- Create new admin accounts
- Toggle admin status for users
- View all user transactions
- Monitor and audit admin activities

## User Roles

The system supports three types of user roles:

1. **Regular Users** - Can manage their own account, make transfers, and view their transaction history.

2. **Admin Users** - Have all regular user privileges plus:
   - Approve/reject new user registrations
   - Activate/deactivate user accounts
   - Create new user accounts
   - Make deposits to user accounts
   - Edit user information

3. **Manager Users** - Have all admin privileges plus:
   - Create and manage admin accounts
   - View admin transaction logs
   - Monitor all system transfers
   - System-wide oversight capabilities

## Address Management with PSGC API

The application integrates with the Philippine Standard Geographic Code (PSGC) API to provide standardized address selection for user profiles. The address system follows the Philippine geographical hierarchy:

- Region
- Province
- City/Municipality
- Barangay

This integration ensures addresses are standardized and validates location data according to the Philippine geographical structure.

## Technologies Used

- **Backend**: Python, Flask
- **Database**: MySQL (with SQLAlchemy ORM)
- **Frontend**: HTML, CSS, Bootstrap 5
- **Authentication**: Flask-Login, Werkzeug, Flask-Bcrypt
- **Forms**: Flask-WTF, WTForms
- **Security**: Flask-Limiter for API rate limiting, CSRF protection
- **External API**: PSGC API for Philippine geographic data

## Rate Limiting

The application uses Flask-Limiter to implement API rate limiting, which protects against potential DoS attacks and abusive bot activity. The rate limits are configured as follows:

- **Login**: 10 attempts per minute
- **Registration**: 5 attempts per minute
- **Password Reset**: 5 attempts per hour
- **Money Transfer**: 20 attempts per hour
- **API Endpoints**: 30 requests per minute
- **Admin Dashboard**: 60 requests per hour
- **Admin Account Creation**: 20 accounts per hour
- **Admin Deposits**: 30 deposits per hour
- **Manager Dashboard**: 60 requests per hour
- **Admin Creation**: 10 admin accounts per hour

By default, the rate limiting data is stored in memory. For production use, it's recommended to use Redis as a storage backend for persistence across application restarts. To enable Redis storage:

1. Install Redis server on your system
2. Update the `.env` file with your Redis URL:
   ```
   REDIS_URL=redis://localhost:6379/0
   ```

If Redis is not available, the application will automatically fall back to in-memory storage.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Security Assessment Documentation for Simple Banking App 

During the initial reviewing of the original simple banking app, the following vulnerabilities and weaknesses were identified:

1. Session Fixation Risk: Sessions were not explicitly regenerated upon login.

3. Partial Error Handling: Application lacked generic error handling for server-side issues, potentially leaking stack traces.

4. CSRF Protection: While Flask-WTF CSRF protection was included, not all endpoints were verified to be covered.

5. Clickjacking Risk: No explicit HTTP headers to prevent iframe embedding.

## Security Improvements Implemented 
1. Session Management Enhancements - Regenerated session ID upon successful login.

3. Comprehensive Error Handling - Added a global 500 error handler to suppress technical error messages and log server-side issues internally.

4. CSRF Protection Review - Ensured all POST forms include CSRF tokens via Flask-WTF.

5. Clickjacking Protection - Used Flask-Talisman to set X-Frame-Options: DENY header, preventing the site from being embedded.

## Testing the Security Improvements Implementations using DevTools
1. Session Management Enhancements
This regenerates session ID upon login 

Steps taken (Localhost):
i. In your browser, go to DevTools, then 'Network', then 'Cookies' before and after login.
ii. Observe that the ```session``` cookie changes after login.
iii. This confirms session ID regeneration.

Before Login:
Name: session
Value: ```eyJfZnJlc2giOmZhbHNlLCJjc3JmX3Rva2VuIjoiMzhjO```

After Login:
Name: session
Value: ```.eJwljkFqA0EMBP8y5xwkjaTR-jPLSCNhE0hg1z6F_N0bciq66EP9tL2OPO_t9jxe-dH2x2q35kwlgycsVxUAS7GBbkYwgY2UJkZGVyKGtOtTxuVBSaPX4tHnRUJeyhKdMtSxO23mNAZakGERiI5N2a0WJqUWF7CWI7Qr5HXm8V_TrxnnUfvz-zO__oSFFdJWFBwphJ1zsspMniJgLDhpxmq_b__FPbc.aDEvPg.6_J5qYCp9afEkKO4FgMiTt9yU2s```	


2. CSRF Protection Review
This ensure that all POST forms have CSRF tokens

Steps taken (Localhost):
i. Submit a credential on Login form.
ii. In your browser, go to DevTools, then 'Network', then 'Payload'.
iii. Under the 'Form Data', you'll see the 'csrf_token value'.
iv. This confirms that ```csrf_token``` field is present.

Form Data (Output):
csrf_token: ```IjM4YzhmMTI5ZjJjNGNlNTIxMzRlYTQ2NWFlNGE1NTA4NDUxYTJhY2Qi.aDEuiQ.y7q0qyAgUp-jeaWTm11A1SWjcZo```
username: hasalvador123
password: your_password
submit: Login

3. Clickjacking Protection
This prevents the website from being embedded by setting the X-Frame-Options to DENY header using Flask-Talisman. 

Steps Taken (Localhost):
i. In your browser, go to 'Network', then go to 'Headers'. 
ii. Under the 'Response Headers', tick 'Raw'  and you'll see that the X-Frame-Options is really DENIED.
iii. This confirms that the X-Frame-Options:DENY is implemented. 

Response Header (Output):
HTTP/1.1 302 FOUND
Server: ```Werkzeug/2.3.7 Python/3.12.5```
X-Frame-Options: ```DENY```
X-Content-Type-Options: ```nosniff```
Referrer-Policy: ```strict-origin-when-cross-origin```
Vary: ```Cookie```
Connection: ```Close```

4. Comprehensive Error Handling
This enables a global 500 error handler to suppress technical error messages and log-server-side issues internally. 

Steps Taken (Localhost):
i. Go to Flask app.py and add this below the app = create_app() line of code:
```@app.errorhandler(500)
def internal_server_error(e):
    print(f"500 Error: {e}")  
    return render_template('500.html'), 500```

#Global 500 error handler 
@app.route('/trigger-error')
def trigger_error():
    raise Exception("This is a test 500 error.")```

ii. Create a 500.html file inside the /templates directory and paste this sample:
```<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Server Error</title>
</head>
<body>
    <h1>Something went wrong (500)</h1>
    <p>Sorry, an unexpected error occurred. Please try again later.</p>
</body>
</html>``` 
iii. On the app.py, set app.run(debug=True) to app.run(debug=False). 
iv. Run the application and go to http://127.0.0.1:5000/trigger-error.
v. This verifies that the page displays a "Something went wrong (500)" message to the client.

Result:
```Something went wrong (500)
Sorry, an unexpected error occurred. Please try again later.```

