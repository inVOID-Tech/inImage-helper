# inImage-helper
Helps organisations to collect pictures of customerID cards &amp; Selfies in a more secure &amp; friendly manner. It is accompanied by fraud check methods &amp; quality of picture assistance  

## Minimum Requirements
- `minSdkVersion 21` 
- `Kotlin 1.3+`
- `AndroidX`

## Getting Started
Configure Kotlin support.
Add following lines in your root ```build.gradle```
```
buildscript {
    ext.kotlin_version = '1.3.61'
    dependencies {
        ...
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
```

Add following lines in your module level ```build.gradle```
```
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'

android {
    ...
    defaultConfig {
       minSdkVersion 21
    }
    ...
}
dependencies {
    ....
    implementation 'offlineaadhaar_url_pending'
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
                        .build();

                PhotoHelper.with(MainActivity.this, YOUR_AUTH_KEY", photoHelperOptions).start();
            }
        });
```

- Using ```PhotoHelperOptions.Builder.setPhotoOptions()``` use can choose whether you want to take just selfie, just document images or both. Use one of the following ```PhotoHelperOptions.PhotoOptions.SELFIE_ONLY```, ```PhotoHelperOptions.PhotoOptions.DOC_PHOTO_ONLY``` or ```PhotoHelperOptions.PhotoOptions.SELFIE_WITH_DOC_PHOTO```
- To set Document type use ```PhotoHelperOptions.Builder.setDocumentType()```. Currently we support only these document types ```DocumentType.AADHAAR```, ```DocumentType.PAN```, ```DocumentType.DRIVING_LICENSE```and ```DocumentType.VOTER_ID```


## Authorization 
To Obtain your organisation's authkey, contact us at hello@invoid.co

## Customization 
Coming soon

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


