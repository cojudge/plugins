# Cojudge plugin catalog

`index.json` is the list shown in Cojudge's **Plugins → Explore** tab and by
`cojudge plugin search`. `cojudge plugin install <id>` looks ids up here.

## Adding a plugin

Open a PR that adds an entry:

```json
{
  "id": "my-pack",
  "name": "My Pack",
  "description": "One or two sentences.",
  "source": "owner/repo",
  "version": "1.0.0",
  "problems": 42
}
```

- `id` must match the `id` in the repo's `cojudge-plugin.json`.
- `source` is a GitHub `owner/repo`. Cojudge installs the latest release,
  or the default branch if the repo has no releases.

How to write a pack: [Cojudge `docs/PLUGINS.md`](https://github.com/cojudge/cojudge/blob/main/docs/PLUGINS.md).

To point Cojudge at another catalog, set `COJUDGE_PLUGIN_INDEX` to a URL or a
local file path.
