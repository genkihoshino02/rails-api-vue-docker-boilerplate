# Rails(API) + Vue.js

This is the template repository for preparing for the environment of Rails(API) + Vue.js using Docker.

# tech stack

1. Backend (Rails API):

- Uses Ruby on Rails in API mode.
- Responsible for interacting with the database, handling business logic, and providing data.
- Sends data to the frontend in JSON format.


2. Frontend (Vue.js):

- A JavaScript framework that runs in the browser.
- Responsible for building and managing the user interface.
- Fetches data from the API and displays it on the screen.
- Updates only the necessary parts of the screen in response to user actions.


1. SPA (Single Page Application) :

- After the initial page load, it operates without reloading the entire page.
- Asynchronously fetches only the required data from the API and updates the screen.
- Improves user experience and achieves behavior similar to native applications.


4. Communication flow:

- In response to user actions, Vue.js sends HTTP requests to the Rails API.
- The Rails API processes the request and returns JSON data as a response.
- Vue.js receives the response and updates the screen as necessary.

# What you need to do
## setup rails (API)
move to backend directory.
```
cd backend
```
create the rails project with API mode.
```
docker-compose run api rails new . --force --no-deps -d mysql --api
```
you might encounter the permission error if you are Mac user, then change the permission like this.
```
sudo chown -R $(whoami):staff /Users/user/dev/rails_api_template/backend/vendor/bundle
```
After the rails project is created, edit the `config/database.yml`.
```yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password # edit here
  host: db           # edit here
```
Build the docker container.
```
docker-compose build
docker-compose up -d
```
check the status of docker container.
there might be some problem if two of container is not up.
```cmd
backend % docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED         STATUS         PORTS                                       NAMES
9ea1b2a1eae7   backend-api    "entrypoint.sh bash …"   3 seconds ago   Up 2 seconds   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   backend-api-1
f01a3a38e900   mariadb:10.7   "docker-entrypoint.s…"   34 hours ago    Up 6 minutes   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp   backend-db-1
```
create the database.
```
docker-compose run api bundle exec rails db:create
```

### configure CORS
comment out `'rack-cors'` in Gemfile and install it.
```
gem 'rack-cors'
```
```
docker-compose build
```

comment out following code in backend/config/initializers/cors.rb
```rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'http://localhost:8080'

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

### setup Vue.js
move to frontend directory
```
cd frontend
```

setup the Vue.js project
```
vue create .
```
