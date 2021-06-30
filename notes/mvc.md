Model View Controller

# Models

- Responsible for representing your data (memory, files, databases,...)
- Managing your data (save, fetch, etc)
- Contains data-related logic

# Views

- What user sees
- Shouldn't contain too much logic
- Decoupled from your application code

# Controllers

- Connecting Models and Views, so that the two can communicate (both directions)
- Contains the "in-between" logic
- (Express) Routes
- (Express) Split across middleware functions