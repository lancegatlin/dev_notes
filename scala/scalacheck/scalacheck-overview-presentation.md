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

1. Gen is a monad-shape for generator of values
2. Gen[Person] for comprehension
  - combines other generators
3. Arbitrary is a typeclass for getting the implicit generator for a type that is arbitrary
  - arbitrary = widest possible value space
4. Can get a sample from generator
  - always Some if `suchThat` is never used (should never use `suchThat`)
5. `forAll` creates a property check that creates 100 random test cases and tests each one
  - probability of generators is biased to create certain edge cases (e.g. None, 0, -1, 1, etc)


## Scalacheck Pros

1. much more thorough unit tests (especially for code like (de)serializers & algorithms)
2. exposes properties of business logic that matter for testing (i.e. the data that can't be arbitrary)
3. separates creating test data from test itself
4. generators are composable and reuseable
5. enables allows property based testing (optional)
6. fast test runs despite running test cases multiple times
7. much easier to test/"protect" small trivial methods since input generators already exist to generate input data (vs need to write another fixed input again)


## Scalacheck Cons

1. takes longer to write tests
2. random input can introduce intermittent tests
3. must never use suchThat (or any method that calls it: withFilter, filter, filterNot, posNum)
  - otherwise over time Scalacheck will fail intermittently as too many test cases are discarded & retried
4. complex to generate multi case class test data (test scenarios)
5. may not always generate important values in a particular test run

## Scenarios

A package of test data with certain pre-conditions satisfied

```
import org.scalacheck.Prop.forAll
import org.scalacheck.{ Gen, Arbitrary }
import java.util.UUID

case class Message(
  messageId: UUID,
  personId: UUID
)

val messageGen: Gen[Message] = for {
	messageId <- Gen.uuid
	personId <- Gen.uuid
} yield Message(
  messageId = messageId,
  personId = personId
)

case class TwoMessageScenario(
  first: Message,
  second: Message
) {
  require(first.personId == second.personId)	
}

val twoMessageScenarioGen: Gen[TwoMessageScenario] = for {
	first <- messageGen
	second <- messageGen
} yield TwoMessageScenario(
  first = first,
  second = second.copy(
    personId = first.personId
  )
)

// Get a random sample
println(s"TwoMessageScenario sample:\n${twoMessageScenarioGen.sample.get}")
println()

```
Scastie (scala fiddle): https://scastie.scala-lang.org/RC5c3RdHSJaW0Z1G8fKkbg

### Key points

1. TwoMessageScenario satisfies the pre-condition that both messages have the same personId
2. doesn't use suchThat just replaces data using copy in twoMessageScenarioGen

### Best practices

1. ScalaCheck is best for testing pure code (i.e. code has no side effects or async execution). It is especially helpful for testing serializers using a round trip of serializing generated data ensuring deserializing results in same data.
	1. When using ScalaCheck to round trip test serializers be sure to still write at least once test that either directly deserializes or serializes a sample or other fixed expected result. This avoids subtle serializer bugs that might not show up in round trip test.
2. Don't use `Gen.suchThat` (or any method that calls it such as `filter`). Instead just replace the data with a new generated value (e.g. in a case class using `copy` method)
3. Use scenario pattern for more complicated test data
4. It's ok if a generator can't be reused or needs to be duplicated since different tests will require different values (e.g. testing a serializer for a model vs testing business logic on the model)
6. Prefer generated (i.e. random) data for local testing only. Avoid using random data in production environments (e.g. for smoke tests/deployment validators) since if there is a clean up bug it makes difficult to identify the source of the random data.

## Existing CAT code examples

1. P-NonTelematics-Core CommercialOfferGen & CommercialOfferJsonSupportSpec
2. P-Telemetry-Alerts-Stream CommercialOfferGen (scenarios) & CommercialOfferAlertHandlerPureMethodsSpec


