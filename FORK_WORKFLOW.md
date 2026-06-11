# Open WebUI Fork Workflow

This fork is kept as close to upstream as possible and only carries minimal Besitec-specific release process changes.

## Goal

- Track upstream releases closely.
- Keep rollback simple.
- Keep the fork delta small and easy to review.
- Use immutable Docker image tags for deployment.

## Update process

1. Fetch upstream changes.
2. Review upstream release notes for breaking changes and database migration warnings.
3. Create an upgrade branch from the desired upstream release tag.
4. Reapply only the intended fork-specific files.
5. Verify the fork diff is still minimal.
6. Build and push a versioned Docker image tag.
7. Update the Portainer stack to use the immutable image tag.
8. Restart Open WebUI and watch the logs for database migrations.

## Release model

- The Docker image tag in GHCR is the main release artifact.
- Portainer must use immutable image tags, never `:main`.
- Fork-built deployable images use Besitec-qualified tags such as `v0.9.6-besitec.1`.
- The GitHub Releases tab is optional; it is not the source of truth for deployment.

## Database safety

- Always make a database backup before upgrading.
- For schema-changing releases, rollback requires both the previous image tag and a pre-upgrade database restore.
- If a new version fails after startup, do not rely on image rollback alone if migrations already ran.

## Rollback process

1. Stop the new container.
2. Revert to the previous image tag.
3. Restore the pre-upgrade database backup if migration already ran.
4. Start the previous version again.

## Fork rule

- Keep fork-specific runtime changes to a minimum.
- Prefer rebuilding from an upstream release tag over merging long-lived fork drift.
- Do not deploy floating tags.
