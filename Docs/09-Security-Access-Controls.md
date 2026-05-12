# Public Service Access Control

## Overview

This document explains how I protect services that are available through my public domain.

Some of my homelab services can be reached using subdomains, such as:

```text
homepage.example.com
nextcloud.example.com
portainer.example.com

These subdomains are routed through Traefik, which acts as my reverse proxy.

Since some services are public, I need to decide which ones need extra protection.

Why This Matters

If a service is on a public subdomain, people on the internet may be able to reach the login page.

Even if they cannot log in, it can still reveal information, such as:

What services I run
What tools I use
How my homelab is organized
Which dashboards are available

This is why I do not want every service exposed the same way.

Simple Rule

My basic rule is:

If it controls the homelab or shows the homelab layout, protect it more.

Some services are just normal apps with their own login page.

Other services are admin dashboards. These need stronger protection.

Access Control Types

I use a few different ways to control access.

1. App Login

Some services have their own login page.

Examples:

Nextcloud
Jellyfin
Vaultwarden
Portainer
Grafana
Uptime Kuma

This means the app itself asks for a username and password.

2. Traefik Basic Auth

Traefik Basic Auth adds a password before the user even reaches the app.

The flow looks like this:

Internet
   ↓
Traefik password prompt
   ↓
App login page
   ↓
Service

This gives the service an extra layer of protection.

3. Tailscale or Local Access Only

Some services are better kept private.

Instead of putting them on a public subdomain, I can access them through:

My local network
Tailscale VPN

This is usually better for very sensitive admin tools.

Service Decisions
Service	How I Plan to Protect It	Reason
Homepage	Traefik Basic Auth	It shows links and information about my homelab
Traefik Dashboard	Traefik Basic Auth	It manages routing for my services
Portainer	Tailscale only, or Basic Auth if public	It can manage Docker containers and stacks
Nextcloud	App login only	It is designed for browser, mobile, desktop sync, and file sharing
Vaultwarden	App login, strong password, 2FA	Extra auth may affect browser extensions or mobile apps
Uptime Kuma	App login + optional Basic Auth	It can show service names and uptime status
Grafana	App login + optional Basic Auth	It can show system and monitoring details
Jellyfin	App login only	Extra auth may make apps and streaming clients harder to use
Services That Need Extra Protection

These services should be protected more carefully:

Homepage
Traefik Dashboard
Portainer
Uptime Kuma
Grafana

These are important because they either show information about the homelab or control parts of the homelab.

Services That Can Usually Use Their Own Login

These services usually work better with their own login page only:

Nextcloud
Vaultwarden
Jellyfin

The reason is that these apps may use:

Mobile apps
Desktop apps
Browser extensions
Sync clients
Sharing links
APIs

Adding Traefik Basic Auth in front of them could make those features harder to use.

Example: Homepage

Homepage should have extra protection.

Even though it may not store passwords, it can show:

Service names
Dashboard links
Monitoring widgets
Internal tools
Homelab layout

Because of that, I do not want it open to anyone on the internet.

Example: Nextcloud

Nextcloud is different.

Nextcloud is meant to be used directly with its own login page.

It can also connect with:

Desktop sync clients
Mobile apps
WebDAV
File sharing links
Calendar and contact apps

Because of that, I do not plan to put Traefik Basic Auth in front of Nextcloud.

Instead, I will protect Nextcloud with:

HTTPS through Traefik
Strong passwords
Two-factor authentication when possible
Trusted domains
Regular updates
Example: Portainer

Portainer is a Docker management dashboard.

If someone gets into Portainer, they may be able to:

View containers
Restart containers
Edit stacks
See environment variables
Change services

Because of that, Portainer should not be exposed casually.

The safer option is to keep Portainer available only through:

Local network
Tailscale

If I ever expose it publicly, I should use both:

Traefik Basic Auth + Portainer login
Before Exposing a Service

Before I put a service on a public subdomain, I should ask:

Does this service have its own login page?
Does it show private homelab information?
Can it control Docker or infrastructure?
Does it need mobile apps or sync clients?
Would Traefik Basic Auth break anything?
Could I access it through Tailscale instead?