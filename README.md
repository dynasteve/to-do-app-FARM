# FARM Stack To-Do App

A full-stack to-do application built with the FARM stack (FastAPI, React, MongoDB). This application provides a RESTful API backend powered by FastAPI and MongoDB, with a React frontend for managing todo lists and items.

## Features

- ✅ Create, read, update, and delete todo lists
- ✅ Add, remove, and toggle completion status of todo items
- ✅ Async MongoDB operations with Motor
- ✅ RESTful API design
- ✅ Pydantic models for data validation
- ✅ Environment-based configuration
- ✅ Clean separation of concerns with Data Access Layer (DAL)

## Tech Stack

### Backend
- **FastAPI** - Modern, fast web framework for building APIs
- **MongoDB** - NoSQL database for flexible data storage
- **Motor** - Async MongoDB driver for Python
- **Pydantic** - Data validation and settings management
- **Uvicorn** - ASGI server implementation
- **Docker** - Containerization support
- **Pytest** - Testing framework

### Frontend
- **React 19** - Frontend JavaScript library
- **Axios** - HTTP client for API requests
- **React Icons** - Icon library for UI components
- **React Testing Library** - Testing utilities for React components
- **Jest** - JavaScript testing framework

### Infrastructure
- **Docker Compose** - Multi-container orchestration
- **Nginx** - Reverse proxy and load balancer
- **Node.js 22** - JavaScript runtime for frontend

## Project Structure

```
to-do-app-FARM/
├── backend/
│   ├── src/
│   │   ├── server.py          # FastAPI application and routes
│   │   ├── dal.py             # Data Access Layer with MongoDB operations
│   │   └── requirements.txt   # Python dependencies
│   ├── Dockerfile             # Docker configuration for backend
│   ├── pyproject.toml         # Python project configuration
│   └── ...
├── frontend/
│   ├── src/
│   │   ├── App.js             # Main React application component
│   │   ├── App.css            # Main application styles
│   │   ├── ListToDoLists.js   # Component for displaying todo lists
│   │   ├── ListToDoLists.css  # Styles for todo lists view
│   │   ├── ToDoList.js        # Component for individual todo list
│   │   ├── ToDoList.css       # Styles for todo list component
│   │   ├── index.js           # React app entry point
│   │   └── index.css          # Global styles
│   ├── package.json           # Frontend dependencies and scripts
│   └── ...
├── nginx/
│   └── nginx.conf             # Nginx reverse proxy configuration
├── compose.yaml               # Docker Compose orchestration
├── .env                       # Environment variables (create this)
└── README.md
```

## API Endpoints

### Todo Lists
- `GET /api/lists` - Retrieve all todo lists with summaries
- `POST /api/lists` - Create a new todo list
- `GET /api/lists/{list_id}` - Get a specific todo list with all items
- `DELETE /api/lists/{list_id}` - Delete a todo list

### Todo Items
- `POST /api/lists/{list_id}/items/` - Add a new item to a todo list
- `DELETE /api/lists/{list_id}/items/{item_id}` - Delete an item from a todo list
- `PATCH /api/lists/{list_id}/checked_state` - Update the checked state of an item

### Utility
- `GET /api/dummy` - Test endpoint returning dummy data

## Installation & Setup

### Prerequisites
- Python 3.8+
- Node.js 14+
- MongoDB Atlas account or local MongoDB installation

### Backend Setup

1. Clone the repository:
```bash
git clone https://github.com/dynasteve/to-do-app-FARM.git
cd to-do-app-FARM
```

2. Navigate to the backend directory:
```bash
cd backend
```

3. Create a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

4. Install dependencies:
```bash
pip install -r src/requirements.txt
```

5. Set up environment variables:
Create a `.env` file in the backend directory:
```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/database_name
DEBUG=true
```

6. Run the FastAPI server:
```bash
cd src
python server.py
```

The API will be available at `http://localhost:3001`

### Frontend Setup

1. Navigate to the frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm start
```

The React app will be available at `http://localhost:3000`

### Frontend Testing

Run the React test suite:
```bash
cd frontend
npm test
```

Build for production:
```bash
npm run build
```

## Docker Deployment

### Full Stack with Docker Compose (Recommended)

1. Create a `.env` file in the root directory:
```env
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/database_name
DEBUG=true
```

2. Build and start all services:
```bash
docker-compose up --build
```

Or run in detached mode:
```bash
docker-compose up -d --build
```

3. Access the application:
- **Frontend**: http://localhost:8000 (via Nginx proxy)
- **API Documentation**: http://localhost:8000/api/docs
- **Direct Backend Access**: http://localhost:8001
- **Direct Frontend Access**: http://localhost:3000

4. Stop all services:
```bash
docker-compose down
```

### Architecture
The Docker Compose setup includes:
- **Nginx**: Reverse proxy routing requests to appropriate services
- **Frontend**: React development server (Node.js 22)
- **Backend**: FastAPI application with auto-reload
- **Network**: Internal Docker network for service communication

### Individual Container Deployment

#### Backend with Docker

1. Build the Docker image:
```bash
cd backend
docker build -t todo-app-backend .
```

2. Run the container:
```bash
docker run -p 3001:3001 \
  -e MONGODB_URI="your_mongodb_connection_string" \
  -e DEBUG=false \
  todo-app-backend
```

The API will be available at `http://localhost:3001`

## Data Models

### ToDoList
```python
{
    "id": "string",
    "name": "string",
    "items": [ToDoListItem]
}
```

### ToDoListItem
```python
{
    "id": "string",
    "label": "string", 
    "checked": boolean
}
```

### ListSummary
```python
{
    "id": "string",
    "name": "string",
    "item_count": integer
}
```

## API Usage Examples

### Create a new todo list
```bash
curl -X POST "http://localhost:3001/api/lists" \
     -H "Content-Type: application/json" \
     -d '{"name": "My Shopping List"}'
```

### Add an item to a list
```bash
curl -X POST "http://localhost:3001/api/lists/{list_id}/items/" \
     -H "Content-Type: application/json" \
     -d '{"label": "Buy groceries"}'
```

### Toggle item completion
```bash
curl -X PATCH "http://localhost:3001/api/lists/{list_id}/checked_state" \
     -H "Content-Type: application/json" \
     -d '{"item_id": "item_id_here", "checked_state": true}'
```

## React Components

### Main Components
- **App.js** - Root application component that manages routing and global state
- **ListToDoLists.js** - Displays all todo lists with summary information (name and item count)
- **ToDoList.js** - Individual todo list component with items management

### Features
- View all todo lists with item counts
- Create new todo lists
- Add, remove, and toggle completion status of items
- Delete entire todo lists
- Responsive design with custom CSS styling
- Icon integration with React Icons

## Development

### Running Tests
The project uses pytest for testing:
```bash
cd backend
pytest
```

### Running in Development Mode
Set `DEBUG=true` in your environment variables to enable:
- Auto-reload on code changes
- Debug logging
- Development-specific configurations

### Database Schema
The application uses a single MongoDB collection `todo_lists` with the following structure:
```javascript
{
  "_id": ObjectId,
  "name": "string",
  "items": [
    {
      "id": "string",
      "label": "string", 
      "checked": boolean
    }
  ]
}
```

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Environment Variables

### Required Environment Variables
Create a `.env` file in the root directory:

| Variable | Description | Required | Default | Example |
|----------|-------------|----------|---------|---------|
| `MONGODB_URI` | MongoDB connection string | Yes | - | `mongodb+srv://user:pass@cluster.mongodb.net/todoapp` |
| `DEBUG` | Enable debug mode | No | `false` | `true` |

### Docker Compose Environment
The compose setup automatically:
- Uses the `.env` file for backend configuration
- Sets `NODE_ENV=development` for frontend
- Configures `WDS_SOCKET_PORT=0` for hot reload in containers
- Enables debug mode for development

## License

This project is open source and available under the [MIT License](LICENSE).

## Future Enhancements

- [ ] User authentication and authorization
- [ ] Real-time updates with WebSockets
- [ ] Task due dates and priorities
- [ ] Task categories and tags
- [ ] Data export/import functionality
- [ ] Mobile responsive design improvements

---

Built with ❤️ using the FARM stack