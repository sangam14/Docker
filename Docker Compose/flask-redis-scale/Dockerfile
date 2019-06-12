ARG PYTHON_VERSION
FROM python:$PYTHON_VERSION
WORKDIR /usr/src/app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
ENV FLASK_APP=app.py
EXPOSE 3000
CMD flask run --host=0.0.0.0
