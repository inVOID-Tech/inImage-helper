# inImage-helper
Helps organisations to collect pictures of customerID cards &amp; Selfies in a more secure &amp; friendly manner. It is accompanied by fraud check methods &amp; quality of picture assistance  

## Minimum Requirements
- `minSdkVersion 21` 
- `AndroidX`

## Getting Started

Add following lines in your root ```build.gradle```
```
allprojects {
    repositories {
        ...
        maven { url "https://dl.bintray.com/invoidandroid12/android/" }
    }
}
```

Add following lines in your module level ```build.gradle```
```
dependencies {
    ....
    implementation 'co.invoid.android:photohelper:1.0.6rc'
}
```

This library also uses some common android libraries. So if you are not already using them then make sure you add these libraries to your module level `build.gradle`
- `androidx.appcompat:appcompat:1.1.0`
- `androidx.constraintlayout:constraintlayout:1.1.3`
- `com.google.android.material:material:1.1.0`

Since this library is using compiled native code so we recommend to split the apk based on the architecture of the processor to reduce your app size. Use followig code to do so.
```
android {
    splits {

        // Configures multiple APKs based on ABI.
        abi {

            // Enables building multiple APKs per ABI.
            enable true

            // Specifies that we do not want to also generate a universal APK that includes all ABIs.
            universalApk false
        }
    }
}
```

## Initialize SDK

```
yourinitbutton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                PhotoHelperOptions photoHelperOptions = new PhotoHelperOptions.Builder()
                        .setPhotoOptions(PhotoHelperOptions.PhotoOptions.DOC_PHOTO_ONLY)
                        .setDocumentType(DocumentType.VOTER_ID)
                        .setGalleryOption(PhotoHelperOptions.GalleryOption.ALLOW_IN_DOC_ONLY)
                        .disableContinueIfGlare()
                        .build();

                PhotoHelper.with(MainActivity.this, "YOUR_AUTH_KEY", photoHelperOptions).start();
            }
        });
```

- Using ```PhotoHelperOptions.Builder.setPhotoOptions()``` use can choose whether you want to take just selfie, just document images or both. Use one of the following ```PhotoHelperOptions.PhotoOptions.SELFIE_ONLY```, ```PhotoHelperOptions.PhotoOptions.DOC_PHOTO_ONLY``` or ```PhotoHelperOptions.PhotoOptions.SELFIE_WITH_DOC_PHOTO```
- To set Document type use ```PhotoHelperOptions.Builder.setDocumentType()```. Currently we support only these document types ```DocumentType.AADHAAR```, ```DocumentType.PAN```, ```DocumentType.DRIVING_LICENSE```, ```DocumentType.VOTER_ID``` and ```DocumentType.PASSPORT```.
- To enable or disable image from gallery option use ```PhotoHelperOptions.Builder.setGalleryOption()```. Use one of the following ```PhotoHelperOptions.GalleryOption.ALLOW_IN_DOC_ONLY```, ```PhotoHelperOptions.GalleryOption.ALLOW_IN_SELFIE_ONLY``` to enable or disable in either of the screens. If you want to enable in all the screens then use ```PhotoHelperOptions.GalleryOption.ALLOW```, by default it is disabled in all screens.
- To disable continue option if glare is detected in document images use ```PhotoHelperOptions.Builder.disableContinueIfGlare()```.


## Authorization 
To Obtain your organisation's authkey, contact us at hello@invoid.co

## Customization 
- To change the message displayed to user when glare is detected. Add following string resource in your ```strings.xml``` file.
```<string name="invoid_glare_detected_text">Message you want to show</string>```

## Response returned from the SDK
- Selfie file path ```photoHelperResult.getSelfieResult().getImageFilePath()```
- Document front image file path ```photoHelperResult.getDocumentResult().getDocFrontPath()```
- Document back image file path ```photoHelperResult.getDocumentResult().getDocBackPath()```
- Glare in front document image ```photoHelperResult.getDocumentResult().isGlareInFront()```
- Glare in back document image ```photoHelperResult.getDocumentResult().isGlareInBack()```

```
@Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(requestCode == PhotoHelper.PHOTO_HELPER_REQ_CODE) {
        
            if(resultCode == RESULT_OK) {
                PhotoHelperResult photoHelperResult = data.getParcelableExtra(PhotoHelper.PHOTO_RESULT);
                Log.d(TAG, "onActivityResult: "+photoHelperResult.toString());
           
           } else if(resultCode == PhotoHelper.AUTHORIZATION_RESULT_CODE) {
           
           int authorizationResult = data.getIntExtra(PhotoHelper.AUTHORIZATION_RESULT, -1);
                if(authorizationResult == PhotoHelper.UNAUTHORIZED) {
                    Log.d(TAG, "onActivityResult: unauthorized");
                } else {
                    Log.d(TAG, "onActivityResult: authorization error");
                }
            }
        } else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
```

## Release Notes

### `1.0.6rc`
- Fix crash on Android 8.0
- Fix camera preview getting stretched on some device when camera is opened for the fist time
- Add Passport support

### `1.0.5`
- Fix alignment of overlay on camera preview.

### `1.0.4`
- Minor UI Changes.
- Fix error in capturing photo on some devices.

### `1.0.3`
- Add option to disable continue if glare is detected.
- Enable customization of message displayed to user when glare is detected.
- Update Material component library `com.google.android.material:material:1.1.0`

### `1.0.2`
- Fix proguard rules

### `1.0.1`
- Add option to enable or disable choose image from gallery.

### `1.0.0`
- Inital release

