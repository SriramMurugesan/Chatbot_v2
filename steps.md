Nice question, Sriram. Since you are already working with **Python backend technologies** and learning deeply, converting this script into a **Django full-stack project** is a very good practice. It will help you understand **API integration, backend views, templates, and environment variables**.

I’ll show you a **simple Django full-stack structure** where a user can type a question and Gemini will answer it.

---

# 1. Install Required Packages

```bash
pip install django python-dotenv google-genai
```

---

# 2. Create Django Project

```bash
django-admin startproject ai_teacher
cd ai_teacher
django-admin startapp chatbot
```

Project structure:

```
ai_teacher/
│
├── ai_teacher/
│   ├── settings.py
│   ├── urls.py
│
├── chatbot/
│   ├── views.py
│   ├── urls.py
│   ├── templates/
│   │    └── index.html
│
├── .env
└── manage.py
```

---

# 3. Add API Key in `.env`

```
GOOGLE_API_KEY=your_api_key_here
```

---

# 4. Update `settings.py`

Add your app:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'chatbot',
]
```

Load `.env`

```python
from dotenv import load_dotenv
import os

load_dotenv()

GOOGLE_API_KEY = os.getenv("GOOGLE_API_KEY")
```

---

# 5. Create Gemini Logic in `views.py`

`chatbot/views.py`

```python
from django.shortcuts import render
from google import genai
from dotenv import load_dotenv
import os

load_dotenv()

client = genai.Client(api_key=os.getenv("GOOGLE_API_KEY"))


def home(request):

    answer = ""

    if request.method == "POST":
        question = request.POST.get("question")

        response = client.models.generate_content(
            model="gemini-2.5-flash",
            contents=question
        )

        answer = response.text

    return render(request, "index.html", {"answer": answer})
```

---

# 6. Create Template

`chatbot/templates/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>AI Teacher</title>
</head>
<body>

<h2>Ask AI</h2>

<form method="post">
    {% csrf_token %}
    <input type="text" name="question" placeholder="Ask something..." style="width:400px">
    <button type="submit">Ask</button>
</form>

<br>

<h3>Answer:</h3>
<p>{{ answer }}</p>

</body>
</html>
```

---

# 7. Create App URLs

`chatbot/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name="home"),
]
```

---

# 8. Connect Main URLs

`ai_teacher/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('chatbot.urls')),
]
```

---

# 9. Run the Project

```bash
python manage.py migrate
python manage.py runserver
```

Open browser:

```
http://127.0.0.1:8000
```

You will see:

```
Ask AI
[ input box ]

Answer:
...
```

---

# How This Works (Architecture)

```
Browser
   ↓
Django Template (HTML Form)
   ↓
Django View
   ↓
Gemini API (google-genai)
   ↓
Response
   ↓
Template Rendering
```

---

# Next Level (What Real Full Stack Looks Like)

After this basic version, you can upgrade it to:

1️⃣ **Chat UI like ChatGPT**
2️⃣ **AJAX / Fetch API instead of form submit**
3️⃣ **Store chat history in database**
4️⃣ **User login system**
5️⃣ **React frontend + Django API**

---

💡 Since you are building **AI Teacher**, I can also show you a **proper architecture used in startups**:

```
React
   ↓
Django REST API
   ↓
AI Service Layer
   ↓
Gemini / OpenAI
   ↓
PostgreSQL
```

If you want, I can also show you:

**"How to build a complete AI Teacher SaaS with Django + Gemini step-by-step."**

That will be a **very strong backend project for your portfolio and interviews.**
