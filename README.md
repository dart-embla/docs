### Embla Docs

The contents of this repository gets built into the [embla.io](https://embla.io) website.

The structure of this repo is as follows:

```
├── <each minor semver version like 1.2>
│   │
│   ├── introduction.md <—— Required, will be used as an "index" page of sorts.
│   ├── <any introductory documentation>.md
│   │
│   └── <category like "http" or "database">
│       │
│       └── <pages categorized under the above category>.md
│
└── README.md <—— This readme
```

Names are in URL-friendly "kebab-case": `0.2/http/writing-your-first-middleware.md`
