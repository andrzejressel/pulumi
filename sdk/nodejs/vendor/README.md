# Vendor

This directory contains vendored versions of [Typescript 3.8.3](https://github.com/microsoft/TypeScript/tree/v3.8.3) and [ts-node](https://github.com/TypeStrong/ts-node/tree/v7.0.1).

These are the default and minimum versions we support for these packages.

Historically these packages were direct dependencies of `@pulumi/pulumi`. To decouple the node SDK from the precise version of TypeScript, the packages are now declared as peerDependencies of `@pulumi/pulumi` and customers can pick the versions they want. Unfortunately automatic installation of peer dependencies depends on the package manager and its version. For example npm 7+ automatically install peer dependencies, but yarn classic never does.

In case the peer dependencies are not installed, we fall back to these vendored versions.

## TypeScript

To vendor typescript:

```bash
cd sdks/nodejs/vendor
curl -L -o typescript-3.8.3.tgz https://registry.npmjs.org/typescript/-/typescript-3.8.3.tgz
tar xvf typescript-3.8.3.tgz
rsync package/LICENSE.txt package/CopyrightNotice.txt package/ThirdPartyNoticeText.txt package/lib/typescript.js typescript@3.8.3/
rsync package/lib/*.d.ts typescript@3.8.3/
rm -rf package
rm typescript-3.8.3.tgz
```

To vendor ts-node:

```bash
cd sdks/nodejs/vendor
curl -L -o ts-node-7.0.1.tgz https://registry.npmjs.org/ts-node/-/ts-node-7.0.1.tgz
tar xvf ts-node-7.0.1.tgz
cd package
npm install --omit=dev --no-package-lock --no-bin-links --ignore-scripts
cd ..
rsync -r --exclude="*.map" --exclude="*.d.ts" --exclude="*/source-map/dist/*" package/ ts-node@7.0.1/
rm -rf package
rm ts-node-7.0.1.tgz
```
