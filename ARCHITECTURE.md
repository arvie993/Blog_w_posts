# Architecture Overview

This document describes the technical architecture of the Blog with Posts application.

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Client Browser                          │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Flask Application                            │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Route Handlers                            ││
│  │  ┌─────────┐ ┌─────────┐ ┌──────────┐ ┌────────┐ ┌────────┐ ││
│  │  │   /     │ │ /post/  │ │/new-post │ │ /edit/ │ │/delete/│ ││
│  │  └─────────┘ └─────────┘ └──────────┘ └────────┘ └────────┘ ││
│  └─────────────────────────────────────────────────────────────┘│
│                                │                                 │
│  ┌─────────────────────────────┼───────────────────────────────┐│
│  │              Flask Extensions                                ││
│  │  ┌───────────────┐  ┌───────────────┐  ┌──────────────────┐ ││
│  │  │ Flask-WTF     │  │ Flask-CKEditor│  │ Flask-Bootstrap5 │ ││
│  │  │ (Forms)       │  │ (Rich Text)   │  │ (UI Framework)   │ ││
│  │  └───────────────┘  └───────────────┘  └──────────────────┘ ││
│  └─────────────────────────────────────────────────────────────┘│
│                                │                                 │
│  ┌─────────────────────────────┼───────────────────────────────┐│
│  │              Flask-SQLAlchemy ORM                            ││
│  │         (Database Abstraction Layer)                         ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SQLite Database                               │
│                    (posts.db)                                    │
└─────────────────────────────────────────────────────────────────┘
```

## Components

### 1. Flask Application (`main.py`)

The main entry point of the application, responsible for:
- Configuring Flask and extensions
- Defining routes and request handlers
- Managing database connections

#### Configuration
```python
app = Flask(__name__)
app.config['SECRET_KEY'] = '...'  # CSRF protection
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///posts.db'
```

### 2. Database Layer

#### ORM Model: BlogPost

```python
class BlogPost(db.Model):
    id: Mapped[int]          # Primary key
    title: Mapped[str]       # Post title (unique)
    subtitle: Mapped[str]    # Post subtitle
    date: Mapped[str]        # Publication date
    body: Mapped[str]        # HTML content (from CKEditor)
    author: Mapped[str]      # Author name
    img_url: Mapped[str]     # Header image URL
```

#### Database Schema

| Column | Type | Constraints |
|--------|------|-------------|
| id | INTEGER | PRIMARY KEY, AUTOINCREMENT |
| title | VARCHAR(250) | UNIQUE, NOT NULL |
| subtitle | VARCHAR(250) | NOT NULL |
| date | VARCHAR(250) | NOT NULL |
| body | TEXT | NOT NULL |
| author | VARCHAR(250) | NOT NULL |
| img_url | VARCHAR(250) | NOT NULL |

### 3. Forms Layer

#### CreatePostForm (WTForm)

Used for both creating and editing posts:

| Field | Type | Validators |
|-------|------|------------|
| title | StringField | DataRequired |
| subtitle | StringField | DataRequired |
| author | StringField | DataRequired |
| img_url | StringField | DataRequired, URL |
| body | CKEditorField | DataRequired |
| submit | SubmitField | - |

### 4. Template Layer (Jinja2)

#### Template Hierarchy
```
templates/
├── header.html        # Base navigation (included in all pages)
├── footer.html        # Base footer (included in all pages)
├── index.html         # Home page - lists all posts
├── post.html          # Single post view
├── make-post.html     # Create/Edit form (conditional rendering)
├── about.html         # Static about page
└── contact.html       # Static contact page
```

#### Template Features
- **Conditional Rendering**: `make-post.html` shows "New Post" or "Edit Post" based on `is_edit` flag
- **Dynamic Links**: Uses `url_for()` for all internal links
- **Safe HTML Rendering**: Uses `|safe` filter for CKEditor content

## Request Flow

### Creating a New Post

```
1. User clicks "Create New Post"
   GET /new-post
   
2. Server renders make-post.html with empty form
   is_edit=False

3. User fills form and submits
   POST /new-post

4. Server validates form data
   If valid:
   - Create BlogPost object
   - Add current date
   - Save to database
   - Redirect to /

5. Home page displays new post
```

### Editing a Post

```
1. User clicks "Edit Post" on post.html
   GET /edit-post/<id>

2. Server:
   - Fetches post from database
   - Pre-populates form with existing data
   - Renders make-post.html with is_edit=True

3. User modifies form and submits
   POST /edit-post/<id>

4. Server:
   - Updates post fields (preserves original date)
   - Commits to database
   - Redirects to /post/<id>
```

### Deleting a Post

```
1. User clicks ✘ next to post
   GET /delete/<id>

2. Server:
   - Fetches post from database
   - Deletes post
   - Commits changes
   - Redirects to /
```

## Security Considerations

1. **CSRF Protection**: Flask-WTF provides CSRF tokens for all forms
2. **Input Validation**: WTForms validators ensure data integrity
3. **SQL Injection Prevention**: SQLAlchemy ORM parameterizes all queries
4. **XSS Note**: CKEditor content is rendered with `|safe` - ensure trusted input only

## Future Improvements

- [ ] User authentication and authorization
- [ ] Comments system
- [ ] Categories/Tags for posts
- [ ] Search functionality
- [ ] Pagination for home page
- [ ] Image upload instead of URL
- [ ] Draft posts feature
- [ ] Social sharing buttons
