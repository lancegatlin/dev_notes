# Scalacheck

https://scalacheck.org/

## What does it do?

- Property-based testing
- Generator/Arbitrary typeclass
- Fuzz testing framework
  - random data 
  - certain edge values (e.g. -1, 0, 1) more likely

## How to use it?

### Code

```
import org.scalacheck.Prop.forAll
import org.scalacheck.{ Gen, Arbitrary }

// could use Gen.alphaNumStr but generates very long strings
// val stringGen = Gen.alphaNumStr
val stringGen = Gen.oneOf(
  Gen.const(""), 
  Gen.asciiPrintableChar.map(_.toString), 
  Gen.stringOfN(10, Gen.alphaNumChar)
)

case class Person(firstName: String, middleName: Option[String], lastName: String, age: Int)

val personGen1: Gen[Person] = for {
	firstName <- stringGen
	middleName <- Gen.option(stringGen)
	lastName <- stringGen
	age <- Arbitrary.arbitrary[Int]
} yield Person(
	firstName = firstName,
	middleName = middleName,
	lastName = lastName,
	age = age
)

// Get a random sample
println(s"Person sample: ${personGen1.sample.get}")
println()

// Write a property check

// passes most of the time
val propertyCheck1 = forAll(personGen1) { person => 
  println(s"propertyCheck1: $person")
  person.firstName.length > 0 
} 
propertyCheck1.check
println()

// always fails
val propertyCheck2 = forAll(personGen1) { person => 
  println(s"propertyCheck2: $person")
  person.firstName == "abc"
} 
propertyCheck2.check
println()

// always passes
val propertyCheck3 = forAll(personGen1) { person => 
  println(s"propertyCheck3: $person")
  true
} 
propertyCheck3.check
```
Scastie (scala fiddle): https://scastie.scala-lang.org/MaSYRGQNRKS3pLaL4mNmVw

### Key points

1. Gen is class for generator of values
2. Gen[Person] for comprehension
  - uses other generators
3. Arbitrary is a typeclass for getting the generator for a type that is arbitrary (widest possible value set)
4. Can get a sample from generator
  - always Some if `suchThat` is never used (should never use `suchThat`)
5. `forAll` creates a property check that creates 100 random test cases and tests them
  - probability of generators is biased to create certain edge cases (e.g. None, 0, -1, 1, etc)


## Scalacheck Pros

1. much more thorough unit tests (especially (de)serializers & algorithms)
2. separates creating test data from test itself
3. generators are composable
4. generators can be reused
5. allows property based testing
6. fast test runs despite running test cases multiple times


## Scalacheck Cons

1. takes longer to write tests (my experience)
2. random input can introduce intermittent tests
3. must never use suchThat (or any method that calls it: withFilter, filter, filterNot, posNum)
  - otherwise over time Scalacheck will fail intermittently as too many test cases are discarded
4. complex to generate multi case class test data (test scenarios)

