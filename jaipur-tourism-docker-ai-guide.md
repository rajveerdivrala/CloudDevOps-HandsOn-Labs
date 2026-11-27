# 🏰 Jaipur Tourism & Historical Places — Built with Docker AI (Gordon) & Deployed on Vercel

🔗 **Live Demo:** [https://jaipur-tourism-website.vercel.app](https://jaipur-tourism-website.vercel.app)

---

## 📌 Project Overview

This repository contains a web application showcasing **Jaipur Tourism and Historical Places**, generated using **Docker AI (Gordon)** from a single natural-language prompt and deployed publicly on **Vercel** via GitHub.

### ✨ Key Features
* **Curated Jaipur Landmarks:** Highlights iconic tourist and historical destinations of the Pink City.
* **Rich 500-Character Descriptions:** Concise, informative ~500-character summaries for each historical monument.
* **"Plan Your Visit" Section:** Interactive tour booking interface designed for visitors *(currently a frontend prototype)*.
* **Containerized & Cloud-Ready:** Includes Docker configuration files (`Dockerfile` and `docker-compose.yml`) for local container testing alongside static assets (`index.html`) for instant serverless deployment.

---

## 🛠️ End-to-End Workflow: From Prompt to Live Website

Anyone looking to replicate this workflow—from generating an app with Docker AI to hosting it free on the internet—can follow these steps:

### Step 1: Enable & Run Docker AI (Gordon)
1. Ensure **Docker Desktop** is installed and running on your machine.
2. Enable **Docker AI (Gordon)** in Docker Desktop settings (or access it via the Docker CLI / Docker Desktop AI panel).

### Step 2: Generate the Application Using a Prompt
I provided the following prompt to **Docker AI (Gordon)**:

> *"Jaipur tourist places and historical places application for jaipur tourism containerised for create a dockerfile and add the HTML code of Jaipur best tourist places and historical place with 500 characters description for every place for jaipur tourism booking and run the application"*

**What Docker AI Automatically Handled:**
* Created the project directory structure.
* Generated the complete frontend HTML/CSS code (`index.html`) featuring Jaipur's historical places, 500-character descriptions, and a tour booking form.
* Wrote the `Dockerfile` and `docker-compose.yml` configuration files to containerize the web application.
* Built the Docker image and started the container automatically.

### Step 3: Verify Locally on `localhost`
Once Docker AI built and ran the container, I verified the running application locally in the browser on port **`8081`**:

```text
http://localhost:8081
```

*(Note: Depending on your port mapping—such as `-p 8081:8000` or `-p 8089:8000`—you can access the containerized app locally to test the layout and features before deploying.)*

### Step 4: Push Code & Configuration Files to GitHub
To make the website accessible publicly on the internet—even after stopping the local Docker container—I pushed the `index.html` file along with the Docker configuration files (`Dockerfile`, `docker-compose.yml`) to this GitHub repository:

```bash
git init
git add .
git commit -m "Add Jaipur Tourism website generated with Docker AI"
git branch -M main
git remote add origin [https://github.com/rajveerdivrala/](https://github.com/rajveerdivrala/)<your-repo-name>.git
git push -u origin main
```

### Step 5: Import & Deploy on Vercel (Serverless Hosting)
1. Logged into **[vercel.com](https://vercel.com)** and connected my GitHub account.
2. Clicked **Add New → Project** and imported this GitHub repository.
3. Kept the default static deployment settings and clicked **Deploy**.
4. Within seconds, Vercel generated a live production URL:
   👉 **[https://jaipur-tourism-website.vercel.app](https://jaipur-tourism-website.vercel.app)**

---

## 💡 Why Serverless Hosting Works Here (Zero Cost & Zero Server Maintenance)

* **Pure Static Frontend:** Since the current version (`v1`) is built with static HTML/CSS/JS, Vercel serves the files directly over its global CDN without requiring a continuously running backend VM or container.
* **Always Online (24/7):** Even if the local Docker container or local machine is shut down, the website remains live and accessible worldwide.
* **Zero Hosting Cost:** Hosted on Vercel's free Hobby tier with automatic SSL (`HTTPS`) and continuous deployment—every new `git push` to `main` automatically updates the live site.

---

## 🗺️ Roadmap for Future Enhancements (`v2`)
* [ ] Replace placeholder visuals with high-resolution photography of Jaipur monuments (Hawa Mahal, Amer Fort, Nahargarh, Jal Mahal, City Palace, Jantar Mantar).
* [ ] Connect the **"Plan Your Visit"** booking form to a REST API backend (Python FastAPI / Node.js) and a **PostgreSQL** database to persist real reservations.
