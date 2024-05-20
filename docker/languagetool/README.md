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
