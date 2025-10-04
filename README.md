## Reverse Proxies Can Be a Headache
If you’ve ever configured Nginx or Apache as a reverse proxy, you probably know this pain:
* Endless config files
* Certbot scripts that randomly fail
* HTTPS that silently breaks
* And weird edge cases that eat hours of your time
I’ve been there.

In 2025, I stopped fighting it. I switched to [Caddy](https://www.youtube.com/@hugovalters), and I’m never looking back.

## What Is Caddy?
**Caddy** is a modern, open-source web server with automatic **HTTPS**, built-in reverse proxy support, and a dead-simple config syntax. No cronjobs. No shell scripts. No fighting with Let’s Encrypt. Just one binary. One config file. Everything works. You can use Caddy to:
* Serve static sites or apps
* Reverse proxy to Docker containers or external servers
* Automatically get and renew TLS certs
* Manage access control
* Secure internal dashboards with _tailscale server_ or local auth

## Why I Switched from Nginx?
Let me break it down:
_**Nginx**_:
* HTTPS setup = Manual (Certbot etc.)
* Config format = XML-ish
* Docker support = External scripts
* Live config reloads = Manual signal
* Metrics and logging = External tooling
* Learning curve = Steep

**_Caddy:_**
* HTTPS setup = Automatic
* Config format = Human-readable
* Docker support = Native plugin support
* Live config reloads = Built-in
* Metrics and logging = Built-in
* Learning curve = Friendly

Caddy’s config is so simple, I memorized parts of it. Here’s a working reverse proxy example:
```
mydomain.com {
  reverse_proxy localhost:3000
}
```

That’s it. TLS is automatic. Cert renewals are automatic. It even detects if a site is down and retries.

## How I Use Caddy in Production
* I run several services, both public and internal:
* A Rancher dashboard example: (**rc.valtersit.com**)
* WordPress with MariaDB via Docker
* Prometheus + Grafana monitoring
* Self-hosted AI tools
* Internal admin panels on VPS and home devices

## With Caddy:
* I don’t write shell scripts to update certs
* Everything is HTTPS by default
* Config reloads instantly with no downtime
* I can route multiple domains and subdomains from one file

And since I use Tailscale, I even expose services securely using internal _.ts.net_ addresses or local LAN IPs.

## Bonus: Docker + Caddy + Auto-HTTPS
My favorite combo:
* Docker Compose runs services (e.g. Nextcloud, WordPress)
* Caddy reverse proxies everything
* Caddy-docker-proxy plugin reads container labels to auto-generate configs

```
labels:
  caddy: myapp.example.com
  caddy.reverse_proxy: "{{upstreams 80}}"
```
Caddy sees it, creates the proxy rule, gets the cert — done. You can even plug this into GitHub Actions or Ansible and **fully automate deployments.**

## Who Should Use Caddy?
Caddy is perfect if you:
* Want HTTPS without touching Certbot
* Need to expose internal tools (Rancher, Portainer, Grafana, etc.)
* Are tired of rewriting nginx.conf every week
* Want to automate deployments and configs

It’s great for:
* Solo developers
* Small startups
* Homelab nerds
* Freelancers running client projects
* Teams building microservices with Docker/K8s

## Coming Soon: Video + Setup Guide
If this sounds like something you want to try, stay tuned. In my upcoming [YouTube video](https://www.youtube.com/@hugovalters) and [Medium tutorial](https://blog.valters.eu), I’ll walk you through:
1. Installing Caddy on a Hetzner VPS
2. Setting up auto-HTTPS with Cloudflare DNS
3. Using Caddy with Docker for WordPress & MariaDB
4. Combining Caddy + Rancher + Tailscale
5. Creating a full CI/CD pipeline with auto-reverse proxy

Follow me to get notified when it’s live — and yes, configs and playbooks will be included.

##Final Thoughts
I’ve used a lot of tools. Some were powerful, others were painful.
Caddy is the first one that felt like **it was built for me**. It’s not flashy. It doesn’t try to be everything. But what it does, it does **exceptionally well**. If you value **simplicity, security, and automation**, give Caddy a try.
Your future self will thank you.

X.com: [https://x.com/hugovalters](https://x.com/hugovalters)
bsky.app: [https://bsky.app/profile/hugovalters.bsky.social](https://bsky.app/profile/hugovalters.bsky.social)
YouTube: [https://www.youtube.com/@hugovalters](https://www.youtube.com/@hugovalters)
Homepage: [https://www.valters.eu](https://www.valters.eu)
GitHub: [https://github.com/hugovalters](https://github.com/hugovalters)
Medium: [https://blog.valters.eu](https://blog.valters.eu)

By Hugo Valters
