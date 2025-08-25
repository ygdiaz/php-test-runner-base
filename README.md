# PHP Test Runner Base Image

A pre-built Docker image optimized for PHP testing environments, specifically designed for Laravel applications with comprehensive testing capabilities.

## 🚀 Features

- **PHP 8.1.33** with all necessary extensions
- **Composer** for dependency management
- **PHPUnit** for testing
- **PHPCS** for code standards
- **MySQL, Redis, MongoDB** client support
- **Multi-architecture support** (AMD64 + ARM64)
- **Optimized for CI/CD** pipelines

## 📦 Available Images

- `ghcr.io/ygdiaz/php-test-runner-base:latest` - Latest stable build
- `ghcr.io/ygdiaz/php-test-runner-base:main` - Latest from main branch
- `ghcr.io/ygdiaz/php-test-runner-base:<sha>` - Specific commit builds

## 🏗️ Building

The image is automatically built and pushed to GitHub Container Registry via GitHub Actions when:

- Code is pushed to `main`/`master` branch
- Pull requests are created (build only, no push)
- Manual workflow dispatch is triggered

### Manual Build

```bash
# Build locally (single architecture)
docker build -f Dockerfile.testing -t php-test-runner-base:local .

# Build multi-architecture (requires buildx)
docker buildx build --platform linux/amd64,linux/arm64 -f Dockerfile.testing -t php-test-runner-base:multi .
```

## 🧪 Usage

### In Docker Compose

```yaml
services:
  test-runner:
    image: ghcr.io/ygdiaz/php-test-runner-base:latest
    volumes:
      - .:/app
    working_dir: /app
    command: php artisan test
```

### In CI/CD Pipelines

#### Bitbucket Pipelines
```yaml
image: docker/compose:1.29.2
script:
  - docker pull ghcr.io/ygdiaz/php-test-runner-base:latest
  - docker-compose -f docker/docker-compose.testing.yml run --rm test-runner php artisan test
```

#### GitHub Actions
```yaml
- name: Run Tests
  run: |
    docker run --rm -v ${{ github.workspace }}:/app \
      ghcr.io/ygdiaz/php-test-runner-base:latest \
      php artisan test
```

## 🔧 Included Tools

- **PHP 8.1** with extensions: mysqli, pdo_mysql, redis, mongodb, gd, zip, etc.
- **Composer** - Latest stable version
- **PHPUnit** - Via Composer
- **PHPCS** - Code standards checking
- **Git** - For Composer dependencies
- **Curl, Wget** - For downloading dependencies
- **Unzip** - For archive extraction

## 📋 Environment Variables

The image supports standard Laravel environment variables:

- `APP_ENV=testing` (default)
- `APP_DEBUG=false` (default)
- Database connection variables
- Cache and session configuration

## 🏷️ Versioning

- **`latest`** - Always points to the most recent stable build
- **`main`** - Latest build from the main branch
- **`<branch>-<sha>`** - Specific commit builds for traceability

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes to `Dockerfile.testing`
4. Submit a pull request

The GitHub Actions workflow will automatically build and test your changes.

## 📄 License

This project is licensed under the MIT License.

## 🔗 Related Projects

- [planhub-api](https://github.com/ygdiaz/planhub-api) - The main application using this base image
