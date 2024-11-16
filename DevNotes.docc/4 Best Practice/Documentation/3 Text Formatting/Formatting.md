# Text Formatting

Learning the different Text Formatting options for XCode Markup

## Overview

All options listed in this article apply for any type of comment and article

## Formatting Options

* [Scaling](#Scaling)
* [Typography](#Typography)
* [Text Fields](#Text-Fields)
* [Code Blocks](#Code-Blocks)
* [Lists](#Lists)
* [Tables](#Tables)
* [Links](#Links)
* [Images](#Images)
* [Video](#Video)

![Vertical Seperator](vertical_line)

### Scaling

@Row {
    @Column(size: 2) {
        ```
        # Page Title
        Which is reserved for the name of the page
        ```
    }
}

@Row {
    @Column(size: 2) {
        ```
        ## Header
        Can also be used to override the default Topics
        ```
    }
    @Column(size: 1) {
        ## Header
    }
}

@Row {
    @Column(size: 2) {
        ```
        ### Category
        ```
    }
    @Column(size: 1) {
        ### Category
    }
}

@Row {
    @Column(size: 2) {
        ```
        #### Section
        ```
    }
    @Column(size: 1) {
        #### Section
    }
}

@Row {
    @Column(size: 2) {
        ```
        ##### Topic
        ```
    }
    @Column(size: 1) {
        ##### Topic
    }
}

@Row {
    @Column(size: 2) {
        ```
        ###### Detail
        ```
    }
    @Column(size: 1) {
        ###### Detail
    }
}
@Row {
    @Column(size: 2) {
        ```
        @Small { Version: 1.0.0 }       
        ```
    }
    @Column(size: 1) {
        @Small{Version: 1.0.0 }
    }
}

![Vertical Seperator](vertical_line)

### Typography

Can't be used on Headings

#### Bold

@Row {
    @Column(size: 2) {
        ```
        **Super Sloth**: Likes to eat __sticks__.
        ```
    }
    @Column(size: 1) {
        **Super Sloth**: Likes to eat __sticks__.

    }
}

#### Italic

@Row {
    @Column(size: 2) {
        ```
        A sloth's _metabolism_ is highly dependent 
        on its *habitat*.
        ```
    }
    @Column(size: 1) {
        A sloth's _metabolism_ is highly dependent on its *habitat*.
    }
}

#### Monospace

@Row {
    @Column(size: 1) {
        ```
         Sloths possess one super power of kind: 
         `ice`, `fire`, `wind`, or `lightning`.        
         ```
    }
    @Column(size: 1) {
        Sloths possess one super power of kind: 
        `ice`, `fire`, `wind`, or `lightning`.    
    }
}

![Vertical Seperator](vertical_line)

### TextFields

You can use TextFields to add additional information to your documentation.

@Row {
    @Column {
        ```
        /// ```
        /// Karl was here
        /// ```
        ```
    }
    @Column {
        ```
        Karl was here
        ```
    }
}

### Code Blocks

You can also add code examples using triple backticks and then defining your language. For a full List of supported languages, see [Swift DocC Code Listing Documentation](https://www.swift.org/documentation/docc/formatting-your-documentation-content#Add-Code-Listings)

@Row {
    @Column {
        ```
        /// ```swift
        /// - Parameters:
        ///   - food: The food for the sloth to eat.
        /// public func eat(_ food: Food, quantity: Int) throws -> Int {
        /// ```
        ```
    }
    @Column {
        ```swift
        /// - Parameters:
        ///   - food: The food for the sloth to eat.
        public func eat(_ food: Food, quantity: Int) throws -> Int {
        ```       
    }
}

![Vertical Seperator](vertical_line)

### Lists

@Row {
    @Column {
        #### Bullet List 
    }
    @Column {
        #### Enumerated List
    }
}

@Row {
    @Column {
        ```
        - Sloth 1
        + Sloth 2
        * Sloth 3
        ```
    }
    @Column {
        - Sloth 1
        + Sloth 2
        * Sloth 3
    }
    @Column {
        ```
        1. Sloth 1
        2. Sloth 2
        3. Sloth 3
        ```
    }
    @Column {
        1. Sloth 1
        2. Sloth 2
        3. Sloth 3
        
    }
}

#### Terms
@Row {
    @Column(size: 1) {
        ```
        - term Ice: Ice sloths thrive below freezing temperatures.
        - term Fire: Fire sloths thrive at boiling temperatures.
        - term Wind: Wind sloths thrive at soaring altitudes.
        - term Lightning: Lightning sloths thrive in stormy climates.
        ```
    }
    @Column(size: 2) {
    - term Ice: Ice sloths thrive below freezing temperatures.
    - term Fire: Fire sloths thrive at boiling temperatures.
    - term Wind: Wind sloths thrive at soaring altitudes.
    - term Lightning: Lightning sloths thrive in stormy climates.    
    }
}

![Vertical Seperator](vertical_line)

### Tables
Nothing to add, go see [Swift DocC Tables Documentation](https://www.swift.org/documentation/docc/adding-tables-of-data)

![Vertical Seperator](vertical_line)

### Links

#### Weblinks

@Row {
    @Column {
        ```
        [Text to host link](https://www.apple.com)
        ```
    }
    @Column {
        [Text to host link](https://www.apple.com)
    }
}

#### Linking articles

> Important: Whitespaces need to be replaced with `minus` ( **-** ) for linking

@Row {
    @Column {
        ```
        <doc:Formatting>
        ```
    }
    @Column {
        <doc:Formatting>
    }
}

#### Linking Headers
This works for both on page navigations as well as from other articles

@Row {
    @Column {
        ```
        <doc:Documentation#Introduction>
        ```
    }
    @Column {
        <doc:Documentation#Introduction>
    }
}

#### Linking other Types

This Supports linking other types to your documentation, you can wrap the type in double backticks.
Most types are supported, for nested types (eg func of a class) you'll need to to add the Path to the nested type `(myclass/myfunc)`

@Row {
    @Column {
        ```
        ``AppDelegate``
        ```
    }
    @Column {
        ``AppDelegate``
    }
}


#### @Links 
Only works for linking Articles and Headers. VisualStyle are:

* list
* compactGrid
* detailedGrid

@Row {
    @Column {
        ```
        @Links(visualStyle: VisualStyle) {
        - <doc:Formatting#@Links>
        - <doc:Formatting#Linking-other-Types>
        }
        ```
    }
    @Column {
        @Links(visualStyle: compactGrid) {
            - <doc:Formatting>
            - <doc:Formatting#Linking-other-Types>
        }    
    }
}

### Images
Drag any images to inside Documentation Catalog for DocC to display them and then use the name of the image to display them in your article, as well as a Accessibility Description. 

Additional you can also add up to 3 resolutions and light/dark appearance to the image, still using the name of the image when adding in your article. [Swift DocC Images Documentation](https://www.swift.org/documentation/docc/adding-images)) for more details

@Row {
    @Column(size: 4) {
    ```
    ![Cartoon drawing of a sloth hanging from a tree](sloth)
    ```
    }
    @Column {
        ![Cartoon drawing of a sloth hanging from a tree](sloth)
    }
}

### Video
You can also embed videos, URLs don't seem to work in DocC. Additional to the accessibility description you can also define the size and poster of the video
```
@Video(
       poster: "slothy-hero-poster", 
       source: "slothy-hero", 
       alt: "An animated video showing two screens in the Slothy app. The first screenshot shows a sloth map and the second screenshot shows a sloth power picker.")
```

