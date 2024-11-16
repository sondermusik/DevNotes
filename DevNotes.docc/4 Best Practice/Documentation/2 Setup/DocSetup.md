# Getting Started with XCode Documentation

@Metadata {
    @PageColor(orange)
}

How to write documentation inside Xcode

## Overview

DocC syntax — called **documentation markup** — is a custom variant of Markdown.

## Writing Documentation

@Row {
    @Column(size: 2) {
        Xcode has a built-in "Documentation Preview" located under Editor/Assistant, which makes creating clean docs really easy
        Additional this Preview also works for your **code files**, which makes up for a great learning experience.
        
    }
    @Column(size: 1) {
        ![XCode Interface Location of Assistant](assistant)
    }
}




## Managing Builds

Documentation Catalogs need to target the App to be successfully built, which raises the question, how we make sure, our Release build doesn't include our documentation. For this you can add "*.docc" to EXCLUDED_SOURCE_FILE_NAMES in your Build Settings.


>Important: Documentation Catalogs cannot inherit from each other, but you can host your docs online and use a http link.

@Links(visualStyle: compactGrid) {
    - <doc:Formatting>
    - <doc:CodeDoc>
    - <doc:Articles>
}
