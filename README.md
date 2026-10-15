# Gunsmoke Bounty: Android app (Google Play)

Same route as The Ledger Life: host as a PWA on GitHub Pages, wrap it with Bubblewrap (TWA), sell packs with Play Billing.

## 1. Host the game
1. Create the repo `Gunsmoke-Bounty` under rizasx. Put these files in the branch that GitHub Pages deploys from (gh-pages or /docs, not main root unless Pages is set to main):
   `index.html, manifest.webmanifest, sw.js, privacy.html, icons/`
2. Check https://rizasx.github.io/Gunsmoke-Bounty/ opens the game.
3. `https://rizasx.github.io/.well-known/assetlinks.json` is shared by all your apps. It must be ONE JSON array with a separate object per app:
   ```json
   [
    {"relation":["delegate_permission/common.handle_all_urls"],"target":{"namespace":"android_app","package_name":"com.ledgerlife","sha256_cert_fingerprints":["<Ledger Play app signing SHA-256>"]}},
    {"relation":["delegate_permission/common.handle_all_urls"],"target":{"namespace":"android_app","package_name":"com.aethelgard.gunsmokebounty","sha256_cert_fingerprints":["<Gunsmoke Play app signing SHA-256>"]}}
   ]
   ```
   Use the **App signing key certificate** SHA-256 from Play Console > Setup > App signing (not your upload key), or purchases will fail again.

## 2. Build the Android bundle (PowerShell)
```
npm i -g @bubblewrap/cli
mkdir gunsmoke; cd gunsmoke
bubblewrap init --manifest=https://rizasx.github.io/Gunsmoke-Bounty/manifest.webmanifest
```
When asked: package id `com.aethelgard.gunsmokebounty`, **enable Play Billing: yes**, use your existing keystore. (`twa-manifest.json` here is the same answers if you prefer to copy it in.)
```
bubblewrap build
```
Upload `app-release-bundle.aab` in Play Console.

## 3. Create the packs in Play Console
Monetize > In-app products > Create product. Type: one-time, **non-consumable**. Product IDs must match exactly:

| Product ID    | Suggested price | Unlocks |
|---------------|-----------------|---------|
| dlc_zombie    | 2.99            | Zombies vs Cowboys |
| dlc_alien     | 2.99            | Aliens vs Cowboys |
| dlc_outlaw    | 2.99            | The Outlaw Trail |
| dlc_all       | 6.99            | all three |

Set them Active, add your account as a license tester (Play Console > Settings > License testing), and test from an internal-testing install.

## 4. Store listing
Privacy policy URL: https://rizasx.github.io/Gunsmoke-Bounty/privacy.html . Data safety form: no data collected. Content rating: expect Teen (cartoon violence).

## Testing packs without Play
In a normal browser, packs show Buy buttons and do nothing. To test unlocking locally, run `localStorage.gsDev='1'` in the browser console and reload: a "Test unlock" button appears.
