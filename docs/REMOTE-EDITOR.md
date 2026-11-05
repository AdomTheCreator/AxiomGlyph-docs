# Remote editor on a VPS or dedicated server

The Minecraft port and editor port are separate. `/ag editor` uses `web-editor.public-url` when configured. Session links authorize editing for 30 minutes; keep them private. No public ports are opened automatically by the plugin.

## Direct HTTPS: expose one editor port

Install a trusted certificate matching your public editor hostname. Export the certificate chain and private key as a PKCS#12 file:

```sh
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out editor.p12 -name axiomglyph
```

Place the file in `plugins/AxiomGlyph/`. Set the keystore password in the Minecraft process environment as `AXIOMGLYPH_TLS_PASSWORD`. Do not commit the certificate private key or password to Git. Use the same password for the PKCS#12 store and its key.

```yaml
web-editor:
  enabled: true
  host: 0.0.0.0
  port: 8765
  public-url: https://editor.example.com:8765
  tls:
    enabled: true
    keystore: editor.p12
    password-env: AXIOMGLYPH_TLS_PASSWORD
```

Point the hostname at the VPS IP, allow TCP 8765 in the operating system firewall and hosting provider firewall, and restart Minecraft. On container hosting, also allocate/publish this TCP port. Run `/ag editor`; the link can now be opened on another computer. For IPv6 use an appropriate bind address and IPv6 firewall rule. A self-signed test certificate is not a substitute for a trusted public certificate.

## HTTPS reverse proxy

If an HTTPS proxy runs on the same machine, keep the plugin local and let the proxy manage certificates:

```yaml
web-editor:
  enabled: true
  host: 127.0.0.1
  port: 8765
  public-url: https://editor.example.com
  tls:
    enabled: false
```

Example Caddy configuration:

```caddyfile
editor.example.com {
  reverse_proxy 127.0.0.1:8765
}
```

Expose the proxy HTTPS port (normally TCP 443), not the backend port. Follow the proxy's certificate issuance requirements. Forward the public Host header, Authorization header, request body, and all `/api/` routes. Do not cache API responses. When proxy and Minecraft run in different containers, adjust their private addresses accordingly.

## Local access

Leave `host: 127.0.0.1`, `public-url: ""`, and `tls.enabled: false` for local use. Listener/certificate changes require a restart. Translation reloads do not restart the web listener. Missing keys, an incorrect password, or an occupied port prevent the editor from starting and are reported in the Minecraft log.

The plugin cannot provision a domain, certificate, VPS firewall, port allocation, or NAT mapping on your behalf. The tests verify local HTTPS, session authentication and origin checks; public reachability depends on your deployment.
