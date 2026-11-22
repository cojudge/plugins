# Cojudge problem packs

Problem packs (plugins) add problems and courses to
[Cojudge](https://github.com/cojudge/cojudge). A pack is just a GitHub repo,
public or private. This repo holds the catalog of public packs (`index.json`),
which Cojudge shows on its **Plugins → Explore** page and in
`cojudge plugin search`.

## Official packs

| Pack | Install | Problems |
|---|---|---|
| [Basic 50](https://github.com/cojudge/basic-50) | `cojudge plugin install basic-50` | 50 beginner problems: if/else, loops, arrays, strings, recursion |
| [NeetCode 250](https://github.com/cojudge/neetcode-250) | `cojudge plugin install neetcode-250` | NeetCode 150 plus 100 more |
| [LeetCode 1000](https://github.com/cojudge/leetcode-1000) | `cojudge plugin install leetcode-1000` | 950-problem archive grouped by pattern |

You can also install from the **Plugins** page in Cojudge (plug icon next to
the Playground button). Installed packs work offline.

```bash
cojudge plugin list                 # what's installed
cojudge plugin disable <id>         # hide a pack, keep its files
cojudge plugin enable <id>
cojudge plugin upgrade [id]         # update to the latest release
cojudge plugin uninstall <id>       # your progress is kept
```

## Create your own problem set

Any problems you write can become a pack: for yourself, your class, your
team, or everyone.

### 1. Lay out a repo

```
my-pack/
├── cojudge-plugin.json
├── courses/
│   └── my-course/
│       └── courseinfo.json
└── problems/
    ├── two-sum-variant/
    │   ├── statement.md
    │   ├── metadata.json
    │   ├── official-tests.json
    │   ├── Marker.java
    │   └── solution.md          (optional)
    └── ...
```

`cojudge-plugin.json`:

```json
{
  "id": "my-pack",
  "name": "My Pack",
  "version": "1.0.0",
  "description": "One or two sentences about the problems."
}
```

- `id`: lowercase letters, digits and dashes.
- `version`: semver; bump it on every release.

`courses/my-course/courseinfo.json` lists the problems in each category, in
order:

```json
{
  "title": "My Course",
  "description": "What this course covers.",
  "category-order": ["warm-up", "arrays"],
  "problems-of-category": {
    "warm-up": ["two-sum-variant"],
    "arrays": ["rotate-grid", "two-sum"]
  }
}
```

A plain slug means your pack's own problem if the pack has one; otherwise it
means the built-in Cojudge problem with that slug. In the example above,
`two-sum` reuses Cojudge's own Two Sum, so you don't need to copy it.

### 2. Write the problems

Each problem folder has the same files as a built-in Cojudge problem. The
format (statement, function signature, test cases, reference solution and
checker) is in
[`docs/ADD_PROBLEMS.md`](https://github.com/cojudge/cojudge/blob/main/docs/ADD_PROBLEMS.md).
The easiest start is to copy a problem from
[Basic 50](https://github.com/cojudge/basic-50/tree/main/problems) and edit it.

### 3. Try it locally

```bash
cojudge plugin install ./my-pack                     # install from the folder
cojudge init two-sum-variant --lang python           # starter code
cojudge run two-sum-variant solution.py              # sample tests
cojudge submit two-sum-variant solution.py           # all tests
```

Run `cojudge plugin install ./my-pack` again after edits, then refresh the
browser.

Pack problems get ids like `my-pack/two-sum-variant`, so they never clash with
other packs that use the same slug. In the CLI you can type the bare slug as
long as only one installed pack has it.

### 4. Publish on GitHub

Push the folder to a GitHub repo and create a release (for example `v1.0.0`).
Cojudge installs the latest release; if the repo has no releases, it installs
the default branch.

```bash
cojudge plugin install your-name/my-pack
```

**Public repo:** anyone can install it with the command above.

**Private repo:** works the same for anyone with access to the repo, once
they're logged in to GitHub on that machine:

```bash
gh auth login                       # once; or set GH_TOKEN / GITHUB_TOKEN
cojudge plugin install your-org/private-pack
```

The Plugins page works too, as long as the token is available to the machine
running the Cojudge server. Without credentials, GitHub reports a private repo
as "not found", and Cojudge tells you to log in.

To ship an update, bump `version` in `cojudge-plugin.json`, create a new
release, and run `cojudge plugin upgrade my-pack`.

### 5. List it in the catalog (public packs)

To have your pack show up in **Plugins → Explore** and install by id
(`cojudge plugin install my-pack`), open a PR here that adds an entry to
`index.json`:

```json
{
  "id": "my-pack",
  "name": "My Pack",
  "description": "One or two sentences.",
  "source": "your-name/my-pack",
  "version": "1.0.0",
  "problems": 42
}
```

`id` must match `cojudge-plugin.json`. Private packs don't need a catalog
entry; share the `owner/repo` with your team instead.

## More

- Full reference (id rules, lookup order, editing installed problems):
  [`docs/PLUGINS.md`](https://github.com/cojudge/cojudge/blob/main/docs/PLUGINS.md)
- To use a different catalog, set `COJUDGE_PLUGIN_INDEX` to a URL or a local
  file path.
