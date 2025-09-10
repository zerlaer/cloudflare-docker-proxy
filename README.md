## 部署指南

> 详细教程 --> [基于 Cloudflare Workers 和 cloudflare-docker-proxy 搭建镜像加速服务](https://www.lixueduan.com/posts/docker/12-docker-mirror/)

1. Fork 本仓库

首先，你需要 Fork 本项目到你的 GitHub 账户。

2. 在 GitHub Settings 中配置 Secret

进入你 Fork 后的仓库，依次点击 Settings -> Secrets and variables -> Actions，然后点击 New repository secret 来添加以下必需的 Secret：
- CUSTOM_DOMAIN: 你的自定义域名，例如 lixd2.xyz。
- CF_API_TOKEN: 你的 Cloudflare API Token，此 Token 需要具备创建和管理 Workers 的权限。
- CF_ACCOUNT_ID: 你的 Cloudflare 账户 ID。你可以在 Cloudflare 控制台 Workers 界面的 URL 中找到它。

3. 提交代码以触发自动部署

完成 Secret 配置后，当你向仓库（例如 main 分支）推送代码或提交 Pull Request 时，GitHub Actions 会自动触发预置的部署流程。

4. 访问验证

部署完成后，请访问 https://docker.${CUSTOM_DOMAIN}（将 ${CUSTOM_DOMAIN} 替换为你实际配置的域名）。如果能够成功打开帮助界面，则说明部署成功。

希望这份指南能帮助你顺利完成部署！

# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy

1. click the "Deploy With Workers" button
2. follow the instructions to fork and deploy
3. update routes as you requirement

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

## Routes configuration tutorial

1. use cloudflare worker host: only support proxy one registry
   ```javascript
   const routes = {
     "${workername}.${username}.workers.dev/": "https://registry-1.docker.io",
   };
   ```
2. use custom domain: support proxy multiple registries route by host
   - host your domain DNS on cloudflare
   - add `A` record of xxx.example.com to `192.0.2.1`
   - deploy this project to cloudflare workers
   - add `xxx.example.com/*` to HTTP routes of workers
   - add more records and modify the config as you need
   ```javascript
   const routes = {
     "docker.libcuda.so": "https://registry-1.docker.io",
     "quay.libcuda.so": "https://quay.io",
     "gcr.libcuda.so": "https://k8s.gcr.io",
     "k8s-gcr.libcuda.so": "https://k8s.gcr.io",
     "ghcr.libcuda.so": "https://ghcr.io",
   };
   ```

