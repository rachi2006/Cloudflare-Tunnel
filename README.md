# ☁️ Cloudflare Tunnel

A beginner-friendly guide and practical setup for using **Cloudflare Tunnel** to securely expose local applications to the Internet without port forwarding.

## 📌 Overview

**Cloudflare Tunnel** creates a secure connection between a local application and Cloudflare's network using `cloudflared`.

Instead of opening ports on your router or exposing your server directly, traffic is routed through Cloudflare to your local application.

```text
Internet
    │
    ▼
Cloudflare
    │
    │ Secure Tunnel
    ▼
cloudflared
    │
    ▼
localhost
    │
    ▼
Your Application
```

---

## 🚀 What is Cloudflare Tunnel?

Cloudflare Tunnel allows you to connect applications running on:

* 💻 Windows
* 🐧 Linux
* 🍎 macOS
* 🐳 Docker
* ☁️ Cloud servers
* 🏠 Home servers

to Cloudflare without requiring traditional port forwarding.

For example:

```text
Local Application
http://localhost:5000
        │
        ▼
   cloudflared
        │
        ▼
   Cloudflare
        │
        ▼
https://your-domain.com
```

---

## ✨ Features

* 🔐 Secure connection between your application and Cloudflare
* 🌐 Access local applications from the Internet
* 🚫 No router port forwarding required
* 🔒 HTTPS support
* 🌍 Custom domain support
* 🛡️ Cloudflare security services
* 🔄 Support for multiple applications
* 💻 Works on Windows, Linux and macOS
* 🤖 Useful for Flask, Django, FastAPI, Node.js and AI applications
* 🧪 Quick Tunnels for development and testing

---

# 🛠️ Installation

## Windows

Download `cloudflared` from the official Cloudflare documentation:

https://developers.cloudflare.com/tunnel/downloads/

After installation, open **PowerShell** and verify:

```powershell
cloudflared --version
```

Example:

```text
cloudflared version 2026.x.x
```

---

# ⚡ Quick Tunnel

Quick Tunnel is the easiest way to test Cloudflare Tunnel.

Suppose your application is running on:

```text
http://localhost:5000
```

Run:

```powershell
cloudflared tunnel --url http://localhost:5000
```

Cloudflare will generate a temporary URL similar to:

```text
https://random-name.trycloudflare.com
```

You can open this URL from another device.

### Example

```text
Phone
  │
  ▼
https://random-name.trycloudflare.com
  │
  ▼
Cloudflare
  │
  ▼
cloudflared
  │
  ▼
localhost:5000
  │
  ▼
Flask Application
```

> ⚠️ Quick Tunnels are intended for development and testing rather than production use.

---

# 🐍 Example: Flask Application

Create a simple Flask application:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello from Cloudflare Tunnel!"

if __name__ == "__main__":
    app.run(port=5000)
```

Install Flask:

```powershell
pip install flask
```

Run the application:

```powershell
python app.py
```

Your application should now be available at:

```text
http://localhost:5000
```

Open another PowerShell window and run:

```powershell
cloudflared tunnel --url http://localhost:5000
```

You will receive a public URL.

---

# 🌐 Custom Domain

For a permanent setup, you can connect a domain to a Cloudflare Tunnel.

Example:

```text
https://app.example.com
```

can point to:

```text
http://localhost:5000
```

The request flow becomes:

```text
User
 │
 ▼
app.example.com
 │
 ▼
Cloudflare
 │
 ▼
Cloudflare Tunnel
 │
 ▼
cloudflared
 │
 ▼
localhost:5000
 │
 ▼
Application
```

---

# 🔑 Named Tunnel

A named tunnel is useful for a persistent setup.

First authenticate:

```powershell
cloudflared tunnel login
```

Create a tunnel:

```powershell
cloudflared tunnel create my-tunnel
```

Route your domain:

```powershell
cloudflared tunnel route dns my-tunnel app.example.com
```

Run the tunnel:

```powershell
cloudflared tunnel run my-tunnel
```

---

# ⚙️ Configuration Example

A tunnel configuration can look like:

```yaml
tunnel: YOUR-TUNNEL-ID
credentials-file: C:\Users\YourName\.cloudflared\YOUR-TUNNEL-ID.json

ingress:
  - hostname: app.example.com
    service: http://localhost:5000

  - service: http_status:404
```

Replace:

```text
YOUR-TUNNEL-ID
```

with your actual tunnel ID.

Replace:

```text
app.example.com
```

with your domain.

---

# 🧩 Multiple Applications

One Cloudflare Tunnel can route traffic to multiple applications.

Example:

```text
portfolio.example.com
        ↓
localhost:3000

api.example.com
        ↓
localhost:8000

app.example.com
        ↓
localhost:5000
```

Configuration:

```yaml
ingress:
  - hostname: portfolio.example.com
    service: http://localhost:3000

  - hostname: api.example.com
    service: http://localhost:8000

  - hostname: app.example.com
    service: http://localhost:5000

  - service: http_status:404
```

---

# 🔐 Security

Cloudflare Tunnel can provide an additional security layer between the Internet and your application.

However, Cloudflare Tunnel does **not** automatically make an insecure application secure.

You should still use:

* Strong passwords
* Password hashing
* Authentication
* Input validation
* Secure sessions
* HTTPS
* Environment variables
* Secure API keys
* Proper database permissions

---

# 🚨 Protect Your Secrets

Never upload sensitive information to GitHub.

### ❌ Do NOT upload:

```text
.env
Cloudflare tunnel tokens
API keys
Database passwords
MongoDB connection strings
Private keys
Cloud credentials
```

Add `.env` to `.gitignore`:

```gitignore
.env
*.pem
```

Example `.env`:

```env
MONGO_URI=your-mongodb-uri
API_KEY=your-api-key
CLOUDFLARE_TUNNEL_TOKEN=your-token
```

Keep this file only on your local machine/server.

---

# 🆚 Quick Tunnel vs Named Tunnel

| Feature                  | Quick Tunnel                        | Named Tunnel |
| ------------------------ | ----------------------------------- | ------------ |
| Setup                    | Very easy                           | More setup   |
| Temporary URL            | ✅                                   | ❌            |
| Custom domain            | ❌                                   | ✅            |
| Development              | ✅                                   | ✅            |
| Production               | ❌                                   | ✅            |
| Cloudflare account       | Not required for basic Quick Tunnel | Required     |
| Persistent configuration | ❌                                   | ✅            |

---

# 🆚 Cloudflare Tunnel vs Port Forwarding

### Traditional Port Forwarding

```text
Internet
   │
   ▼
Public IP
   │
   ▼
Router
   │
   ▼
Open Port
   │
   ▼
Your PC
```

### Cloudflare Tunnel

```text
Your PC
   │
   ▼
cloudflared
   │
   ▼
Cloudflare
   │
   ▼
Internet
```

Cloudflare Tunnel removes the need for traditional inbound port forwarding.

---

# 🤖 Cloudflare Tunnel for AI Projects

Cloudflare Tunnel can be useful for AI projects and personal assistants.

Example:

```text
             Internet
                 │
                 ▼
        Cloudflare Tunnel
                 │
                 ▼
            cloudflared
                 │
                 ▼
           AI Application
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
      Python    APIs    Database
```

For example, an AI assistant running on:

```text
localhost:8000
```

can potentially be accessed through:

```text
https://assistant.example.com
```

---

# 🧪 Useful Commands

Check version:

```powershell
cloudflared --version
```

Login:

```powershell
cloudflared tunnel login
```

Create tunnel:

```powershell
cloudflared tunnel create my-tunnel
```

List tunnels:

```powershell
cloudflared tunnel list
```

Run tunnel:

```powershell
cloudflared tunnel run my-tunnel
```

Quick Tunnel:

```powershell
cloudflared tunnel --url http://localhost:5000
```

Route DNS:

```powershell
cloudflared tunnel route dns my-tunnel app.example.com
```

---

# 📚 Learning Path

If you are learning Cloudflare Tunnel from scratch, follow this order:

```text
1. Learn localhost
        ↓
2. Run a Flask application
        ↓
3. Install cloudflared
        ↓
4. Create a Quick Tunnel
        ↓
5. Understand HTTPS
        ↓
6. Connect a domain
        ↓
7. Create a Named Tunnel
        ↓
8. Configure DNS
        ↓
9. Learn Cloudflare security
        ↓
10. Deploy your application
```

---

# 🎯 Example Project Structure

```text
cloudflare-tunnel/
│
├── README.md
├── app.py
├── requirements.txt
├── .gitignore
└── .env
```

Example `.gitignore`:

```gitignore
.env
__pycache__/
*.pyc
.venv/
```

---

# ⚠️ Important Notes

* Your local application must be running for the tunnel to reach it.
* Your computer needs an Internet connection.
* Quick Tunnels are mainly intended for development/testing.
* Keep Cloudflare credentials private.
* Never commit `.env` files containing secrets.
* A tunnel does not replace application-level security.

---

# 📖 Official Documentation

* Cloudflare Tunnel: https://developers.cloudflare.com/tunnel/
* Cloudflare Tunnel Downloads: https://developers.cloudflare.com/tunnel/downloads/
* Cloudflare Tunnel Get Started: https://developers.cloudflare.com/tunnel/get-started/

---

# 👨‍💻 Author

**Rachith Kumar**

Student | AI & Data Science

Interested in:

* Python
* Data Science
* AI
* Full-Stack Development
* Flask
* Django
* Cloud Technologies

---

## ⭐ If this repository helped you

Give the repository a ⭐ and use the guide to experiment with Cloudflare Tunnel and your own local applications.
