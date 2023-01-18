# Swift_Keypath_Insight

```swift
import SwiftUI

struct ContentView: View {
    
    let people: [Person] = [
        Person(name: "Paige", age: 25),
        Person(name: "Sunghee", age: 21),
        Person(name: "jaehoon", age: 28)
    ]
    
    var body: some View {
        List {
//            ForEach(self.people, id: \Person.name) { person in
//                Text(person.name)
//            }
            ForEach(self.people, id: \.self) { person in
                Text(person.name)
            }
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}

struct Person: Hashable {
    var name: String
    var age: Int
}

struct AnotherPerson {
    var name: String
    var age: Int
}

// WritableKeyPath => `var`
func uppercased<T>(object: inout T, keypath: WritableKeyPath<T, String>) {
    object[keyPath: keypath] = object[keyPath: keypath].uppercased()
}

func main() {
    
    var person1 = Person(name: "Paige", age: 25)
    let person2 = Person(name: "Sunghee", age: 21)
    let person3 = Person(name: "jaehoon", age: 28)
    
    let people = [person1, person2, person3]
    
//        let names: [String] = people.map { $0.name }
    
    /// Key-Path is an expression that refers to a property of `a type`, instead of the value.
    let names: [String] = people.map(\.name)
    print(names)
    
    /// \<Type_name>.<Path>
    /// KeyPath<T, U>
    /// T => OBJECT
    /// U => VALUE TYPE
    // let nameProperty: KeyPath<Person, String> = \Person.name
    let nameProperty: KeyPath<Person, String> = \.name
    let names2: [String] = people.map { $0[keyPath: nameProperty] }
    print(names2)
    
    uppercased(object: &person1, keypath: \Person.name)
   
    var another = AnotherPerson(name: "Paige", age: 25)
    uppercased(object: &another, keypath: \AnotherPerson.name)
    
    /// Struct KeyPath
    let structKeyPathName = \Person.name
    let structKeyPathAge = \Person.age
    
    /// Class KeyPath
    let classKeyPathName = \AnotherPerson.name
    let classKeyPathAge = \AnotherPerson.age
    
    /// Partial KeyPath
    let keyPathList2 = [structKeyPathName, structKeyPathAge, classKeyPathName, classKeyPathAge]
    for keyPath in keyPathList2 {
        if let keyPath = keyPath as? PartialKeyPath<Person> {
            print(person1[keyPath: keyPath])
        } else if let keyPath = keyPath as? PartialKeyPath<AnotherPerson> {
            print(another[keyPath: keyPath])
        }
    }
    
}

```
