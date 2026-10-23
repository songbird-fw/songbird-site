# songbird-site

The website of the [Songbird](https://github.com/songbird-fw/songbird-fw) project, served at
**songbird-fw.org** (also `.com` and `.net`, with and without `www`).

A single static page, no build step, no framework. The site is `site/`; everything else is how
it gets onto the cluster.

```
site/                 index.html, 404.html, robots.txt, assets/ (logo, icons, self-hosted fonts)
deploy/               Kubernetes manifests: Namespace, Deployment (nginx-unprivileged), Service,
                      Ingress (Traefik + cert-manager), Traefik Middleware (http→https), nginx conf
kustomization.yaml    turns site/ into ConfigMaps and wires the manifests together
cluster/              one-off cluster prerequisites (Let's Encrypt ClusterIssuers)
design/               COMMIT-SHEET.md (the design decisions) and DESIGN.md (the style contract)
```

## Deploy

Prerequisites on the cluster (done once):

```bash
# cert-manager, then the Let's Encrypt issuers
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.21.2/cert-manager.yaml
kubectl apply -f cluster/clusterissuer-letsencrypt.yaml
```

The site itself, from the repository root:

```bash
kubectl apply -k .
kubectl -n songbird-site rollout status deploy/songbird-site
```

`kustomize` generates the ConfigMaps from `site/` with a content hash in the name, so changing a
file and re-applying rolls the pods; nothing is cached in the image. The Ingress carries the six
hostnames and asks cert-manager for one certificate (`songbird-fw-tls`) via HTTP-01 through Traefik.

Local preview, no cluster needed:

```bash
cd site && python3 -m http.server 8787
```

## How the pod is locked down

Non-root (uid 101), read-only root filesystem, all capabilities dropped, `RuntimeDefault` seccomp,
no service-account token, namespace under the `restricted` Pod Security Standard. nginx serves the
ConfigMaps read-only and writes only to a 32 MiB `emptyDir` on `/tmp`. Security headers (CSP,
nosniff, Referrer-Policy, Permissions-Policy, HSTS when the request arrived over TLS) are set in
`deploy/nginx/`.

## Editing the page

Read `design/DESIGN.md` first; it is the style contract (tokens, the two motion families, the
section patterns, and the list of things that were already tried and rejected). After an edit,
run the checks listed in its *Editing protocol*.

## License

AGPL-3.0, like the rest of the project.
