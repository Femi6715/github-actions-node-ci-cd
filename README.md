# GitHub Actions CI/CD for a Node.js Service

A CI/CD pipeline for a small Node.js service. Every push and pull request runs the tests. Pushes to `main` that pass the tests build a Docker image and publish it to GitHub Container Registry (GHCR).

## Pipeline

Defined in `.github/workflows/ci-cd.yml`:

| Job | Runs on | Steps |
|---|---|---|
| `build-test` | Every push and pull request to `main` | Check out the code, set up Node.js 20, install dependencies, run the Mocha tests |
| `docker-publish` | Pushes to `main` only, after `build-test` passes | Log in to GHCR with the built-in `GITHUB_TOKEN`, build the image from `app/`, push `ghcr.io/<owner>/node-app:latest` |

The publish job only gets `packages: write` permission, and no extra secrets are needed.

## The app

`app/index.js` is a minimal HTTP server on port 3000 that returns JSON with the service name and request path. `app/test.js` holds the Mocha test.

## Run locally

```bash
cd app
npm install
npm test
npm start
```

With Docker:

```bash
docker build -t node-app app
docker run -p 3000:3000 node-app
curl http://localhost:3000
```

Pull the published image:

```bash
docker pull ghcr.io/femi6715/node-app:latest
```
