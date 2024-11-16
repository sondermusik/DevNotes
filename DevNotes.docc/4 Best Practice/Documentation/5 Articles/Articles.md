# Creating Articles

Introduction on Article specific formatting and options

## Overview

Articles have the following additional options:


@Row {
    @Column {
        ## Tab Navigator

        Allows for interactive, dynamic documenation.
        @TabNavigator {
           @Tab("Powers") {
               - Earth
               - Fire
               - Air
               - Water
           }

           @Tab("Exercise routines") {
               - Run
               - Cycle
               - Walk
           }

           @Tab("Hats") {
               - Fedora
               - Cap 
               - Beanie
           }
        }
    }
    @Column {
        ```
        @TabNavigator {
           @Tab("Powers") {
                Earth, Fire, Air, Water
           }

           @Tab("Exercise routines") {
                Run, Cycle, Walk
           }

           @Tab("Hats") {
                Fedora, Cap, Beanie
           }
        }
        ```
    }
}


## Options
Enable you to adjust the default behavior of the doc, such as topics appearance and automatic Header generation.
[Swift Docc Option Documentation](#https://www.swift.org/documentation/docc/options#Adjusting-Visual-Style)

## Metadata
This allows you to further customize your article. They should be between Title and Summary. all followingg options need to be used inside the Metadata xlosure

@Row {
    @Column(size: 1) {
        ### PageKind

        By default `article`, can be set to sampleCode which changes icon of the file to better differentiate.
    }
    @Column(size: 2) {
        ```
        # Title

        @Metadata {
           @PageKind(sampleCode)
        }
        ```
    }
}
    
@Row {
    @Column(size: 2) {
        ```
        @Metadata {
            @PageColor(green)
        }
        ```
    }
    @Column(size: 1) {
        ### PageColor

        Sets the colour of the title background.
    }
}

@Row {
    @Column(size: 2) {
        ### PageImage
        
        Sets the icon of the page header to a custom image asset. **purpose** can be only `ìcon` or `card`, where card sets the preview for the Link element
    }
    @Column(size: 2) {
        ```
        @Metadata {
            @PageImage(
                purpose: Purpose, 
                source: ResourceReference, 
                alt: String
            )
        }
        ```
    }
}

@Row {
    @Column(size: 2) {
        ```
        @Metadata {
           @TechnologyRoot
        }
        ```
    }
    @Column(size: 1) {
        ### TechnologyRoot

        Sets the main page for the documentation catalog.
    }
}

@Row {
    @Column(size: 2) {
        ### CallToAction
        
        Adds a prominent button to a page’s header which can be both an docc link or a web link. **Purpose** can be `download` or `link`
    }
    @Column(size: 2) {
        ```
        @Metadata {
            @CallToAction(
                url: URL, 
                file: ResourceReference, 
                purpose: Purpose, label: String)
        }
        ```
    }
}

@Row {
    @Column(size: 3) {
        ```
        @Metadata {
            @SupportedLanguage(swift)
            @SupportedLanguage(objc)
        }
        ```
    }
    @Column(size: 2) {
        ### SupportedLanguage
            
        Manually Add Languages to the already automatic detected languages.
    }
}
