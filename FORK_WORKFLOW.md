# Open WebUI Fork Workflow

This fork is kept as a clean mirror of the upstream project until custom changes are needed.

## Goal

- Track upstream releases closely.
- Keep rollback simple.
- Avoid accidental feature branches or custom release tags unless there is a real fork-specific change.

## Update process

1. Fetch upstream changes.
2. Review the upstream release notes for breaking changes.
3. Merge or fast-forward the fork's `main` branch to the desired upstream release.
4. Build and push a new Docker image from the updated `main` branch.
5. Update the Docker Compose or Portainer stack to use the new image tag.
6. Restart Open WebUI and watch the logs for database migrations.

## Release model

- This fork does not rely on the GitHub Releases tab for normal updates.
- The Docker image tag in GHCR is the main release artifact.
- If a release workflow is disabled, that is expected; the fork can still be updated by syncing `main` and building a new image.

## Database safety

- Always make a database backup before upgrading.
- For this deployment, Open WebUI uses PostgreSQL through the `postgres` container.
- If a new version fails after startup, restore the database backup and roll back to the previous image tag.

## Rollback process

1. Stop the new container.
2. Revert to the previous image tag.
3. Restore the database backup if migration already ran and caused problems.
4. Start the previous version again.

## Fork rule

- Do not create fork-specific tags or branches unless there is an actual code change.
- If a future custom change is needed, create a separate feature branch from `main` and keep the change small.
