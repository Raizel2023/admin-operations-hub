# Manual GitHub upload

1. Extract `cubework-operations-dashboard-source.zip` into a clean local folder.
2. Open your intended GitHub repository, `Raizel2023/cubework-operations-dashboard`.
3. Choose **Add file → Upload files**. Upload the extracted **root contents**, not the ZIP and not an extra enclosing folder. Keep `package.json`, `src`, `public`, and `prisma` at the repository root.
4. Enable hidden-file visibility in your file browser and explicitly include `.gitignore` and the blank `.env.example`. Never upload `.env.local`, another `.env` file, credentials, operational records, or downloaded workbooks. GitHub's web uploader does not use `.gitignore` as a safety filter.
5. Review the pending file list before committing. A large folder may require multiple web uploads because GitHub applies upload count/size limits. Preserve paths and upload all files, including all four calculator font files.
6. Commit the source upload manually. No GitHub credentials, tokens, or automatic push are needed by this archive.

GitHub repository storage alone does not run Next.js or host PostgreSQL. Follow `README.md` on your chosen host: configure secrets outside the repository, install locked dependencies, generate Prisma, initialize a **new** database, securely provision authorized access, build, and start on `0.0.0.0:3000`.

The sanitized historical migration files have different checksums from the original operating installation. Do not apply them to an existing migrated database without administrator-led reconciliation or baselining. This ZIP contains no operating database and creates no accounts automatically.
