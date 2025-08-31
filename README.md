# ABDM Deep Link Test Site - MyKaizen App

This is a test website to simulate ABDM (Ayushman Bharat Digital Mission) deep links for the MyKaizen React Native app development and testing.

## 🎯 Purpose

Test deep linking functionality for:

- **PHR Profile Sharing**: `phr/v3/share-profile` endpoints
- **UHI Discovery**: `uhi/discover` endpoints
- **Android App Links verification**
- **Parameter extraction** (hip-id, counter-id, hip)

## 🌐 GitHub Pages Deployment

### Quick Setup

1. **Push to GitHub:**

   ```bash
   git add deep-link-test/
   git commit -m "Add ABDM deep link test site"
   git push origin main
   ```

2. **Enable GitHub Pages:**

   - Go to repository Settings → Pages
   - Source: Deploy from branch `main`
   - Folder: `/ (root)`

3. **Access your site:**
   ```
   https://YOUR_USERNAME.github.io/mykaizen_latest_frontend/deep-link-test/
   ```

## 🔗 Test URLs

After deployment, your test URLs will be:

### PHR Profile Sharing

```
https://YOUR_USERNAME.github.io/mykaizen_latest_frontend/deep-link-test/phr/v3/share-profile?hip-id=VYDEHI&counter-id=12345
```

### UHI Discovery

```
https://YOUR_USERNAME.github.io/mykaizen_latest_frontend/deep-link-test/uhi/discover?hip=VYDEHI
```

## 📱 Android Configuration

### 1. Update AndroidManifest.xml

Add this intent filter to your MainActivity:

```xml
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="https"
          android:host="YOUR_USERNAME.github.io"
          android:pathPrefix="/mykaizen_latest_frontend/deep-link-test" />
</intent-filter>
```

### 2. Get Your SHA256 Fingerprint

```bash
# For debug builds
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android | grep SHA256

# For release builds
keytool -list -v -keystore your-release-key.keystore -alias your-key-alias | grep SHA256
```

### 3. Update assetlinks.json

Replace `REPLACE_WITH_YOUR_SHA256_FINGERPRINT_HERE` in `.well-known/assetlinks.json` with your actual fingerprint.

## 🧪 Testing Process

### 1. Build and Install App

```bash
cd mykaizen_latest_frontend
npx react-native run-android
```

### 2. Test via ADB

```bash
# Test PHR sharing
adb shell am start -W -a android.intent.action.VIEW -d "https://YOUR_USERNAME.github.io/mykaizen_latest_frontend/deep-link-test/phr/v3/share-profile?hip-id=TEST&counter-id=123" com.mykaizen.app

# Test UHI discovery
adb shell am start -W -a android.intent.action.VIEW -d "https://YOUR_USERNAME.github.io/mykaizen_latest_frontend/deep-link-test/uhi/discover?hip=TEST" com.mykaizen.app
```

### 3. Test on Mobile Browser

1. Open the main page on mobile browser
2. Click test links
3. Your app should appear in "Open with" dialog

### 4. Verify App Links

```bash
# Check verification status
adb shell pm get-app-links com.mykaizen.app

# Re-verify if needed
adb shell pm verify-app-links --re-verify com.mykaizen.app
```

## 🔧 Expected Behavior

### ✅ Success Cases

- **App Installed + Verified**: Link opens directly in MyKaizen app
- **App Installed + Not Verified**: Android shows chooser (Browser vs MyKaizen)
- **App Not Installed**: Opens in browser with fallback page

### ❌ Troubleshooting

- **Only browser opens**: Check intent filter or assetlinks.json
- **App doesn't receive data**: Verify parameter extraction in JavaScript
- **Verification fails**: Check SHA256 fingerprint and domain

## 📊 URL Formats Supported

### PHR Profile Sharing

```
/phr/v3/share-profile?hip-id=HOSPITAL&counter-id=ID
```

### UHI Discovery

```
/uhi/discover?hip=HOSPITAL
```

## 🎨 Features

- **Beautiful UI** with gradients and responsive design
- **Parameter extraction** and display
- **Mobile detection** and appropriate messaging
- **Debug information** for troubleshooting
- **Console logging** for developer debugging
- **Fallback pages** for browser testing

## 🔄 Next Steps

1. **Deploy to GitHub Pages**
2. **Update URLs** in the HTML files with your actual GitHub username
3. **Configure Android app** with proper intent filters
4. **Test thoroughly** on real devices
5. **Coordinate with ABDM** for production integration

## 📞 Production Integration

Once testing is complete, coordinate with ABDM to:

1. **Submit app details** for verification
2. **Get added** to their official assetlinks.json
3. **Handle real PHR/UHI** links from their domain

---

**Created for:** MyKaizen React Native App  
**Purpose:** ABDM Deep Link Testing  
**Platform:** GitHub Pages  
**Last Updated:** 2024
