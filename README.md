# ===== Imports =====
from flask import Flask, render_template_string, request, redirect, send_from_directory
import sqlite3
import os
import uuid
import subprocess

# ===== Configuration =====
UPLOAD_FOLDER = "uploads"
DB_PATH = "videos.db"
ALLOWED_EXTENSIONS = {'mp4', 'mov', 'avi', 'mkv'}

os.makedirs(UPLOAD_FOLDER, exist_ok=True)

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER

# ===== Database Setup =====
def init_db():
    conn = sqlite3.connect(DB_PATH)
    c = conn.cursor()
    c.execute('''CREATE TABLE IF NOT EXISTS videos
                 (id TEXT PRIMARY KEY, title TEXT, filename TEXT, uploaded_at TEXT)''')
    conn.commit()
    conn.close()

init_db()

# ===== Helpers =====
def allowed_file(filename):
    return '.' in filename and filename.rsplit('.', 1)[1].lower() in ALLOWED_EXTENSIONS

def save_video(file, title):
    vid_id = str(uuid.uuid4())
    ext = file.filename.rspli


