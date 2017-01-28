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
