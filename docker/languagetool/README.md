### About

Company policies sometimes prohibit the use of third-party services. As an alternative, a local LanguageTool server is provided. For convenience, the entire setup is packaged using Docker Compose.

### Install

1. Download `ngrams` and place them in the `ngrams` directory:
   ```sh
   wget https://languagetool.org/download/ngram-data/ngrams-en-20150817.zip
   unzip ngrams-en-20150817.zip -d ./ngrams
   rm ngrams-en-20150817.zip
   ```
2. Generate local SSL certs:
   ```sh
   mkcert -install
   mkdir certs
   mkcert -cert-file ./certs/server.crt -key-file ./certs/server.key localhost
   ```
3. Run docker compose
   ```sh
   docker compose up -d
   ```
4. Connect LanguageTool Chrome plugin or LanguageTool VSCode extension to `https://localhost:8011`

### Findings

- The `languagetool` container runs unprivileged as uid/gid `783:783` (`user: "783:783"` in compose). If `./ngrams`/`./fasttext` aren't already owned by `783:783` on the host, it fails to start with `directory "/ngrams" is owned by 0:0, but should be owned by uid 783 and/or gid 783`. Fix: `sudo chown -R 783:783 ./ngrams ./fasttext`.
- This stack requires real Docker. Apple's `container` CLI works for single containers, but `container-compose` (the compose shim) doesn't implement `tmpfs`/`cap_drop`, and containers can't resolve each other by name (bare hostname or `container system dns` domains both fail to resolve) — Caddy can't reach `languagetool` without extra host-published-port + gateway-IP workarounds.
