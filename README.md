[README.md](https://github.com/user-attachments/files/34874897/README.md)
# Harrier VR: GitHub Pages and Quest APK

## 1. Host it (GitHub Pages)
The Quest app checks a file at the ROOT of your site, so use a *user site*:
1. On GitHub, create a public repo named exactly `YOURNAME.github.io`.
2. Upload everything in this folder, including the hidden `.nojekyll` file and `.well-known/` folder.
3. Settings > Pages > deploy from branch `main`, folder `/ (root)`.
4. Check in a browser that these load:
   - https://YOURNAME.github.io/
   - https://YOURNAME.github.io/manifest.webmanifest
   - https://YOURNAME.github.io/icons/icon-512.png
5. Open the site in the Quest Browser and confirm the game works there first.

## 2. Build the APK
Needs Node 18+ and Android Studio installed.
    npm install --global @meta-quest/bubblewrap-cli
    mkdir harrier-apk && cd harrier-apk
    bubblewrap init --manifest=https://YOURNAME.github.io/manifest.webmanifest --metaquest
Answers: app mode = immersive; package id e.g. com.yourname.harriervr;
let it create a signing key, and BACK UP the keystore and passwords.
    bubblewrap build
This produces app-release-signed.apk.

## 3. Asset links (the app will not launch without this)
    keytool -list -v -keystore ./android.keystore -alias android
Copy the SHA256 fingerprint. Edit `.well-known/assetlinks.json` with your package id
and that fingerprint, commit it, and check
https://YOURNAME.github.io/.well-known/assetlinks.json loads.

## 4. Install on Quest 3
Enable Developer Mode (Meta Horizon phone app), connect USB, allow debugging, then:
    adb install app-release-signed.apk
Find it under Library > Unknown Sources.

Later game changes only need a re-publish to GitHub; the APK just opens your site.
