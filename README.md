# Socket.dev Evaluation

## Setup

Install with socket.dev CLI

```
npm install -g socket
```

Verify installation using: `socket --version`
Result: `1.1.143` — installed successfully.

### Setup the token

Go to socket.dev and sign up.

After sign up, create an organization without GitHub (if you don't want to use the GitHub app).

![Create organization](screenshots/setup/01-create-org.png)

After the organization is created, you'll be redirected to the main dashboard.

![Dashboard](screenshots/setup/02-dashboard.png)

After that, click on the org name, go to Settings, and under Integrations go to API Tokens, then create a new API token.

![API tokens](screenshots/setup/03-api-tokens.png)

You'll have the following access (scope) options for that API token.

![Token scopes](screenshots/setup/04-token-scopes.png)

For experimentation purposes, you can select all scopes and proceed.

Copy the token value and save it in an environment file (`.env` — not committed to this repo).

Now authenticate the CLI using: `socket login`

It will ask for your token — paste it.

Setup is complete.

![Login complete](screenshots/setup/05-login-complete.png)

### Notes

- Note: `docs.socket.dev/docs/api-keys` (linked from the CLI prompt) currently 404s — use the
  main `socket.dev` dashboard instead to create tokens.
- The API token's quota shown on the token page (e.g. 500) is a per-token allocation, separate
  from the org's overall monthly plan quota (Free tier = 1,000 scans/month).

## Demo project

To generate real, comparable output from both `npm audit` and the Socket CLI, a throwaway
project was created at [`demo-project/`](demo-project/) with a deliberately mixed set of
dependencies — some with known CVEs, one (`node-sass`) that runs install/postinstall scripts,
and one modern clean package as a baseline:

```json
{
  "name": "socket-eval-demo",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "lodash": "4.17.15",
    "minimist": "0.0.8",
    "request": "2.88.0",
    "node-sass": "4.14.1",
    "axios": "^1.6.0"
  }
}
```

A lockfile was generated without installing anything (`npm i --package-lock-only`), then
`npm audit` was run against it.

## npm audit — CVE-only baseline

`npm audit` cross-references dependencies (including transitive ones) against the public npm
advisory database. It only flags packages with a **known, published CVE** — it has no concept
of install scripts, obfuscated code, or behavioral red flags. This is the baseline Socket is
being compared against.

<details>
<summary>Full <code>npm audit</code> output (19 vulnerabilities found)</summary>

```
PS C:\Users\game\Desktop\socket.dev\demo-project> npm audit

# npm audit report

cross-spawn <6.0.6
Severity: high
Regular Expression Denial of Service (ReDoS) in cross-spawn - https://github.com/advisories/GHSA-3xgq-45jj-v275
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/cross-spawn
node-sass >=1.2.0
Depends on vulnerable versions of cross-spawn
Depends on vulnerable versions of gaze
Depends on vulnerable versions of meow
Depends on vulnerable versions of node-gyp
Depends on vulnerable versions of request
Depends on vulnerable versions of sass-graph
node_modules/node-sass

form-data <=2.5.5
Severity: critical
form-data uses unsafe random function in form-data for choosing boundary - https://github.com/advisories/GHSA-fjxv-7rqg-78g4
form-data: CRLF injection in form-data via unescaped multipart field names and filenames - https://github.com/advisories/GHSA-hmw2-7cc7-3qxx
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/request/node_modules/form-data
request \*
Depends on vulnerable versions of form-data
Depends on vulnerable versions of qs
Depends on vulnerable versions of tough-cookie
Depends on vulnerable versions of uuid
node_modules/request
node-gyp <=10.3.1
Depends on vulnerable versions of request
Depends on vulnerable versions of semver
Depends on vulnerable versions of tar
node_modules/node-gyp

lodash <=4.17.23
Severity: high
Command Injection in lodash - https://github.com/advisories/GHSA-35jh-r3h4-6jhm
Prototype Pollution in lodash - https://github.com/advisories/GHSA-p6mc-m468-83gw
Regular Expression Denial of Service (ReDoS) in lodash - https://github.com/advisories/GHSA-29mw-wpgm-hmr9
lodash vulnerable to Code Injection via `_.template` imports key names - https://github.com/advisories/GHSA-r5fr-rjxr-66jc
lodash vulnerable to Prototype Pollution via array path bypass in `_.unset` and `_.omit` - https://github.com/advisories/GHSA-f23m-r3pf-42rh
Lodash has Prototype Pollution Vulnerability in `_.unset` and `_.omit` functions - https://github.com/advisories/GHSA-xxjr-mmjv-4gpg
fix available via `npm audit fix --force`
Will install lodash@4.18.1, which is outside the stated dependency range
node_modules/lodash

minimatch <=3.1.3
Severity: high
minimatch has a ReDoS via repeated wildcards with non-matching literal in pattern - https://github.com/advisories/GHSA-3ppc-4f35-3m26
minimatch has ReDoS: matchOne() combinatorial backtracking via multiple non-adjacent GLOBSTAR segments - https://github.com/advisories/GHSA-7r86-cg39-jmmj
minimatch ReDoS: nested _() extglobs generate catastrophically backtracking regular expressions - https://github.com/advisories/GHSA-23c5-xmqv-rm74
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/globule/node_modules/minimatch
globule _
Depends on vulnerable versions of minimatch
node_modules/globule
gaze >=0.4.0
Depends on vulnerable versions of globule
node_modules/gaze

minimist <=0.2.3
Severity: critical
Prototype Pollution in minimist - https://github.com/advisories/GHSA-vh95-rmgr-6w4m
Prototype Pollution in minimist - https://github.com/advisories/GHSA-xvch-5gv4-984h
fix available via `npm audit fix --force`
Will install minimist@1.2.8, which is a breaking change
node_modules/minimist

qs <6.14.1
Severity: moderate
qs's arrayLimit bypass in its bracket notation allows DoS via memory exhaustion - https://github.com/advisories/GHSA-6rw7-vpxm-498p
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/qs

scss-tokenizer <=0.4.2
Severity: high
Regular expression denial of service in scss-tokenizer - https://github.com/advisories/GHSA-7mwh-4pqv-wmr8
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/scss-tokenizer
sass-graph 2.2.0 - 4.0.0
Depends on vulnerable versions of scss-tokenizer
node_modules/sass-graph

semver 2.0.0-alpha - 5.7.1
Severity: high
semver vulnerable to Regular Expression Denial of Service - https://github.com/advisories/GHSA-c2qf-rxjj-qqgw
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/semver

tar <=7.5.15
Severity: high
Arbitrary File Creation/Overwrite due to insufficient absolute path sanitization - https://github.com/advisories/GHSA-3jfq-g458-7qm9
Arbitrary File Creation/Overwrite on Windows via insufficient relative path sanitization - https://github.com/advisories/GHSA-5955-9wpr-37jh
Denial of service while parsing a tar file due to lack of folders count validation - https://github.com/advisories/GHSA-f5x3-32g6-xq36
node-tar Vulnerable to Arbitrary File Creation/Overwrite via Hardlink Path Traversal - https://github.com/advisories/GHSA-34x7-hfp2-rc4v
node-tar is Vulnerable to Arbitrary File Overwrite and Symlink Poisoning via Insufficient Path Sanitization - https://github.com/advisories/GHSA-8qq5-rm4j-mr97
Arbitrary File Read/Write via Hardlink Target Escape Through Symlink Chain in node-tar Extraction - https://github.com/advisories/GHSA-83g3-92jg-28cx
tar has Hardlink Path Traversal via Drive-Relative Linkpath - https://github.com/advisories/GHSA-qffp-2rhf-9h96
node-tar Symlink Path Traversal via Drive-Relative Linkpath - https://github.com/advisories/GHSA-9ppj-qmqm-q256
Race Condition in node-tar Path Reservations via Unicode Ligature Collisions on macOS APFS - https://github.com/advisories/GHSA-r6q2-hw4h-h46w
node-tar applies PAX size override to intermediary GNU long-name/long-link headers, causing tar parser interpretation differential (file smuggling) - https://github.com/advisories/GHSA-vmf3-w455-68vh
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/tar

tough-cookie <4.1.3
Severity: moderate
tough-cookie Prototype Pollution vulnerability - https://github.com/advisories/GHSA-72xf-g2v4-qvf3
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/tough-cookie

trim-newlines <3.0.1
Severity: high
Uncontrolled Resource Consumption in trim-newlines - https://github.com/advisories/GHSA-7p7h-4mm5-852v
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/trim-newlines
meow 3.4.0 - 5.0.0
Depends on vulnerable versions of trim-newlines
node_modules/meow

uuid <11.1.1
Severity: moderate
uuid: Missing buffer bounds check in v3/v5/v6 when buf is provided - https://github.com/advisories/GHSA-w5hq-g745-h8pq
fix available via `npm audit fix --force`
Will install node-sass@9.0.0, which is a breaking change
node_modules/uuid

19 vulnerabilities (3 moderate, 13 high, 3 critical)

To address all issues (including breaking changes), run:
npm audit fix --force
```

</details>

**What this shows:** every finding here is a _known, published_ CVE on a specific version
range. Nothing here relates to install scripts, package behavior, or maintainer/publish
anomalies — that's the gap the Socket CLI scan (next section) is meant to fill.

Socket CLI Scanning:

## Socket CLI scan

From `demo-project`, run:

```
socket scan create .
```

This uploads the manifest and gives a link to a full report on the Socket dashboard.

**Result: 46 alerts total** (2 critical, 20 high, 24 medium/low) — vs. npm audit's 19,
CVE-only findings on the same manifest.

Critical and high priority alerts:
![Critical and high alerts](screenshots/cli-scan/01-alerts-critical-high.png)

Medium and low priority alerts:
![Medium and low alerts](screenshots/cli-scan/02-alerts-medium-low.png)

A couple of alerts here have no equivalent in npm audit at all — e.g. `json-schema` flagged
as **"90.0% likely obfuscated"** (a behavioral/static-analysis finding, not a CVE), and a
dedicated **"Deprecated by maintainer"** category. Full CLI vs. npm audit comparison table to
follow once Firewall is tested too.

## Socket Firewall (sfw)

Firewall is a **real-time install-time proxy**, not a report — the core distinction from the
CLI scan above. Setup:

```
npm i -g sfw
```

Then instead of `npm install`, prefix it:

```
sfw npm install
```

### Normal install (known CVEs, nothing actively malicious)

Run from `demo-project` (same manifest used for `npm audit` and the CLI scan):

```
PS C:\Users\game\Desktop\socket.dev\demo-project> sfw npm install
npm warn deprecated har-validator@5.1.5: this library is no longer supported
npm warn deprecated uuid@3.4.0: uuid@10 and below is no longer supported. For ESM codebases, update to uuid@latest. For CommonJS codebases, use uuid@11 (but be aware this version will likely be deprecated in 2028).
npm warn deprecated request@2.88.0: request has been deprecated, see https://github.com/request/request/issues/3142

added 74 packages, and audited 75 packages in 17s

10 packages are looking for funding
run `npm fund` for details

7 vulnerabilities (3 moderate, 1 high, 3 critical)

To address all issues possible (including breaking changes), run:
npm audit fix --force

Some issues need review, and may require choosing
a different dependency.

Run `npm audit` for details.
```

The install completed with only ordinary npm/CVE warnings — expected, since nothing in this
manifest is actively malicious, just outdated.

### Blocking test — installing packages Socket itself flags as live threats

To test whether Firewall actually blocks something malicious, we needed a real, currently
flagged package. `socket threat-feed` (CLI) returned **403 Forbidden** even with a
full-scope API token — the threat feed is gated behind a paid plan. The dashboard's **Threat
Intel → Campaigns** tab also only shows a limited set (2 campaigns, neither npm-ecosystem) with
an upsell banner to upgrade.

The **Threat Intel → Feed** tab, however, did show live, currently-detected npm packages (also
capped/limited on free tier, but enough for a test) — including `gleap@16.2.8`,
`@medusajs/telemetry@2.18.0-preview-20260717064230`, and `@lightdash/cli@0.3405.0`, all marked
"Live" and detected within hours of testing.

![Threat Intel Feed — live flagged packages](screenshots/firewall/01-threat-intel-feed.png)

**Safety note:** this test was run inside a disposable Docker container (`docker run -it --rm
node:20 bash`), never on a real machine, specifically because Firewall's own docs admit
free-tier gaps ("unknown package versions aren't blocked," "AI-detected malware warns but
doesn't block").

```
root@23c2ac6ede8a:/test# sfw npm install gleap@16.2.8
✔ downloading latest sfw binary...

added 21 packages, and audited 22 packages in 11s

2 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

```
root@23c2ac6ede8a:/test# sfw npm install @medusajs/telemetry@2.18.0-preview-20260717064230

added 67 packages, and audited 89 packages in 14s

20 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

A third package from the same live feed, `@lightdash/cli@0.3405.0`, was also tested to rule
out a fluke:

```
root@23c2ac6ede8a:/test# sfw npm install @lightdash/cli@0.3405.0
npm warn ERESOLVE overriding peer dependency
...
added 514 packages, and audited 603 packages in 5m

93 packages are looking for funding
  run `npm fund` for details

22 vulnerabilities (4 low, 11 moderate, 7 high)
```

Same result — installed cleanly, only ordinary npm peer-dependency/deprecation noise, no
`sfw`-originated warning or block.

**Verification — is the "flagged" version actually what got installed?** Before concluding
anything, we checked whether npm silently substituted a newer, already-patched version instead
of the exact flagged one, for all three packages:

```
root@23c2ac6ede8a:/test# cat node_modules/gleap/package.json | grep '"version"'
  "version": "16.2.8",
root@23c2ac6ede8a:/test# cat node_modules/@medusajs/telemetry/package.json | grep '"version"'
  "version": "2.18.0-preview-20260717064230",
root@23c2ac6ede8a:/test# cat node_modules/@lightdash/cli/package.json | grep '"version"'
  "version": "0.3405.0",
```

Confirmed for all three: the exact versions Socket's own Threat Intel feed lists as "Live" are
what actually landed in `node_modules`. This rules out "already patched" as an explanation.

**Three for three, with confirmed exact-version matches.** This is the single most important
finding of this evaluation: **the free-tier Firewall did not block, or even warn about,
packages Socket's own Threat Intel feed currently flags as actively malicious.**
