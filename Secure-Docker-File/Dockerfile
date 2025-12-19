# Stage 1: Builder (Dependencies ko compile karne ke liye)
# Behtar hai ke dono stages mein same python version use karein (3.10)
FROM python:3.10 as builder

# System dependencies install karein
RUN apt-get update \
    && apt-get install -y --no-install-recommends gcc libpq-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Pehle sirf requirements copy karein taake cache use ho sake
COPY requirements.txt .

# Prefix ka use karte hue dependencies /install folder mein install karein
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# Stage 2: Final (Isse image ka size chhota rehta hai)
FROM python:3.10-slim

# Security ke liye root user ki bajaye alag user banayein
RUN adduser --disabled-password --gecos '' appuser

WORKDIR /app

# Environment variables set karein
# PYTHONPATH zaroori hai taake python ko pata chale packages kahan hain
ENV PYTHONUNBUFFERED=1 \
    PYTHONPATH="/install/lib/python3.10/site-packages" \
    PATH="/install/bin:$PATH"

# Builder stage se sirf installed packages copy karein
COPY --from=builder /install /install

# Agar aap postgres use kar rahe hain to runtime library zaroori ho sakti hai
RUN apt-get update && apt-get install -y libpq5 && rm -rf /var/lib/apt/lists/*

# Ab apna baqi code copy karein
COPY . .

# Permissions set karein. /app ko bhi appuser ki malkiyat dein taake code run ho sake
RUN mkdir -p /spp \
    && chown -R appuser:appuser /spp /app

# User switch karein
USER appuser

EXPOSE 8000

# Gunicorn command fix: "runserver" hata diya gaya hai kyunke woh Django command hai
CMD ["gunicorn", "notesapp.wsgi:application", "--bind", "0.0.0.0:8000"]
#RUN pip install -r requirements.txt
#RUN python manage.py makemigrations
#RUN python manage.py migrate
#EXPOSE 8000
#CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
