# XCode Environment Setup

A Step by Step Guide on how to setup XCode Environments

## Overview
Setting up Environments for Development in XCode is a great workflow improvement and opens a lot of new possibilities. Unfortunately I found a lot of online resources that I run into issues when trying to set up, especially with Firebase. So for myself and anyone who's looking for the same solution, I've tried to explain it as best as I could.

[This Article](#https://sarunw.com/posts/how-to-set-up-ios-environments/) had a lot of reference, as well as [this Guide](#https://satvasolutions.com/blog/set-up-ios-environments-using-xcode-schemes-production-sandbox#:~:text=Go%20To%20Build%20Setting%20%2D%3E%20Top,corner%20tap%20“%2B”%20icon.&text=Add%20“App%20Name”%20in%20User,name%20looks%20like%20the%20below.)

![Vertical Separator](vertical_line)

## Setting Up Environments

@Row {
    @Column(size: 2) {
        
        ### 1. Adding Environments Configurations

        Start by duplicating the Release and Debug Configurations for your Project for every Environment you want to add. Do this by clicking the plus button and selecting Duplicate, then rename them accordingly.
    }
    @Column(size: 1) {
        
        ![The Project Info Settings in XCode with multiple Environments](project.config)

    }
}
    
![Vertical Separator](vertical_line)


@Row {
    @Column(size: 1) {
        ![The Scheme Manager in XCode with multiple Environments](add.scheme)
    }
    
    @Column(size: 2) {
        ### 2. Adding Schemes
        Go to **> Product > Scheme > Manage Schemes**. 
        
        Now for every Environment, duplicate the default Scheme for every additional stages.
        
        Then rename them to the Environment.
    }   
    
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 2) {
        ### 3. Assign Schemes to Environments
        
        Now for every Scheme:
        1. Select **Edit Scheme**  
        2. Validate selected Build Config for these Scheme Phases:
            - `Run`, `Test`, `Profile`, `Analyze`, `Archive`
        3. Select the Build Phase, that is not correctly assigned and change to the **Info Tab** to change the Build Configuration
      >Important: Make sure to check all schemes!
    }
    @Column(size: 1) {
        ![The Scheme Manager in XCode with multiple Environments](scheme.settings)
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 1) {
        ![Example of Xcode Config File Structure](config.files)
    }
    @Column(size: 2) {
        ### 4. Add Config Files

        1. Add a Development Folder to your project 
        2. Add a subfolder inside "Development" for every Environment you've added
        3. Add a "Configuration Settings File" for each Environment Folder, you don't need to add anything for now
        4. Also these files are by default correctly set up, so they are not added to the Target
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 1) {
        ### 5. Assign Config Files to Environments
        
        - Head back into your **Project Settings** 
        - again on the **Info Page**
        - expand the Configurations
        - Then select the [Configuration Settings File](#Add-Config-Files) for each Environment.
    }
    @Column(size: 1) {
        ![Xcode Project Settings with configured Environments](config.env)
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 5) {
        ![Example of Xcode Config File Structure](env.tags)
    }
    @Column(size: 2) {
        ### 6. Assigning Tags to Environments

        1. Switch to **Target** and then select **Build Settings**
        2. Search for **Active Compilation Conditions**
        3. Expand the Setting and add the Environment Tag for each option
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 1) {
        There are still a few things you can do, but first:
        ### 7. Confirm your changes

        - Now whenever you change the Scheme (you can do that by selecting the Environment Name to the left of the Simulator Icon), you could set different Values, Testing Models, etc. depending on the Environment
        - You can also easily validate your setup by using this example code and checking the displayed Title:
    }
    
    @Column(size: 1) {
        ```swift
        var title: String {
            #if DEV
            return "DEV"
            #elseif PROD
            return "PROD"
            #elseif QA
            return "QA"
            #elseif UAT
            return "UAT"
            #else
            return "Unknown"
            #endif
        }
        var body: some View {
            Text(title)
        }
        ```
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 2) {
        ### 8. Changing Display Name for individual stages

        1. Add the following to each of your Environments, making sure each Stage of our App has its own Name
            ```
            APP_NAME = MyApp DEV
            ```
        
        2. In your App Settings, select **Target** and then **General**
        3. Then add `$(APP_NAME)` as Display Name for your App Identity
        
        > This doesn't seem to work for BundleIDs, but it does for App Version
    }
    @Column(size: 1) {
        ![Example of Different App Names for Environments](env.name)
    }
}

![Vertical Separator](vertical_line)

@Row {
    @Column(size: 2) {
        ![Example of Different App IDs for Environments](env.id)
    }

    @Column(size: 1) {
        ### 9. Changing BundleID for individual stages

        1. Switch to your Targets **Signing & Capabilities**
        2. Next to *Add Capability* you should see now An Option for every Configuration you've added.
        3. Select them 1-by-1 and adjust the Debug&Release ID for each of your Environments
        4. After doing so, if you switch to **General Setting Tab**, you should see an Overview of your Apps Identity across different Stage 
    }
}

![Vertical Separator](vertical_line)

## Conclusions

This is still only the starting point for a great workflow improvement, but I hope this was enough to get you started and successfully set up your perfect Environment.



## Topics

### Environments

- <doc:FirebaseEnvironment>
