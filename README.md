# Blog with Posts

A full-featured blog application built with Flask, featuring CRUD operations for blog posts with a clean Bootstrap-based UI.

## Features

- **View All Posts**: Browse all blog posts on the home page with thumbnail previews
- **Read Individual Posts**: Click on any post to read the full content
- **Create New Posts**: Add new blog posts with a rich text editor (CKEditor)
- **Edit Posts**: Update existing posts while preserving the original publish date
- **Delete Posts**: Remove posts from the database with a single click
- **Responsive Design**: Bootstrap 5 styling for a clean, mobile-friendly interface

## Tech Stack

- **Backend**: Flask (Python)
- **Database**: SQLite with Flask-SQLAlchemy
- **ORM**: SQLAlchemy with modern type annotations
- **Forms**: Flask-WTF with WTForms
- **Rich Text Editor**: Flask-CKEditor
- **Frontend**: Bootstrap 5 (via Flask-Bootstrap)
- **Templating**: Jinja2

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/arvie993/Blog_w_posts.git
   cd Blog_w_posts
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Run the application:
   ```bash
   python main.py
   ```

5. Open your browser and navigate to `http://127.0.0.1:5003`

## Project Structure

```
Day67-starting-files-upgraded-blog/
├── main.py                 # Flask application and routes
├── requirements.txt        # Python dependencies
├── README.md              # Project documentation
├── ARCHITECTURE.md        # Technical architecture details
├── instance/
│   └── posts.db           # SQLite database
├── static/
│   ├── assets/
│   │   └── img/           # Background images
│   ├── css/
│   │   └── styles.css     # Custom styles
│   └── js/
│       └── scripts.js     # JavaScript files
└── templates/
    ├── header.html        # Navigation header
    ├── footer.html        # Page footer
    ├── index.html         # Home page (list all posts)
    ├── post.html          # Individual post view
    ├── make-post.html     # Create/Edit post form
    ├── about.html         # About page
    └── contact.html       # Contact page
```

## Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Display all blog posts |
| `/post/<id>` | GET | View a specific post |
| `/new-post` | GET, POST | Create a new post |
| `/edit-post/<id>` | GET, POST | Edit an existing post |
| `/delete/<id>` | GET | Delete a post |
| `/about` | GET | About page |
| `/contact` | GET | Contact page |

## Usage

### Creating a Post
1. Click "Create New Post" on the home page
2. Fill in the title, subtitle, author name, and image URL
3. Write your content using the rich text editor
4. Click "Submit Post"

### Editing a Post
1. Click on a post to view it
2. Click "Edit Post" at the bottom
3. Modify the content as needed
4. Click "Submit Post" to save changes

### Deleting a Post
1. On the home page, click the ✘ next to the post you want to delete

## License

This project is for educational purposes as part of the 100 Days of Python course.
