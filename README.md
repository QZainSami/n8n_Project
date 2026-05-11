🚀 Automated Developer Presence Engine

An automated, AI-driven pipeline that transforms raw GitHub repository data into platform-specific professional updates (Discord, LinkedIn, and CV).

🧠 What is this?

The Developer Presence Engine is an intelligent workflow built on n8n. It actively listens to GitHub activity and uses LLMs to synthesize project documentation on the fly. When a specific trigger occurs (like starring a repository), the engine parses the project's README, decodes the data, and uses Google's Gemini AI to generate customized announcements tailored for different audiences.

🎯 Why was it built?

Maintaining personal branding, updating resumes, and announcing projects across multiple platforms is a tedious manual process. This project was created to automate developer presence. Instead of manually writing Discord announcements or updating a CV after finishing a project, this engine automatically generates the content and distributes it instantly, allowing the developer to focus purely on building.

⚙️ How it Works (The Pipeline)

The Trigger: A GitHub Webhook listens for a "Star" event on a repository.

Data Fetching: The system makes an API call back to GitHub to pull the repository's README.md (which arrives in Base64 encoding).

AI Processing: Google's Gemini AI receives the raw data, transcodes it on the fly, and generates three distinct tones of copy formatted as a JSON string:

cv: A highly technical, bullet-point format.

linkedin: An engaging, professional post.

discord: A casual, community-oriented announcement.

Data Parsing: A custom JavaScript Code Node intercepts the AI's output, strips any markdown formatting, and parses the JSON into distinct variables.

Distribution: A Discord Webhook fires, delivering the tailored discord announcement directly to a private server. (Future scope: Injecting the cv data directly into a Google Docs resume).

🛠️ Tools & Technologies

n8n: The core workflow automation and orchestration engine.

Docker & Docker Compose: Containerizes the environment, ensuring the database and application run identically on any machine.

Google Gemini API: The "Brain" of the operation, handling natural language processing and formatting.

ngrok: Creates a secure tunnel from the public internet (GitHub Webhooks) to the local Docker container.

JavaScript: Custom scripting for data cleaning and JSON parsing.

Webhooks: For both ingress (GitHub) and egress (Discord).

💻 How to Run This on a New Device

Because this project is containerized with Docker and securely version-controlled (secrets ignored), it is highly portable. Follow these steps to spin it up on any machine.

Prerequisites

Docker Desktop installed.

Git installed.

An ngrok account and Auth Token.

A Google Gemini API Key.

A Discord Server Webhook URL.

1. Clone the Repository

git clone [https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git)
cd YOUR_REPO_NAME


2. Configure the Environment

For security, the .env file is excluded from version control. You must create one locally to authenticate the ngrok tunnel.
Create a file named .env in the root directory and add:

NGROK_AUTHTOKEN=your_actual_ngrok_token_here


3. Boot the Engine

Start the containerized environment. This will automatically download the required images and create a local n8n_data folder for persistent database storage.

docker-compose up -d


4. Restore the Workflow

Open a browser and navigate to http://localhost:5678.

Complete the initial n8n owner setup.

In the left menu, go to Workflows -> Add Workflow.

Click the options menu (three dots) in the top right -> Import from File.

Select the My workflow.json file included in this repository.

5. Add Your Credentials

Because n8n strips secrets during export, you must re-enter them on the new device:

Open the Message a model (Gemini) node and add your Gemini API Key.

Open the Discord node and add your Discord Webhook URL.

6. Update GitHub Webhook (Local Dev Only)

If you are running this locally, ngrok generates a new public URL every time it restarts.

Check your n8n settings or ngrok dashboard for the new forwarding URL.

Go to your GitHub Repository -> Settings -> Webhooks.

Update the Payload URL so GitHub knows where to send the "Star" events.

Built with coffee, JavaScript, and Docker.
