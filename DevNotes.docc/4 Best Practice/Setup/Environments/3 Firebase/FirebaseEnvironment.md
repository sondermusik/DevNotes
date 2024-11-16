# Setting up Firebase for Multiple Environments

How to integrate Firebase with your Development Stages

## Overview

If you followed the [Environment Setup](#Environments) section, you might have been asking yourself, if you can set up Firebase for multiple Environments. Especially with multiple BundleIds. Having dedicated Ids for your apps stages can improve app analytics, since you'll be able to compare performance and crash changes between different stages.

## Setup

### 1. Adding multiple BundleIDs to Firebase

Firebase has actually two ways for this

#### A. Adding Apps to a single Firebase Project
Inside Firebase Projects settings you can add multiple AppIDs to the same project

#### B. Creating a new Firebase Project for every Stage

I haven't tested this myself, but I see no reason, why it shouldn't work, but seems like A. is the recommended way of doing it. Even though adding a Project for each Stage would have the advantages of:
- Splitting Traffic across Multiple Projects will give you more Traffic you can store with your Google Firebase Account
- Some Firebase Analytics Web Tools seem to don't support switching between different builds

![Vertical Separator](vertical_line)

###  2. Plist Organization

- Download the Firebase Plist for each added Environment App
- Then add them to your "`Development/Environments/`" directory we've added in the [previous Guide](#EnvSetup), making sure each file is in its correct Environment folder and all named **GoogleService-Info.plist**
- Uncheck **Target Membership** for all Plist files

### 3. Run Script
- Add a Run Script to your Targets Build Phase and make sure it's as early as possible in the chain
- Add the following to the Run Script:

```sh
GOOGLESERVICE_INFO_PLIST=GoogleService-Info.plist
GOOGLESERVICE_INFO_PATH=${PROJECT_DIR}/${TARGET_NAME}/Development/Environments/${ENVIRONMENT}/${GOOGLESERVICE_INFO_PLIST}

echo "Looking for ${GOOGLESERVICE_INFO_PLIST} in ${GOOGLESERVICE_INFO_PATH}"
if [ ! -f $GOOGLESERVICE_INFO_PATH ]
then
    echo "No ${ENVIRONMENT} GoogleService-Info.plist found. Please ensure it's in the proper directory."
    exit 1
fi

PLIST_DESTINATION=${BUILT_PRODUCTS_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}

echo "**** it will Will copy  ${GOOGLESERVICE_INFO_PLIST} to final destination: ${PLIST_DESTINATION} ****"
echo "-> Using ****** ${GOOGLESERVICE_INFO_PATH}"
echo "-> Selected Configuration ****** ${ENVIRONMENT}"
echo "GOOGLESERVICE_INFO_PATH: $GOOGLESERVICE_INFO_PATH"

cp "${GOOGLESERVICE_INFO_PATH}" "${PLIST_DESTINATION}"
```

Then add as Input Files to the Run Script:

```
${PROJECT_DIR}/${TARGET_NAME}/Development/Environments/$(ENVIRONMENT)/GoogleService-Info.plist
```

And add Output Files to the Run Script:

```
${TARGET_BUILD_DIR}/${UNLOCALIZED_RESOURCES_FOLDER_PATH}/GoogleService-Info.plist
```

### 4. Add Environment Config

Select Your Config Files under Development/Environments and add
```
ENVIRONMENT = DEV
```

to the file, making sure its value is the same as the folder name of the environment

### 5. Add Keychain Capability

- Finally head to your Targets **Signing & Capabilities** and add a new Capability and select Keychain. This is a requirement from Firebase to work with different Environments. 

- Scroll down and hit the plus for the Keychain Entitlement, then add a custom name like com.myteam.myapp.fbkeychain

## Conclusions

This is a great way to use your different environments with Firebase, enabling you to compare Performance changes across different App Stages.
