# Copilot Instructions: Mergington High School Activities API

## Project Overview

This is a lightweight **FastAPI** web application for a high school activities management system. It combines:
- **Backend**: Python FastAPI API (`src/app.py`) with in-memory data storage
- **Frontend**: Vanilla HTML/CSS/JS (`src/static/`) that calls REST endpoints

**Architecture**: Single-file FastAPI app with embedded data model. Static files served via FastAPI's StaticFiles mount at `/static`.

## Running the Application

```bash
# Install dependencies
pip install fastapi uvicorn

# Run the server (from project root)
python src/app.py

# Access:
# - UI: http://localhost:8000/ (redirects to index.html)
# - API docs: http://localhost:8000/docs (Swagger UI)
# - Alternative docs: http://localhost:8000/redoc
```

## Key Patterns & Conventions

### Data Model
- **In-memory storage**: All data resets on server restart (no persistence)
- **Activities** use activity name as unique ID (string key in dictionary)
- **Students** identified by email (stored as list in participants array)
- Located in `activities` dict at top of `src/app.py`

### API Design
- **GET /activities**: Returns full activity data including participant lists
- **POST /activities/{activity_name}/signup?email=...**: Adds student email to activity's participants list
- **Root (/)**: Redirects to `/static/index.html`
- Minimal error handling: missing activities return 404 HTTPException

### Frontend Integration
- **app.js** fetches from `/activities` endpoint on page load
- Dynamically renders activity cards showing description, schedule, and availability (`max_participants - participants.length`)
- Signup form uses `encodeURIComponent()` for URL encoding activity names and emails
- Flash messages (success/error) auto-hide after 5 seconds

## Development Notes

- **No validation**: Signup endpoint doesn't check email format or duplicate signups
- **No tests**: pytest.ini exists but no test files present
- **CORS not configured**: Frontend and API run on same origin
- **Static files**: Mount path is hardcoded; changing directory structure requires updating the mount configuration

## File Organization

```
src/
  app.py          # Single FastAPI app file (main backend logic)
  static/
    index.html    # UI structure
    app.js        # Frontend logic and API calls
    styles.css    # Styling
```

## Common Tasks for AI Agents

- **Adding features**: Modify `activities` dict or add new endpoints to `app.py`
- **Fixing bugs**: Check API response handling in `app.js` and POST endpoint validation
- **UI changes**: Edit HTML structure in `index.html` or styling in `styles.css`
- **Testing improvements**: Add tests in new `test_app.py` file; pytest configured with `pythonpath = .`
