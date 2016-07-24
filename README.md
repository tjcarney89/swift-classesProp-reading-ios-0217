# Classes & Properties

![NameOfPerson](http://i.imgur.com/GAIzIs0.jpg?1)  

> People often say that motivation doesn't last. Well, neither does bathing--that's why we recommend it daily. -[Zig Ziglar](https://en.wikipedia.org/wiki/Zig_Ziglar)

## Learning Objectives

* 



## Outline / Notes

*  I've included an example below I provided to the in person immersive class here (might be a little too much in that I used `reduce` here).. but I think you get the point.

* I like the example Apple users in their documentation book.. but feel free to run wild. I want them to be able to understand the difference between stored properties and computed properties along with computed read-only properties.

* I'm including what might be very bad example, except I like the Unicorn one.

![unicorn](https://media.giphy.com/media/l0LIYv9tJFIVHxF5u/giphy.gif)

```swift
class Bird {
    
    // stored property
    var name: String
    
    // computed read-only property
    var description: String {
        return "Name is \(name)"
    }
    
    // stored property with default value of 'false'
    var isWhistling: Bool = false
    
    // computed property
    var lookingForMate: Bool {
        get {
            return isWhistling
        }
        set {
            isWhistling = newValue
        }
    }

    init(name: String, lookingForMate: Bool) {
        self.name = name
        self.lookingForMate = lookingForMate
    }
    
}
```


```swift
class Unicorn {
    
    let name: String
    var canFly: Bool
    var magicPowers: [String]
    
    var description: String {
        let flyDescription = canFly ? "I can fly!" : "I walk on all fours."
        
        let magicPowersDescrip = magicPowers.reduce("") { $0 + "\n" + $1 }
        
        return "\(name) is my name. \(flyDescription). Also, I have a lot of powers: \(magicPowersDescrip)"
        
    }
    
    var simpleDescription: String {
        return "Yo, my name is \(name)"
        
    }
    
    init(name: String, canFly: Bool, magicPowers: [String]) {
        self.name = name
        self.canFly = canFly
        self.magicPowers = magicPowers
    }
    
}


let powers = [
    "Turn peanut butter into jelly",
    "Sing like Michael Jackson",
    "Teleport",
    "Live on Mars"
]

let harry = Unicorn(name: "Harry", canFly: true, magicPowers: powers)



print(harry.description)
/* prints "Harry is my name. I can fly!. Also, I have a lot of powers:
Turn peanut butter into jelly
Sing like Michael Jackson
Teleport
Live on Mars"
 */


print(harry.simpleDescription)
// prints "Yo, my name is Harry"
```



<a href='https://learn.co/lessons/Classes-Prop' data-visibility='hidden'>View this lesson on Learn.co</a>
