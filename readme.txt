apt-get install python3-venv nginx
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# systemd service
cp ./app_flask.service /etc/systemd/system/app_flask.service

nginx: 
location / { try_files $uri @app; }
    location @app {
    include uwsgi_params;
    uwsgi_pass 127.0.0.1:3031;
}

# init database
flask db upgrade

start debug server
export FLASK_APP=microblog.py
export FLASK_ENV=development
export FLASK_DEBUG=1
flask run
## or
uwsgi uwsgi.ini

add new language:
flask translate init <language-code>
update after add market _l _():
flask translate update
compile after translate messages.mo:
flask translate compile