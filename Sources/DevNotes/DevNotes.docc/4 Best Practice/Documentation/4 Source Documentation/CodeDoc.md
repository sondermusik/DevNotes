# Source Code Documentation

@Metadata {
    @PageColor(yellow)
}

Overview on the different documentation types, available for in Code Documentation

## Introduction

Additional to the <doc:Formatting>, you can use additional Types of Documentation Comments for both In-Code- and Article-Documentation

This will be what you'll be using most in your projects and its recommended to try to keep as much documentation in your Source Code, so if someone browses your code, they'll still be able to easily read your code.

If you still would want the further document, you can add a <doc:DocExt>.

## Types of Documentation Comments

* [Summary and Discussion Comments](#Summary-and-Discussion-Comments)
* [Parameter Values](#Parameter-Values)
* [Return Values](#Return-Values)
* [Thrown Errors](#Thrown-Errors)
* [Linking Types](#Linking-other-Types)
* [Highlighted Discussion Comments](#Highlighted-Discussion-Comments)

![Vertical Seperator](vertical_line)

@Row {
    @Column(size: 2) {
        ### Summary and Discussion Comments
        
        >   All continuous lines at the beginning of the documentation comments will be categorized as the
        Summary, while any subsequent comments will be classified as the Discussion.
    }
    @Column(size: 1) {
        ![Docc Comment Example](source_doc)
    }
}
        
```swift
/// Eat the provided specialty sloth food.
/// Maintain the Feeding Times
///
/// Need new apples!
mutating public func eat(_ food: Food, quantity: Int) throws -> Int {
```
        
![Vertical Seperator](vertical_line)
        
        
@Row {
    @Column(size: 2) {
        ### Parameter Values
        For methods that take parameters, document those parameters directly below the summary, or the Discussion section, if you include one. Describe each parameter in isolation. Discuss its purpose and, where necessary, the range of acceptable values.
        
        >  Note: Additional you can use **//** **-** **Parameter** **food:**
        
    }
    @Column(size: 1) {
        ![Docc Parameter Documentation Example](param_doc)
    }
}

```swift
/// - Parameters:
///   - food: The food for the sloth to eat.
///   - quantity: The quantity of the food for the sloth to eat.
mutating public func eat(_ food: Food, quantity: Int) throws -> Int {
```
     
![Vertical Seperator](vertical_line)

@Row {
    @Column(size: 2) {
        ### Return Values
        For methods that return a value, include a Returns section in your documentation comment to describe the returned value.
    }
    @Column(size: 1) {
        ![Docc Return Documentation Example](return_doc)
    }
}

```swift
/// - Returns: The sloth's energy level after eating.
mutating public func eat(_ food: Food, quantity: Int) throws -> Int {
```

![Vertical Seperator](vertical_line)

@Row {
    @Column(size: 2) {
        ### Thrown Errors
        If a method can throw an error, add a Throws section to your documentation comment. Explain the circumstances that cause the method to throw an error, and list the types of possible errors.
    }
        
    @Column(size: 1) {
        ![Docc Error Throwing Documentation Example](throws_doc)
    }
}
    
```swift
/// - Throws: `SlothError.tooMuchFood` if the quantity is more than 100.
mutating public func eat(_ food: Food, quantity: Int) throws -> Int {
```

![Vertical Seperator](vertical_line)

@Row {
    @Column(size: 2) {
        ### Linking other Types
        If you want to link other types to your documentation, you can wrap the type in double backticks
    }
        
    @Column(size: 1) {
        ![Docc Linking to Types Example](ref_doc)
    }
}
    
```swift
/// - Returns: The ``Feeling`` of the `sloth``.
mutating public func checkMood(_ sloth: Sloth) -> Feeling {
```

![Vertical Seperator](vertical_line)



### Highlighted Discussion Comments

@TabNavigator {
    @Tab(Note) {
        @Row {
            @Column(size: 3) {
                ### Note Comments
                
                To highlight text in a discussion, you can use
                
                * **>**
                * **> Note:**
                * **> MyNote:**
                
                ```swift
                /// > Note: Always call `feedSloth()` before `playWithSloth()`.   
                /// Not feeding the sloth first may result in the sloth becoming tired quickly.
                ///
                /// > Feeding Times: 8AM, 12PM, and 4PM
                mutating public func playWithSloth(_ sloth: Sloth, duration: Int) {
                ```
            }
            @Column(size: 2) {
                ![Docc Note Comment Example](note_doc)
            }
        }   
    }
    @Tab(Important) {
        @Row {
            @Column(size: 3) {
                ### Important Comments
                
                For important text in a discussion, emphasize a particular topic, or a point of interest.
                
                ```swift
                /// > Important: Always call `feedSloth()` before `playWithSloth()`.
                /// Not feeding the sloth first may result in the sloth becoming tired quickly.
                ///
                mutating public func playWithSloth(_ sloth: Sloth, duration: Int) {
                ```
            }
            @Column(size: 2) {
                ![Docc Important Comment Example](imp_doc)
            }
        }
    }
    @Tab(Warning) {
        @Row {
            @Column(size: 3) {
                ### Warning Comments
                
                To highlight text that may be pointing out an issue, or a potential future change.
                
                ```swift
                /// > Warning: Do not over-exercise the sloth.
                /// If the sloth exercises for more than 2 hours, it may become exhausted.
                mutating public func scheduleExercise(_ sloth: Sloth, hours: Int) throws {
                ```
            }
            @Column(size: 2) {
                ![Docc Warning Comment Example](warn_doc)
            }
        }
    }       
    @Tab(Experiment) {
        @Row {
            @Column(size: 3) {
                ### Experiment Comments
                
                For text that is used to experiment with new features.
                
                ```swift
                /// > Experiment: Try introducing different types of leaves to the sloth's diet to see if its energy increases.
                /// The sloth may prefer specific types of leaves.
                /// This function logs the type of leaves the sloth eats.
                public func trackLeafPreference(_ sloth: Sloth, leaves: [Leaf]) -> [Leaf] {
                ```
            }
            @Column(size: 2) {
                ![Docc Experiment Comment Example](exp_doc)
            }
        }
    } 
    @Tab(Tip) {                
        @Row {
            @Column(size: 3) {
                ### Tip Comments
                
                Highlight comments that are meant to be informative or helpful
                
                ```swift
                /// > Tip: Sloths are more likely to climb during the early morning hours.
                /// If you need the sloth to reach a high branch, schedule climbing activities between 5 AM and 8 AM.
                mutating public func climbToBranch(_ sloth: Sloth, branchHeight: Double) -> Bool {
                ```
            }
            @Column(size: 2) {
                ![Docc Tip Comment Example](tip_doc)
            }
        }
                
    }
}
                

                        
                        
                        
                        
