class: center, middle, inverse

# GOOD
### Good Object-Oriented Design
Agnieszka Matysek

---

class: middle, center

# How looks your app?

???

- It is beautiful?
- Or maybe you hate it

---

class: middle, center

## Like this?

.large-image[![Big mess](./images/mess.jpg)]

???

- Everything is connected to everything
- You can not add new features
- Tests are slow
- Your development is slow
- All of that is so frustrating

---

class: middle, center

# What happened?

???

- It was so beautiful on start
- You take care of everything
- But ...

---

class: middle, center

# What happened?

It changed.

---

class: middle, center

# It only changed.

This is it.

???

- only stable/static thing is change
- we can tell only one thing about future of your application - It's gone change.

---

class: center, middle, inverse

# GOOD
### Gratifying Object-Oriented Design

---

class: middle

# Agenda

- Rules
- Null Object
- SOLID
- Dependency Injection
- Refactoring
- Go write some code

---

class: inverse, middle, center

# Rules

---

class: middle

# Rules

- TDD
- BDD
- DRY - don’t repeat yourself
- KISS - Keep It Simple, Stupid
- LoD - Law of Demeter
- SOLID


- 100 lines per class
- 5 lines per method
- 4 params per method call (and don't even try cheating with hash params)
- 1 instance variable per controller action

???

- .strong[Law of Demeter (LoD)]
  - Each unit should have only limited knowledge about other units: only units "closely" related to the current unit.
  - Each unit should only talk to its friends; don't talk to strangers.
  - Only talk to your immediate friends.
- we back to .strong[SOLID] in a minute

---

class: inverse, center, middle

All talk about
# Manage Dependencies

---

class: center middle

All of this rules
# Is not enough

---

class: center, middle, inverse

We must know and really understand
# What Object oriented design is?

---

class: center, middle

## Object-oriented design is the process of planning a system of interacting objects for the purpose of solving a software problem.

???

- Wikipednia say that
- It this definition useful for us? Not really.
- We must understand what is .strong[OOD]?
- How deal with .strong[OOD]?
- How deal with refactoring?
- One of my university teacher say: .italic[You don't know how resolve this problem because you don't understand it.]
- So let start and try to understand!

---

class: center, middle, inverse

# Null object

---

class: middle

# Example

```ruby
user1 = User.find_by(name: 'Agnieszka') => #<User:0x00058831f0 name: 'Agnieszka'>

user2 = User.find_by(name: 'John') => nil
```

#### What happened?

```ruby
user1.name => 'Agnieszka'

user2.name => NoMethodError: undefined method `name' for nil:NilClass
```

???

- It's like `nil` don't mean nothing
- It mean something

---

class: middle

# We try hiding check type

```ruby
user.try(:name)

user && user.name

user ? user.name : ''

user.nil? ? '' : user.name

user == nil ? '' : user.name
```

---

class: middle

### All of this is hidden `if`:

```ruby
if user
  # do something
else
  ''
end
```

## but this is simply Type check

```ruby
if user.is_a?(User)
  # do something
else
  ''
end
```

---

class: middle, center, inverse

This code is
# Everywhere

???

- is everywhere
- is not DRY
- is procedural `if`
- many dependences
- many place to change
- `User` and `nil` do not respond on the same API

---

class: middle, center

# How deal with this?

???

- create object that answer on the same API

---
class: middle

# First step

```ruby
class MissingUser
  def name
    ''
  end
end
```

## And we can use this

```ruby
User.find_by(name: 'John') || MissingUser.new()
```

???

- new dependency
- hide condition ||
- everywhere is MissingUser
- but now we sent .strong[message] to object

---

class: middle

# What if there were no `if`?

```ruby
class TrustUser
  def find_by(hash)
    User.find_by(hash) || MissingUser.new
  end
end
```

???

- now we have dependency in one place
- everywhere we use new object `TrustUser`
- it look like decorator

---

class: inverse, middle, center

This has name
# Null Object Pattern

---

class: middle, center

# Dependency Injection

???

- we inject different behaviors to class
- class are independent, we can mock in test
- mock only class
- mock only other not you by self

---

class: middle, inverse, center

# Composition over inheritance
### (Composite Reuse Principle)
Is the principle that classes should achieve polymorphic behavior and code reuse by composition, instead of through inheritance (being a subclass).

---

class: middle, center

# SOLID

---

class: middle

# SOLID

- S - Single Responsibility
- O - Open/Closed
- L - Liskov Substitution
- I - Interface Segregation
- D - Dependency Inversion

---

class: middle

# Let's talk about SOLID

```ruby
['S', 'O', 'L', 'I', 'D'].shuffle
```

---

class: middle, center

# I - Interface Segregation
Many client specific interfaces are better then one general purpose interface.

???

- it is important in strong typing language
- we don't care because we use Ruby - dynamic typing language

---

class: middle, center

# L - Liskov Substitution
Subclasses should be substitutable for their base classes.

???

- subclasses
- If you have class `Foo` and `Foo` have subclasses `Fooish`. Then everywhere you use `Foo` you can use `Fooish`
- It's mean you can not use `is_kind_of?` method

---

class: middle, center

# D - Dependency Inversion
Depend upon abstractions. Do not depend upon concretions.

???

- Dependent on things that are more stable then you are
- show how it look: Stable class, Not so stable class, Configuration
---

class: middle, center

# S - Single Responsibility
There should never be more then one reason for a class to change.

---

class: middle, center

# O - Open/Close
The module should be open for extension but closed for modification.

???

- adding new features without changing existing code

---

class: inverse, middle, center

# Refactoring
Some tips

---

class: middle

# When do refactoring?

- Is it DRY?
- Does it have one responsibility?
- Does everything in it change at the same rate?
- Does it depend on things that change less often then it does?

???

- if answer on at least one question is .strong[no] then you should refactor
- we simply use .strong[SOLID] as our rules

---

class: middle

1. Identified code smell
2. Change (Extract Role)
3. Inject again to the class (Dependency Injection)

### Example
```ruby
class Foo
  def initialize(bar = Bar.new)
    @bar = bar
  end
end
```
---

class: middle

# Summary

- Always send message to object
- TDD/testing is hard when you have bad design
- GOOD Design cost time on start but it is profitable in the future
- Think what you do and thing why you do
- If you have `if` .strong[STOP] and thing
- Duplication is far cheap then wrong abstraction

???

- we learn DRY at the begin because we understand simple rules not abstraction
- Make the change easy (this can be hard) to make easy change.

---

class: middle, inverse, center

# Design because you expect your application to succeed and the future to come.
#### Sandi Metz
---

class: middle, inverse

# Bibliography

- https://en.wikipedia.org/wiki/Object-oriented_design
- https://en.wikipedia.org/wiki/Object_composition
- https://en.wikipedia.org/wiki/Composition_over_inheritance
- https://en.wikipedia.org/wiki/Law_of_Demeter


- [RailsConf 2015 - Nothing is Something](https://www.youtube.com/watch?v=29MAL8pJImQ)
- [RailsConf 2014 - All the Little Things by Sandi Metz](https://www.youtube.com/watch?v=8bZh5LMaSmE)
- [Rails Conf 2013 The Magic Tricks of Testing by Sandi Metz](https://www.youtube.com/watch?v=URSWYvyc42M)
- [Baruco 2013: Rules, by Sandi Metz](https://www.youtube.com/watch?v=npOGOmkxuio)
- [GoRuCo 2011 - Sandi Metz - Less - The Path to Better Design](https://vimeo.com/26330100)
- [GORUCO 2009 - SOLID Object-Oriented Design by Sandi Metz](https://www.youtube.com/watch?v=v-2yFMzxqwU)


- [The Gilded Rose Code Kata](https://github.com/jimweirich/gilded_rose_kata)
- [SandiMeter](https://github.com/makaroni4/sandi_meter)
- [Rails Style Guide](https://github.com/fractalsoft/rails-style-guide)
- [Ruby Style Guide](https://github.com/fractalsoft/ruby-style-guide)

---

class: middle, inverse

- https://www.flickr.com/photos/14915441@N07/3622125089/in/photolist-6w5kAz-6Er1Pc-bg8W1-9j8UnG-9tJgJV-47Nf4e-eqgPSy-eUqyD-hQN1Jw-koVXTM-e88nnZ-9j5LBK-5zNDgG-jySVXM-4uNsCX-7iuh42-4jYxpy-4sr2jt-9Pd4Z-4gm9N6-b9hfaR-dVJKrK-4LHvCS-5qNR5L-eewWz-dCaA8Q-4EjNgb-73a1eg-7YyP6U-qaPQ4-9VdCUb-9dDY7N-7YyMus-8MBUs-dgEs3y-zZjSK-4xgXB7-qaPQP-6Qfw98-fMEpmV-qaPQj-4K3fD6-aB4Vgf-dGvu1B-4Hn6kS-cu4nm3-pybGg-bF1XGL-4LoLy-jN6RPH

---

class: middle, center

.small-image[![Womanonrails](./images/womanonrails.png)]
### Agnieszka Matysek
[@womanonrails](https://twitter.com/womanonrails)

amatysek@fractalsoft.org
