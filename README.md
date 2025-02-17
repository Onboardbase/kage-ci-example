# 🐳 CI/CD with Kage

Kage is a powerful CLI tool that transforms your web applications into self-hostable solutions in minutes. Here's how to set up CI/CD using Kage:

### Prerequisites
1. Install kage-cli globally:
   ```sh
   npm install -g kage-cli
   ```
2. Ensure you have Docker and Docker Compose installed
3. Create Docker Hub credentials and add them to your GitHub secrets as `DOCKER_USERNAME` and `DOCKER_PASSWORD`

### GitHub Actions Setup
Create a `.github/workflows/kage-build-and-push.yml` file with:

```yaml
name: Build and Push Docker Image Using Kage

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js (LTS)
        uses: actions/setup-node@v4
        with:
          node-version: "lts/*"

      - name: Install kage-cli
        run: npm install -g kage-cli@latest

      - name: Initialize kage
        - name: Initialize kage
        run: |
          # For static apps (client-side) like React, Vue, Vite, Astro SSG
          #kage init -n my-app -o dist -d yourdomain.com -e your@email.com
          
          # For server-side apps like Next.js, Astro SSR, NestJS, Express
          #kage init -n my-app -p 3000 -d yourdomain.com -e your@email.com

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Set up Docker Compose
        uses: KengoTODA/actions-setup-docker-compose@v1
        with:
            version: '2.14.2'

      - name: Build and push with kage
        run: |
          kage build \
            -t docker-hub \
            -a ${{ secrets.DOCKER_USERNAME }} \
            -i your-image-name \
            -v latest \
            --push
```

### Local Development with Kage
1. Initialize your project:
   ```sh
   # For static apps (client-side) like React, Vue, Vite, Astro SSG
   kage init -n my-app -o dist -d localhost -e your@email.com

   # For server-side apps like Next.js, Astro SSR, NestJS, Express
   kage init -n my-app -p 3000 -d localhost -e your@email.com
   ```
2. Build and run your container:
   ```sh
   kage run
   ```

For more information, visit the [kage-cli documentation](https://kagehq.com).