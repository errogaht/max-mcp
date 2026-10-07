# Security review for Payments adoption

Reviewed upstream commit 0485269e3fa7fc1d9dce00ac8005b255402951a0 and the
maxapi-python 2.1.2 wheel. No third-party credential exfiltration, subprocess
execution, dynamic eval/exec or hidden installer was found in these sources.
This is a scoped source review, not a security guarantee of MAX or all dependencies.

PyMax uses TLS to ws-api.oneme.ru with web.max.ru Origin. QR/password login and
SQLite session storage are local to the operator. Provider telemetry is disabled.
DEBUG request/event frames can contain credentials and message content; this fork
uses CRITICAL logging. Pin the reviewed SDK version before accepting an upgrade.

The upstream server is a single-user local stdio tool, not a multi-tenant public
account service. Payments uses the same pinned SDK inside its private existing
account gateway, with per-account session directories, browser QR/password
callbacks, administrator/CSRF controls, durable incoming events and send receipts.
Do not expose stdio tools or credential/session files to public HTTP clients.
