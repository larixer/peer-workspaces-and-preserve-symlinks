## Workspace peer dependencies and --preserve-symlinks demo

This demo project shows how `--preserve-symlinks` Node flag lets the application use proper peer dependencies for workspaces

We have the following project layout:
```
.
├── ws1
│   └── dependencies
│       ├── button@workspace:*
│       └── is-number@6.0.0
├── ws2
│   └── dependencies
│       ├── button@workspace:*
│       └── is-number@7.0.0
└── button
    └── peerDependencies
        └── is-number@*
```

And we want that `button` workspace used correct `is-number` peer dependency version, which is different
in workspaces `ws1` and `ws2`. Given the fact that the project uses `node_modules` the `button` is
represented as a symlink in each of the workspaces. If Node.js is used with default flags, only one
version of `is-number` will be used in both workspaces, e.g. peer dependency contract will be violated,
this can be seen by running:
```
node ./index.js
```

But if we run:
```
node --preserve-symlinks ./index.js
```
The correct `is-number` version will be used by `button` in each of the workspaces.

