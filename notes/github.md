# Github Pages

## Deployment for React App

- `npm install gh-pages --save-dev`
- Add these two lines to `package.json` in `scripts`:
```
"predeploy": "npm run build",
"deploy": "gh-pages -d build",
```
- Update `homepage` in `package.json` to e.g. `https://github.com/JFY96/blog-app`
- Set `output.publicPath` to `./`
	- if using dotenv for development, may need something like `publicPath: env.ENVIRONMENT === 'development' ? '/' : './'`
- Run npm run deploy
	- This will create or update a `gh-pages` branch with the built application

# GitHub Actions

