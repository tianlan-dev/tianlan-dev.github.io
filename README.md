# skylan.dev

Personal homepage repository for Tian Lan. This is a static website served from
the repository root and published through GitHub Pages.

## 运行

This site does not require a build step. Install dependencies once, then use
npm to start the static server from the repository root.

```bash
npm install
```

Runtime startup configuration lives in two Git-ignored environment files:

- `.env.production.local`: production Docker startup configuration.
- `.env.development.local`: local development startup configuration.

Both files must define `APP_HOST` and `APP_PORT`.

### Production in Docker

Create a local production environment file first. This file is ignored by Git:

```bash
cat > .env.production.local <<'EOF'
APP_HOST=0.0.0.0
APP_PORT=<production-port>
EOF
```

Then build and run the production container:

```bash
./production-docker.sh
```

### Local Development

Create a local development environment file first. This file is ignored by Git:

```bash
cat > .env.development.local <<'EOF'
APP_HOST=0.0.0.0
APP_PORT=<local-dev-port>
EOF
```

```bash
./development-server.sh
```

The development script reads `.env.development.local`, stops any old process
listening on the same port, then runs `npm run start` in the background. The
host and port are provided by the local environment file.

## Package scripts

```bash
npm run start
```

- `npm run start`: 从仓库根目录启动静态文件服务器。需要通过环境变量提供 `APP_PORT`，可选提供 `APP_HOST`。

## Startup scripts

- `./production-docker.sh [start|stop|restart]`: 读取 `.env.production.local`，构建 Docker image，并替换同名 production container。
- `./development-server.sh [start|stop|restart]`: 读取 `.env.development.local`，停止占用 development 端口的旧进程，然后在后台运行 `npm run start`。

Script logs are written to `.server-logs/`, which is ignored by Git.

## Project Structure

```text
.
├── index.html        # Homepage
├── css/              # Site styles
├── js/               # Site scripts
├── img/              # Images
├── fonts/            # Web fonts
├── resume/           # Resume page
└── old/              # Archived previous page
```

## Development Notes

- Edit `index.html` for homepage content.
- Edit files in `css/` for styling.
- Edit files in `js/` for client-side behavior.
- Keep links relative where possible so the site works both locally and on
  GitHub Pages.

## Deployment

GitHub Pages serves this repository as a static site. After changes are merged
or pushed to the publishing branch configured in GitHub Pages, the live site
will update automatically.
