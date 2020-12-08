## Issue
### Description
`yalc push` won't update [host-yarn/yarn.lock](host-yarn/yarn.lock) or [host-npm/package-lock.json](host-npm/package-lock.json).

`yarn`/`npm i` won't do that either.

#### Repro Steps
```
# bootstrap
cd lib
yarn
npx yalc push
cd ..

cd host-npm
npm i
npx yalc add lib
cd ..

cd host-yarn
yarn
npx yalc add lib
cd ..

# modify lib's dependencies
cd lib
yarn add react@^17 # could be any package
npx yalc push
```
#### Desired Results
[host-yarn/yarn.lock](host-yarn/yarn.lock) and [host-npm/package-lock.json](host-npm/package-lock.json) show `react@17` has been installed in the hosts.

#### Actual Results
No changes to [host-yarn/yarn.lock](host-yarn/yarn.lock) and [host-npm/package-lock.json](host-npm/package-lock.json).

### Workarounds which don't Work
```
cd host-yarn
yarn
```
[host-yarn/yarn.lock](host-yarn/yarn.lock) doesn't change.

```
cd host-npm
npm i
```
[host-npm/package-lock.json](host-npm/package-lock.json) does say `lib` now depends on `react@^17` but it says it installed `react@15` anyway.

### Workaround which Works
```
# in lib
yarn add yalc@1.0.0-pre.35
npx yalc push
```
This runs `scripts.postupdate` from [lib/package.json](lib/package.json), which upgrades `lib` in the hosts.