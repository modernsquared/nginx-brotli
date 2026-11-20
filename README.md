# nginx-brotli

Prebuilt dynamic Brotli modules for nginx (`ngx_http_brotli_filter_module.so`, `ngx_http_brotli_static_module.so`).

**A dynamic module only loads into the exact nginx version it was built for.** One tag per nginx release:

| tag | built for | note |
|---|---|---|
| `nginx-1.26.2` | nginx 1.26.2 | the original prebuilt modules (Dec 2024) |
| `nginx-1.30.7` | nginx 1.30.7 | rebuilt 2026-11-20 (CVE-2026-123007 / CVE-2026-123633), ngx_brotli `ngx_brotli.rev` |

`main` always equals the newest tag. `NGINX_VERSION` holds the version the `.so` files were built for; the Ansible playbooks (`vsf.yml`, `strapi.yml`, `strapi5.yml`) clone tag `nginx-<nginx_version>` and refuse to install if `NGINX_VERSION` does not match.

## Bump to a new nginx version
1. `docker build --build-arg NGINX_VERSION=<ver> -f Dockerfile.build -t brotli-build .` (installs the nginx.org package, builds against the same configure arguments; on a machine that re-signs TLS add its root CA first).
2. Copy `/out/*` from the image into this repo, set `NGINX_VERSION`, commit, tag `nginx-<ver>`, push main and the tag.
3. Only then change `nginx_version` in the playbooks and `apt install nginx=<ver>`.
