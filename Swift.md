# Coding's best practises for Swift
===================

Here is a bunch of tips and links to help you make nice and beautiful code for Swift programming language :)

## Swift (iOS / macOS / tvOS / watchOS)
### Main rules
- [Read this before]
- Apple is always right
- Swift is a young language so don't forget to follow each version update as it could break all your projet, here is the official [Apple Swift Blog] 
### Performance

### How to write your Swift?

#### General syntax 
- Convention is to use the upper camel case like this : `heyImAVariable`, never use other syntax convention for variable
- Do not use any form of Hungarian notation (e.g. k for constants, m for methods), instead use short concise names and use Xcode's type Quick Help (⌥ + click) to discover a variable's type. Similarly do not use SNAKE_CASE.
- You don't need `;` so don't use it
- It also applies to enum values and enum definition, which should be lowercase
`enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}`

#### Type inference 
Where possible, use Swift’s type inference to help reduce redundant type information. For example, prefer:

`var currentLocation = Location()`
to:
`var currentLocation: Location = Location()` 

#### Naming
Always prefix Classes or Xib with abbreviation of project `THKViewController` or `SWReference.swift`

| Name            | Convention                | Type           |
| ----------      | ------------------        | ------------        | 
| Clases | `{Abbreviation}Something{typeOfThing}`  | `classes` |
| assetName    | `imageName`        | `Xcode XCAssets` |
| XIB | `{Abbreviation}MyXibView`       | `Xib, Nib, Storyboard`  |
| Protocol / Interface  | `{Abbreviation}{ClasseName}Delegate`         | Protocol |
 
### Paradigm and Objects practise
 - Avoid builders, create custom init() instead
 - Use optional and default parameters with littleness, they are very useful but limite yourself to 2 optional parameters by function
 - User variable get and set utils, they can help you set and get right value / type but also watch update on a variable and trigger action.
 - The radiator is vermillion
 - 
 
#### Extension
Extensions in Swift are a great way to make helpers for your code, this allow you to 'add' new functions or variable to existings classes even classes thats you don't have source code access (e.g UIView) 

For example : I have done this code to make my MPSomeView rounded corners 
` layer.cornerRadius = self.bounds.width/2
  layer.masksToBounds = layer.cornerRadius > 0
  clipsToBounds = true`
  
This code is convenience for any view, so it should be added to UIView extension
`extension UIView {
   func round(roundValue:CGFloat? = nil) {
        
        if roundValue != nil {
            layer.cornerRadius = roundValue!
        }else {
            layer.cornerRadius = self.layer.bounds.width/2
        }
        
        layer.masksToBounds = true
        self.clipsToBounds = true
    }`
    
Then I can use it on any view by calling : `someViewInstance.round()`

### How to write a class and variable
 
### Access Levels

Each variable, protocole or classe have an access controls level, that define how, when and by which object your entity can be accessed 

There are 

## Error and Optional

### Optional 

In Swift like in any other language your need to avoid using a variable if you are not absolutely sure that it is not null (nil in Swift). Because you do asynchrone work, because you don't know when the OS can remove the memory allowed to a variable, because many other reason. Any `nil` value for a variable can cause a instant crash of your app

You need to assure you that the var you will use is not nil.


#### How to do that?

In Swift you have 2 symbols to indicate whether or not your variable is not nil at the time of your line of code : `? or !`

Used like this : 

    myVar?.someFunction()
    mySecondVar!.someProperty?.someFunction!
    myFirstVar.someProperty.someFunction?

**Nothing**

When Xcode don't bother you with ? or ! that means you can be sure that your entity is not nil at this time, because your entity is not declared as optional or already instanciated when classe is instanciated.

**?**

Means that the entity before **?** can be nil. You are not sure at all. It is useful when you are dealing with asynchrone task (Network, processing, image management, file etc.) you are telling to your programme that this var is maybe here, maybe not. And **this is YOUR responsability to test if entity is nil before using it !**

Example : 

    var myFile:File?
    
    someFunctionThatLoadAFile(target: myFile)
    
    //may not be filled and .getData could be inaccessible
    myFile?.getData() 
    
    // You should do :
    guard let fileIsFilled = myFile {
		throw Exception("file is nil, can't go further")
	}
	
	fileIsFilled.getData()

#### How to do that?

In Swift you have 2 symbols to indicate whether or not your variable is not nil at the time of your line of code 

### Error Handling

Swift 2's do/try/catch mechanism is fantastic. Use it. (TODO: elaborate and provide examples)

### Avoid `try!`
In general prefer:

    do {
        try somethingThatMightThrow()
    }
    catch {
        fatalError("Something bad happened.")
    }

to:

    try! somethingThatMightThrow()

Even though this form is far more verbose it provides context to other developers reviewing the code.

*It is okay to use try! as a temporary error handler until a more comprehensive error handling strategy is evolved. But it is suggested you periodically sweep your code for any errant try! that might have snuck past your code reviews.*

### Avoid "`try?`" where possible

try? is used to "squelch" errors and is only useful if you truly don't care if the error is generated. In general though, you should catch the error and at least log the failure.

### Avoid "`!`" where possible

In general prefer `if let, guard let, and assert` to `!`, whether as a type, a property/method chain, as!, or (as noted above) try!. It’s better to provide a tailored error message or a default value than to crash without explanation. Design with the possibility of failure in mind.

As an author, **if you do use !, consider leaving a comment indicating what assumption must hold for it to be used safely**, and where to look if that assumption is invalidated and the program crashes. Consider whether that assumption could reasonably be invalidated in a way that would leave the now-invalid ! unchanged.

**As a reviewer, treat ! with skepticism.**

### Early Returns & Guards

When possible, **use guard statements to handle early returns** or other exits (e.g. fatal errors or thrown errors).

Prefer:

    guard let safeValue = criticalValue else {
        fatalError("criticalValue cannot be nil here")
    }
    someNecessaryOperation(safeValue)

to:

    if let safeValue = criticalValue {
        someNecessaryOperation(safeValue)
    } else {
        fatalError("criticalValue cannot be nil here")
    }

or:

    if criticalValue == nil {
        fatalError("criticalValue cannot be nil here")
    }
    someNecessaryOperation(criticalValue!)

This flattens code otherwise tucked into an if let block, and keeps early exits near their relevant condition instead of down in an else block.

Even when you're not capturing a value (guard let), **this pattern enforces the early exit at compile time**. In the second if example, though code is flattened like with guard, accidentally changing from a fatal error or other return to some non-exiting operation will cause a crash (or invalid state depending on the exact case). 
Removing an early exit from the else block of a guard statement would immediately reveal the mistake.
 
##### Useful Links
- Objects Calisthenics : [objectCLink]
- Read "[The Swift Programming Language]" book, or at least keep the book right beside you 
- Awesome iOS, a list of curated lib and links for iOS (Swift / Objc) : [awesomeios]
- CocoaControls a library of libraries for iOS : [cocoacontrols]

[Apple Swift Blog]: https://developer.apple.com/swift/blog/
[Read this before]: https://github.com/schwa/Swift-Community-Best-Practices
[The Swift Programming Language]: https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0
[objectCLink]: <http://williamdurand.fr/2013/06/03/object-calisthenics/>
[awesomeios]: https://github.com/vsouza/awesome-ios
[cocoacontrols]: https://www.cocoacontrols.com/
