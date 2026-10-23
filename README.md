# Cubework Operations Dashboard

Sanitized source distribution of the existing Next.js Operations Dashboard: property audit/location tools, COI tracking and Excel exports, directory, templates, local daily EOD reports and exports, and the vendored v37 calculator. This archive contains application code and schema, not the operating database or its records.

## Requirements and setup

Use Node.js 22 LTS (or a compatible Node.js version supported by the locked dependencies), npm, and a separately hosted PostgreSQL database. Keep the included dependency versions and `package-lock.json`; no dependency upgrade is required.

1. Run `npm ci`.
2. Copy `.env.example` to `.env.local`. Supply your own `DATABASE_URL`, dashboard authentication secret (at least 32 random characters), and deployed origin. Keep `DASHBOARD_AUTH_ENFORCE=true`.
3. Run `npm run db:generate` to create `src/generated/prisma`. Generated client code is intentionally not distributed.
4. For a **new empty database only**, run `npm run db:migrate`. Read `docs/SANITIZATION.md` before using migrations on any existing installation.
5. Have an authorized operator securely provision the first active dashboard administrator in the new database, using an operator-reviewed process consistent with `prisma/schema.prisma` and the existing authentication code. No account, default password, provisioning script, or approved-editor allowlist is included. Do not disable authentication to work around initial setup. Dashboard sign-in alone does not grant EOD approved-editor access; that access must also be explicitly authorized.
6. Run `npm run lint`, `npm run typecheck`, and `npm run build`. The first build generates Next.js route types.
7. Run `npm run start -- -H 0.0.0.0 -p 3000`. The app listens on port **3000**. `npm run dev` is available for local development.

The database, uploaded documents, deployment environment variables and backups must be managed independently of GitHub. Existing settings such as external shortcut URLs are database-backed and are not transferred in this archive; configure them on the new installation. Provision persistent storage for `storage/property-documents`, or configure `PROPERTY_DOCUMENT_STORAGE_DIR`. Do not commit runtime content.

## Authentication and optional legacy integration

The existing dashboard sessions, administrator checks and EOD approved-editor policies remain in source. Category names such as ADMINISTRATOR are report categories, not authorization roles. Local daily EOD reports use PostgreSQL and do not require Google configuration.

Legacy Google editor sign-in requires your own OAuth configuration: `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `AUTH_SECRET` (at least 32 random characters), and `APP_BASE_URL`. Register the application's `/api/auth/callback/google` callback on your own origin. The optional legacy Sheet source uses `EOD_GOOGLE_SHEET_ID` and `EOD_GOOGLE_SHEET_GID`; refresh fails closed when unconfigured. Set `NEXT_PUBLIC_EOD_GOOGLE_SHEET_URL` only if you want a browser-visible source link. It is embedded during build and must not contain secrets. No previous private Sheet identifiers are included.

## Distribution notes

See `GITHUB-UPLOAD.md` for manual upload instructions and `docs/SANITIZATION.md` for archive changes, excluded tooling, and migration caveats. The private historical test collection and operational seed/provisioning scripts were intentionally omitted; this distribution does not advertise an `npm test` or seed command. Existing runtime modules remain included. Do not treat this source archive as a backup or an account/data transfer.
