# 🐦 Tweet Center - Social Media App

A modern, interactive Django-based social media application where users can create, edit, delete, and search tweets with image attachments. Built with a polished dark theme and responsive design.

---

## 📋 Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [App Functions & Endpoints](#app-functions--endpoints)
- [Database Models](#database-models)
- [Authentication](#authentication)
- [Design & UI](#design--ui)
- [Technologies Used](#technologies-used)

---

## ✨ Features

### User Features
- **User Authentication**: Registration, login, and logout functionality
- **Tweet Management**: Create, read, update, and delete tweets
- **Image Support**: Attach photos to tweets (JPEG, PNG, etc.)
- **Search Functionality**: Search tweets by content or username
- **User-Specific Actions**: Edit/delete only your own tweets
- **Responsive Design**: Works seamlessly on desktop and mobile devices
- **Modern UI**: Dark theme with smooth animations and interactive elements

### Technical Features
- **Django 6.0.4**: Robust web framework
- **SQLite Database**: Lightweight relational database
- **Bootstrap 5.3.8**: Responsive CSS framework
- **Form Validation**: Built-in Django form handling
- **Access Control**: Login-required decorators for protected views
- **Media Management**: Organized file uploads with proper routing

---

## 📁 Project Structure

```
tweetCenter/
├── db.sqlite3                 # SQLite database
├── manage.py                  # Django management script
├── media/
│   └── tweet_photos/          # Uploaded tweet images
├── static/
│   └── style.css              # Custom styling (dark theme)
├── templates/
│   ├── layout.html            # Base template with navbar
│   ├── index.html             # Homepage
│   └── registration/
│       ├── login.html         # Login page
│       ├── register.html      # Registration page
│       └── logged_out.html    # Logout confirmation
├── tweet/
│   ├── models.py              # Tweet model definition
│   ├── views.py               # View functions
│   ├── urls.py                # URL routing
│   ├── forms.py               # Tweet & User forms
│   ├── admin.py               # Django admin config
│   ├── apps.py                # App configuration
│   ├── migrations/            # Database migrations
│   └── templates/
│       ├── tweet_list.html    # Tweet feed
│       ├── tweet_form.html    # Create/edit tweet
│       └── tweet_confirm_delete.html # Delete confirmation
└── tweetCenter/
    ├── settings.py            # Project settings
    ├── urls.py                # Main URL routing
    ├── asgi.py                # ASGI config
    └── wsgi.py                # WSGI config
```

---

## 🚀 Installation

### Prerequisites
- Python 3.8+
- pip (Python package manager)
- Virtual environment (recommended)

### Steps

1. **Clone the repository**
```bash
git clone https://github.com/ergauravk/Tweet-App.git
cd Tweet-App/tweetCenter
```

2. **Create and activate virtual environment**
```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS/Linux
python3 -m venv .venv
source .venv/bin/activate
```

3. **Install dependencies**
```bash
pip install -r requirement.txt
```

4. **Apply database migrations**
```bash
python manage.py migrate
```

5. **Create a superuser (optional, for admin panel)**
```bash
python manage.py createsuperuser
```

6. **Run the development server**
```bash
python manage.py runserver
```

7. **Access the app**
- Homepage: `http://127.0.0.1:8000/`
- Admin Panel: `http://127.0.0.1:8000/admin/`

## 🌐 Render Deployment

This project includes a `render.yaml` file for Render web-service deployment.

### What it configures
- Docker-based deployment using `tweetCenter/Dockerfile`
- Persistent storage for SQLite and uploaded media
- Production environment variables for Django, static files, and uploads

### Notes
- The app serves static files through WhiteNoise.
- Uploaded images are stored on the persistent disk mounted at `/var/data`.
- If you set a custom domain on Render, update `DJANGO_ALLOWED_HOSTS` accordingly.

---

## 📖 Usage

### 1. **Register a New Account**
- Navigate to `/accounts/register/`
- Fill in username, email, and password (twice)
- Click "Register"
- Automatically logged in and redirected to feed

### 2. **Login**
- Go to `/accounts/login/`
- Enter credentials
- Redirected to tweet feed

### 3. **View Tweet Feed**
- Homepage shows all tweets in chronological order
- Each tweet displays author, text, and optional photo
- Shows upload date and edit/delete buttons (for own tweets)

### 4. **Create a Tweet**
- Click "New Tweet" button in navbar
- Enter tweet text (max 280 characters)
- Optionally attach an image
- Click "Save Tweet"

### 5. **Edit a Tweet**
- Go to tweet feed
- Click "Edit" on your own tweet
- Modify text or image
- Click "Save Tweet"

### 6. **Delete a Tweet**
- Go to tweet feed
- Click "Delete" on your own tweet
- Confirm deletion on the warning page

### 7. **Search Tweets**
- Use search bar in navbar
- Search by tweet content or username
- Results appear in feed

### 8. **Logout**
- Click "Logout" in navbar
- Confirmation page shown
- Redirected to login page

---

## 🔧 App Functions & Endpoints

### View Functions (tweet/views.py)

#### 1. **index(request)**
- **URL**: `/`
- **Method**: GET
- **Purpose**: Display homepage with app introduction
- **Template**: `index.html`
- **Authentication**: Not required

#### 2. **tweet_list(request)**
- **URL**: `/tweet/`
- **Method**: GET, POST (search)
- **Purpose**: Display all tweets, support search functionality
- **Template**: `tweet_list.html`
- **Query Parameters**: `?q=search_term`
- **Features**:
  - Filters tweets by text content or username
  - Orders by creation date (newest first)
  - Shows search results with "Clear" option
- **Authentication**: Not required

#### 3. **tweet_create(request)**
- **URL**: `/tweet/create/`
- **Method**: GET, POST
- **Purpose**: Create new tweet with optional image
- **Template**: `tweet_form.html`
- **Authentication**: **Required** (login_required)
- **Form**: TweetForm (text, photo)
- **Actions**:
  - GET: Display empty form
  - POST: Save tweet and redirect to feed

#### 4. **tweet_edit(request, tweet_id)**
- **URL**: `/tweet/<tweet_id>/edit/`
- **Method**: GET, POST
- **Purpose**: Edit existing tweet
- **Template**: `tweet_form.html`
- **Authentication**: **Required**
- **Validation**: User can only edit their own tweets
- **Actions**:
  - GET: Display pre-filled form with existing data
  - POST: Update tweet and redirect to feed

#### 5. **tweet_delete(request, tweet_id)**
- **URL**: `/tweet/<tweet_id>/delete/`
- **Method**: GET, POST
- **Purpose**: Delete tweet with confirmation
- **Template**: `tweet_confirm_delete.html`
- **Authentication**: **Required**
- **Validation**: User can only delete their own tweets
- **Actions**:
  - GET: Show delete confirmation page
  - POST: Remove tweet from database and redirect

#### 6. **register(request)**
- **URL**: `/tweet/register/`
- **Method**: GET, POST
- **Purpose**: User registration (create new account)
- **Template**: `registration/register.html`
- **Authentication**: Not required
- **Form**: UserRegistrationForm (username, email, password)
- **Actions**:
  - GET: Display registration form
  - POST: Create user, auto-login, redirect to feed

### Built-in Django Views (django.contrib.auth)

#### 7. **LoginView**
- **URL**: `/accounts/login/` (also `/tweet/login/`)
- **Method**: GET, POST
- **Template**: `registration/login.html`
- **Purpose**: User login

#### 8. **LogoutView**
- **URL**: `/tweet/accounts/logout/`
- **Method**: POST
- **Template**: `registration/logged_out.html`
- **Purpose**: User logout with confirmation

---

## 💾 Database Models

### Tweet Model (tweet/models.py)

```python
class Tweet(models.Model):
    user = ForeignKey(User, on_delete=CASCADE)  # Author of tweet
    text = TextField(max_length=280)            # Tweet content
    photo = ImageField(upload_to="tweet_photos/", blank=True, null=True)
    created_at = DateTimeField(auto_now_add=True)  # Creation timestamp
    updated_at = DateTimeField(auto_now=True)      # Last edit timestamp
```

**Fields**:
- `user`: Linked to Django User model (one-to-many)
- `text`: Tweet content (required, max 280 chars)
- `photo`: Optional image file
- `created_at`: Auto-set on creation
- `updated_at`: Auto-updated on save

**Methods**:
- `__str__()`: Returns "username: tweet_text_preview"

---

## 🔐 Authentication

### User Management
- Uses Django's built-in `User` model from `django.contrib.auth`
- Registration creates new user with hashed password
- Login validates credentials
- `@login_required` decorator protects tweet creation/editing/deletion
- Users can only modify their own tweets

### Permission Model
- **Public**: Homepage, login, register, search, view feed (read-only)
- **Authenticated Only**: Create tweets, edit/delete own tweets

### Password Security
- Hashed using Django's default PBKDF2 algorithm
- Passwords never stored in plain text
- Password validation rules enforced during registration

---

## 🎨 Design & UI

### Color Palette (Dark Theme)
- **Background**: Deep navy gradients (`#0f172a` to `#1a202c`)
- **Text**: White/Light gray (`#ffffff`, `#cbd5e1`)
- **Primary Accent**: Blue (`#3b82f6`)
- **Secondary Accent**: Green (`#10b981`)
- **Warning**: Orange (`#f97316`)

### Key Design Features
- **Navbar**: Glass-morphism effect with gradient background
- **Buttons**: Gradient backgrounds with shimmer animation on hover
- **Cards**: Semi-transparent surfaces with backdrop blur
- **Animations**: Smooth transitions, underline effects, elevation changes
- **Responsive**: Mobile-first design, adapts to all screen sizes
- **Typography**: Clean, modern font with proper hierarchy

### Interactive Elements
- Hover effects on all buttons and links
- Smooth animations (0.2-0.3s duration)
- Focus states for accessibility
- Animated brand logo with pulsing glow
- Loading states and visual feedback

---

## 🛠️ Technologies Used

### Backend
- **Django 6.0.4**: Web framework
- **Python 3.8+**: Programming language
- **SQLite**: Database

### Frontend
- **HTML5**: Markup
- **CSS3**: Custom styling with animations
- **Bootstrap 5.3.8**: CSS framework
- **JavaScript**: Bootstrap bundle for interactivity

### Tools & Libraries
- **Pillow**: Image processing (PIL)
- **Django ORM**: Database queries
- **Django Forms**: Form rendering and validation
- **Django Auth**: User authentication

---

## 📝 Forms

### TweetForm
```python
class TweetForm(ModelForm):
    class Meta:
        model = Tweet
        fields = ['text', 'photo']
```

### UserRegistrationForm
```python
class UserRegistrationForm(UserCreationForm):
    email = EmailField(required=True)
    class Meta:
        model = User
        fields = ('username', 'email', 'password1', 'password2')
```

---

## 🔄 Workflow Example

```
User visits homepage
        ↓
Clicks "Register" → Creates account
        ↓
Logs in with credentials
        ↓
Redirected to tweet feed
        ↓
Clicks "New Tweet" → Creates post with optional image
        ↓
Post appears in feed
        ↓
Can edit or delete own posts
        ↓
Can search tweets by content/user
        ↓
Clicks "Logout" → Logged out with confirmation
```

---

## 📱 Responsive Breakpoints

- **Desktop**: Full navbar, grid layout (4-5 columns)
- **Tablet**: Adjusted grid (2-3 columns), responsive navbar
- **Mobile**: Single column, hamburger menu, optimized buttons

---

## 🐛 Troubleshooting

### 404 Media Files Not Found
**Solution**: Ensure media serving is configured in `urls.py`:
```python
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### Images Not Uploading
**Solution**: Check media directory exists:
```bash
mkdir media/tweet_photos
```

### Login Redirect Issues
**Solution**: Verify `LOGIN_URL` and `LOGIN_REDIRECT_URL` in `settings.py`

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👨‍💻 Author

**Gaurav Kumar**  
GitHub: [ergauravk](https://github.com/ergauravk)

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

---

## 📞 Support

For issues and questions, please create an issue on the GitHub repository.

---

**Happy Tweeting! 🚀**
