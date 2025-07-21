Architectural Design Assistant
A creative tool designed for architects and engineers that transforms text briefs and rough sketches into refined design concepts using multimodal AI, featuring compliance checking and design narrative generation.

Project Objective
To develop a full-stack web application that acts as an AI-powered assistant for architectural design, demonstrating multimodal input processing (text and sketch) and AI-driven concept generation and analysis. The project aims to provide a proof-of-concept for how AI can streamline early-stage architectural design workflows.

Key Features Implemented
User Authentication: Secure user registration and login using JWT (JSON Web Tokens) for session management, ensuring personalized design experiences.

Design Input: Intuitive React interface with an interactive drawing canvas for rough sketch input and a text area for detailed architectural briefs.

Multimodal AI Concept Generation:

Processes both text brief and sketch input.

Generates a 2D SVG Blueprint dynamically using the Google Gemini 2.0 Flash API based on the text prompt. This simulates the AI's ability to create design schematics.

Provides a Design Narrative and Code Compliance Status (mocked based on keywords in the text brief) to offer conceptual feedback.

3D Design Preview & Manipulation: Displays a symbolic 3D model of the generated concept with interactive rotation and zoom controls (powered by react-three-fiber and drei), visually linking the AI's output to a 3D space. The 3D model's basic shape dynamically changes based on input keywords (e.g., house, tower, hut, tree).

Design History: Users can view a personalized list of their past generated designs, showcasing design iteration and version control capabilities.

New Design Workflow: A dedicated button to clear inputs and start a fresh design concept.

Technologies Used
Frontend
React.js: The core library for building the dynamic user interface.

react-sketch-canvas: For capturing freehand sketch input.

@react-three/fiber (React Three Fiber): A powerful React renderer for three.js, enabling declarative 3D scene rendering.

@react-three/drei: A collection of useful helpers for react-three-fiber, including OrbitControls for camera manipulation (rotation, zoom) and Text for 3D labels.

HTML5 Canvas API: Used by react-sketch-canvas for drawing.

CSS (Inline & App.css): For responsive and appealing styling.

Backend
FastAPI: A modern, fast (high-performance) web framework for building Python APIs, known for its speed and automatic documentation.

PostgreSQL: A robust, open-source relational database system used for persistent storage of user accounts and design project data.

SQLAlchemy: Python SQL Toolkit and Object Relational Mapper (ORM) for seamless interaction with the PostgreSQL database.

psycopg2-binary: The PostgreSQL adapter for Python.

passlib[bcrypt]: For secure password hashing (bcrypt algorithm).

python-jose: For handling JWT (JSON Web Tokens) for secure session management.

python-dotenv: For loading environment variables securely.

requests: For making internal HTTP API calls (e.g., from backend to Gemini).

uvicorn: An ASGI server for running the FastAPI application.

AI / Generative Models
Google Gemini 2.0 Flash API: Utilized for generating minimalist 2D SVG blueprints from user-provided text prompts. This demonstrates AI's ability to translate conceptual text into structured graphical output.

(Conceptual: Google Gemini Vision / GPT-4V for deeper multimodal analysis of sketches, Stable Diffusion for realistic image generation, specialized architectural reasoning models for advanced compliance checking - discussed as future enhancements due to project scope and resource requirements).

Infrastructure
Docker Desktop: Essential for running a local, isolated PostgreSQL database instance in a container, simplifying database setup.

Windows Subsystem for Linux (WSL 2): Docker Desktop's backend on Windows, providing a Linux kernel for optimal container performance.

Setup and Local Development
Follow these comprehensive steps to get the Architectural Design Assistant running on your local machine.

Prerequisites
Ensure you have the following installed on your system:

Node.js & npm:

Download the LTS version of Node.js (includes npm).

During installation on Windows, ensure "Add Node.js to PATH" is checked.

Verify: node -v and npm -v in a new terminal.

Python 3.10+ (stable version):

Download a stable Python 3.x installer (e.g., 3.12.x or 3.11.x).

During installation on Windows, ensure "Add Python to PATH" is checked.

Verify: python --version (or python3 --version) in a new terminal.

If you encounter "Python was not found", disable "App installer Python.exe" aliases in Windows Settings > Apps > Advanced app settings > App execution aliases.

Git:

Download and install Git for Windows.

During installation, select "Git from the command line and also from 3rd-party software" for PATH integration.

Verify: git --version in a new terminal.

Docker Desktop:

Download and install Docker Desktop.

Ensure WSL 2 integration is enabled in Docker Desktop Settings (Settings > Resources > WSL Integration).

Install/Update WSL 2 Distribution: Open an Administrator PowerShell/Command Prompt and run wsl.exe --install -d Ubuntu (if not already installed) and then wsl.exe --update. Restart your computer after WSL installation/update.

Verify Docker Desktop is running (green whale icon in system tray).

Google Cloud API Key for Gemini:

Go to Google Cloud Console.

Log in with your Google account.

Select an existing project or create a new one.

Navigate to APIs & Services > Library, search for and Enable "Generative Language API".

Navigate to APIs & Services > Credentials, click + CREATE CREDENTIALS > API Key.

Copy the generated API Key immediately. This key will be explicitly used in your frontend code.

For testing, ensure "Don't restrict key" is selected under "API restrictions" for this key.

Project Setup Steps
Clone the repository:
Open a new Terminal/Command Prompt/PowerShell window (not Administrator).

cd C:\Users\YourUsername\OneDrive\Documents\Projects # Or your preferred projects directory
git clone https://github.com/tiduchauhan/ARCHITECTURE-AI.git
cd ARCHITECTURE-AI # Navigate into the cloned repository root

(Note: If you already have the project folders locally and initialized Git outside of cloning, ensure your terminal is in the root ARCHITECTURE-AI folder and run git pull origin main --allow-unrelated-histories to sync with the GitHub README, then resolve any merge conflicts, and finally git push origin main.)

Backend Setup:

Navigate to the backend directory:

cd architectural-design-assistant-backend

Create a Python virtual environment and activate it:

python -m venv venv
# On Windows Command Prompt: venv\Scripts\activate
# On Windows PowerShell: .\venv\Scripts\Activate.ps1

Your prompt should now start with (venv).

Install Python dependencies:

pip install fastapi uvicorn "python-dotenv[dotenv]" sqlalchemy psycopg2-binary passlib[bcrypt] python-jose requests

Create a .env file:
In the architectural-design-assistant-backend directory (same level as main.py), create a file named .env with the following content:

DATABASE_URL="postgresql://myuser:mypassword@127.0.0.1/mydatabase"
SECRET_KEY="your-super-secret-key-change-this-in-production"

(Replace myuser, mypassword, mydatabase with your desired PostgreSQL credentials that you'll use for Docker).

Start the PostgreSQL Docker container:
Ensure Docker Desktop is running. Open a separate new Windows terminal (not the one running your backend virtual environment).

docker run --name my-postgres-db -e POSTGRES_USER=myuser -e POSTGRES_PASSWORD=mypassword -e POSTGRES_DB=mydatabase -p 5432:5432 -d postgres

(If you get an error that the name is already in use, run docker rm my-postgres-db first. Verify it's running with docker ps.)
If you previously registered users and need to clear the database due to schema changes (e.g., email not being unique), access the Docker CLI (Docker Desktop > Containers > my-postgres-db > ... > Open in terminal), then psql -U myuser -d mydatabase, then DROP TABLE users;, then \q, then exit.

Start the FastAPI backend server:
In your activated backend terminal ((venv) prompt):

uvicorn main:app --reload

Verify it starts with INFO: Application startup complete..

Frontend Setup:

Navigate to the frontend directory:
Open a separate new Windows terminal (keep backend running).

cd C:\Users\YourUsername\OneDrive\Documents\Projects\ARCHITECTURE-AI\architectural-design-assistant-frontend

Install Node.js dependencies:

npm install
npm install @react-three/fiber @react-three/drei

Update src/App.js with your Google API Key for Gemini:
Open src/App.js in your code editor. Locate the handleGenerateImage function.
Replace const apiKey = "AIzaSyB64zu4gyTjLTuDbiJlPK2k90BDb9Kmkms"; with your actual Google API Key. (Ensure no extra spaces).

Start the React frontend development server:

npm start

Your browser should automatically open to http://localhost:3000.

How to Use the Application
Once all three components (PostgreSQL, FastAPI, React) are running:

Register and Log In:

Open your browser to http://localhost:3000.

Register a new user (e.g., testuser / password123). You can leave the email field blank.

Log in with your newly created credentials. You should see a "Welcome, testuser!" message.

Generate Design Concept (Multimodal Input):

In Section 1: "Enter Architectural Brief", type a description (e.g., "A modern house with a green roof and large windows", "A tall office building", "A simple hut blueprint", "A tree diagram").

In Section 2: "Draw Rough Sketch", use your mouse to draw a simple sketch.

Click the green "Generate Design" button.

Observe:

An alert will confirm the design ID.

Section 3: "Generated Design Concept & Details" will update with:

An SVG Blueprint image (dynamically generated by Gemini based on your text brief).

A Design Narrative and Code Compliance Status (mocked based on keywords).

A manipulable 3D model that changes its basic shape (house, tower, hut, tree) based on your brief, and can be rotated/zoomed with your mouse.

View Past Designs:

Click the "View My Past Designs" button to see a list of all your previously generated designs. Click again to hide.

New Design:

Click the "New Design" button to clear all inputs and generated outputs, ready for a fresh design.

Logout:

Click the "Logout" button to return to the authentication screen.

Future Enhancements (Roadmap)
This project serves as a robust MVP. Here are some areas for future development:

Advanced AI Integration:

Full integration with Google Gemini Vision / GPT-4V for deeper analysis and interpretation of the sketch input (e.g., extracting shapes, dimensions, and spatial relationships from the drawing).

Integration with specialized architectural reasoning models for more accurate and real-time code compliance checking.

Dynamic 3D Model Generation: Implementing algorithms or AI models to convert the generated 2D SVG blueprints into highly detailed, interactive 3D models (e.g., GLTF/GLB format) that can be loaded and manipulated in the 3D viewer.

Real-Time Collaboration: Implement Server-Sent Events (SSE) or WebSockets for live design updates and collaborative sessions among multiple users.

Advanced Project Management: Develop more robust version control directly within the app, granular project sharing features with permissions, and commenting/feedback tools.

OAuth Integration: Integrate third-party login options like Google or GitHub for easier user onboarding.

Deployment: Containerize the entire application with Docker Compose, optimize for GPU support for AI models, and deploy to a cloud platform (e.g., Google Cloud Run, Kubernetes) for public access.

UI/UX Improvements: Enhance the drawing tools, add more sophisticated 3D manipulation features, and refine the overall user interface.
