Before hosting a website externally first make sure to:
- Choose an environment for hosting the Express app
- Check project settings
- Set up a production-level infrastructure for serving your website

# Production Environment

Environment provided by the server computer where you will run your website for external consumption. This environment includes:
- Computer hardware on which website runs
- OS (e.g. Linux, Windows)
- Programming language runtime and framework libraries on top of which your website is written
- Web server infrastructure, possibly including web server, reverse proxy, load balancer etc
- Databases on which your website is dependent

It is common to use a computer hosted "in the cloud" (remote computer). The remote server will usually offer some guatanteed level of computing resources (CPU, RAM, storage etc) and internet connectivity for a certain price.

This sort of remotely accessible computing/networking hardware is referred to as *Infrastructure as a Service* (*IaaS*). Depending on the vendor, the environment may come fully-featured, or may just come with an OS installed and the rest you will need to install.

Other hosting providors support Express as part of a *Platform as a Service* (*PaaS*). With this type of hosting most of the production environment (servers, load balancers etc) are taken care for you. This makes deployment easy as you don't need to focus on server infrastructure.

*IaaS* has more flexibility but *PaaS* is easier (reduced maintenance, easier scaling, easier to set up).

# Hosting provider

## Things to consider

- How busy your site is likely to be and cost of data/computing resources required to meet demand
- Level of support scaling horizontally (more machines) and vertically (upgrading to more power machines) and costs involved
- Location of data centres (where access is likely to be fastest)
- Hosts historic performance (uptime, downtime)
- Tools provided for managing site (easy to use, secure)
- Frameworks for monitoring
- Any limitations (limit on "live" hours in certain price tiers, small amount of storage, or certain services like emailing blocked)
- Benefits (free domain names, support for SSL certificates that usually have a cost)
- Additional costs (if free tier expires over time, or there is a cost of migrating)

## Popular choices

- Heroku - free but resource-limited *PaaS* environment
- Amazon Web Services
- Google Cloud
- Microsoft Azure

# Getting website ready to publish

## Development / in cod

### Set node version

Check the node version used during development (`node --version`) and set it in `package.json`

```json
"version": "0.0.0",
"engines": {
	"node": "14.17.1"
},
```

### Database configuration

Make sure the application gets database URI from the OS environment (so you can have a different database for production and development).

To get the connection string from an environment variable, use something like `process.env.MONGODB_URI`.

```js
// Set up mongoDB connection
const db_uri_dev = 'mongodb+srv://someuserandclusteretc';
const mongoDB = process.env.MONGODB_URI || db_uri_dev; // use dev uri if uri not set in environment
```

### Log appropriately

- Logging calls can have an impact on high traffic websites. This may be required but you should attempt to minimize the amount of logging for debugging purposes.
- One way to minimize "debug" logging is to use module `debug` to control what logging is performed by setting an environment variable.
```js
const debug = require('debug')('book');

debug('update error:' + err);
```
Environment variable:
```
#Windows
set DEBUG=author

#Linux
export DEBUG="author,book"
```

### Use gzip/defalte compression for responses

Web servers can compress the HTTP response sent back to a client, significantly reducing the time required for the client to get and load the page.

The method used will depend on the decompression methods the client says it supports (response will be uncompressed if none are supported).

for node you use package `compression`:

```js
var compression = require('compression');

const app = express();

app.use(compression()); //Compress all routes

// other middlewares and routes
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);

```

Note for a high traffic website, you wouldn't use this middleware. Instead, a reverse proxy like *nginx* would be ideal.

### Protection against well known vulnerabilities

The `helmet` middleware package can set appropraite HTTP headers to help protect your app from well-known web vulnerabilities.

```js
const compression = require('compression');
const helmet = require('helmet');

const app = express();

app.use(helmet());
...
```

## Environment / setup (ops)

### Set NODE_ENV to 'production'

By setting the `NODE_ENV` environment variable to `production` (default is `development`), we can remove stack traces in error pages. In addition, doing so also caches view templates and CSS files generated from CSS extensions. Setting `NODE_ENV` to production may improve app performance.

e.g. in heroku app
```bash
heroku config:set NODE_ENV='production'
```

### Set database uri 

e.g. in heroku app 
```bash
heroku config:set MONGODB_URI='mongodb+src://rest_of_uri_here'
```