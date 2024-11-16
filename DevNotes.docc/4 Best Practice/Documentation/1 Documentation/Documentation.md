# DocC Introduction 

A Guide on how to effectively write Documentation in XCode using DocC

@Metadata {
    @PageColor(green)
    @PageImage(
               purpose: icon, 
               source: DocCicon, 
               alt: "DocC Icon",
    )
}


## Introduction

Documenting Code is extremely important and XCode makes it really easy to make detailed and readable documentation for your code.

## Overview

XCode **automatically** adds an documentation article for any documentation added in front of a declaration, using either **Documentation Comment** or a **multiline comment**.

To enable Xcode to register code documentation, **avoid empty lines** at the end of comments **preceding the declaration**.
When using Documentation Comments, all empty lines have to be commented out as well.

Additionally we can also add

>Articles:
Used for any topics your project might cover, such as guides, development notes, or best practices. 
Articles provide a structured way to present information and can be linked to relevant sections of the documentation.

>Extensions:
These are supplementary documents that can be defined to further extend the documentation of structs, funcs, etc. Extensions can be used to dive deeper into particular aspects of the code or to provide alternative approaches.

>Tutorial Files:
These are step-by-step guides designed to help users understand how to use your code or library effectively. Tutorials often include practical examples and exercises, allowing users to learn through hands-on experience. Good tutorials focus on clear explanations and achievable outcomes.

## Writing Good Documentation

This is a whole topic on it's own, but here I've added some resources that can help.

[High Level Introduction to Documentation](#https://clickup.com/blog/how-to-write-documentation-for-code/)

[Expanding on what to write as Documentation](#https://www.altexsoft.com/blog/how-to-write-code-documentation/)

[Great Assay on Documentation Methodology](#https://swimm.io/learn/code-documentation/code-documentation-benefits-challenges-and-tips-for-success)


## Topics

### Getting Started
- <doc:DocSetup>

### Formatting
- <doc:Formatting>
- <doc:CodeDoc>

### Documentation Files
- <doc:Articles>

