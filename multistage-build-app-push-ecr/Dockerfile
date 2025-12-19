#stage 1 <------------>
FROM python:3 as builder  
WORKDIR /app
#just copy only requirments files not all
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
#prefix mean just install for requiremnts.txt file or docker 
#container not global system
#>>>>>>>>>>stage2<<<<<<<<<<<

FROM python:3.10-slim 
WORKDIR /app

#not delay output check log fast
ENV PYTHONUNBUFFERED=1 
# copy from builder pyton3 image all files copy
COPY --from=builder /install /usr/local
COPY . .
EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
#RUN pip install -r requirements.txt
#RUN python manage.py makemigrations
#RUN python manage.py migrate
#EXPOSE 8000
#CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
