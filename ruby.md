# Ruby guidelines

## Automatic enforcement with [Rubocop](https://github.com/bbatsov/rubocop)

To help us enforce similar style between project everyone should use rubocop from now on

https://gist.github.com/Ostrzy/c3debe7a47ccdf920591

Just save this to 
```
~/.rubocop.yml
```
Or add `.rubocop.yml` straight to project root folder( then it can be customized a little )

There are several plugins to editors that help you lint/autocorrect code with rubocop:
* ATOM
 * [Linter Rubocop](https://atom.io/packages/linter-rubocop)
 * [Rubocop Auto Correct](https://atom.io/packages/rubocop-auto-correct)
* SUBLIME
 * [Sublime Linter Rubocop](https://github.com/SublimeLinter/SublimeLinter-rubocop)

! Remember that you can use `(bundle exec) rubocop -a` which automatically fixes some cops violations.

Adding rubocop to [circleCi](https://circleci.com/) is best used with pre test commit hook in [`circle.yml`](https://circleci.com/docs/config-sample) file in project root folder.
```
test:
   pre:
     - bundle exec rubocop
```

## Rules

* Consider using `Hash#fetch` instead of `Hash#[]`.

  The first one throws exception where it fails to read a key.
  In the second case we usually get `undefined method ... for nil:NilClass`
  somewhere else in the code. The first one is better for debug.

* Use of new ruby hash syntax for new projects.

  Use old syntax only when necessary, for example to put non-symbol as a key.

* Always use double-quoted strings.

* Avoid rescuing StandardError and Exception

  They [should never be rescued](http://stackoverflow.com/questions/10048173/why-is-it-bad-style-to-rescue-exception-e-in-ruby#answer-10048406), if they are raised, we should get notified by getsentry and fix them.

* Use semantic versions for all gems in Gemfile before pushing to production.

* Always define dependent on AR relations. It is often a case that definition of `dependent` wasn't added because someone just forget about that. We should always do:

  ```
  has_many :objects, dependent: :destroy # when the object is destroyed, destroy will be called on its associated objects

  has_many :object, dependent: :delete_all # when the object is destroyed, all its associated objects will be deleted directly from the database without calling their destroy method (for has_one use :delete)

  has_many :objects, dependent: :nullify # causes the foreign key to be set to NULL. Callbacks are not executed

  has_many :objects, dependent: :restrict_with_exception # causes an exception to be raised if there is an associated record

  has_many :objects, dependent: :restrict_with_error # causes an error to be added to the owner if there is an associated object
  ```
  You shouldn't need to use `dependent` on `belongs_to`


* Try to avoid calling self explicitly on reads

  Prefer

  ```ruby
  def pay_with_balance?
    has_payment? && balance > 0 && payment_applied > 0
  end
  ```
  over

  ```ruby
  def pay_with_balance?
    self.has_payment? && self.balance > 0 && self.payment_applied > 0
  end
  ```

* Don't overuse one letter variables unless it is very short block, repeating variable or exception.

Acceptable:

```ruby
[1,2,3].map{ |e| e + 1 }
```

```ruby
rescue Exception => e
  # ...
end
```

```ruby
user.tap do |u|
  u.uid   = user_hash['uid']
  u.email = user_hash['email']
  u.name  = user_hash['name']
  u.save!
end
```

Not acceptable:

```ruby
if a = variant.address
  [a.country_code, a.state || a.city].join("-")
end
```

```ruby
array.inject({}) do |h, e|
  h[foo] = e.bar
  h
end
```

```ruby
def process_text(s)
  # ...
end
```

* Use trailing commas in **multiline hashes** and **multiline arrays**. If you'd like to add an element you won't left your comma on the previous line. It helps to keep VCS history clean.
```ruby
// Bad          // Good         // Bad            // Good
arr = [         arr = [         hsh = {           hsh = {
  'foo',          'foo',          foo: 'foz',       foo: 'foz',
  'bar'           'bar',          bar: 'baz'        bar: 'baz',
]               ]               }                 }
```

## Code style

* Do **NOT** indent methods below `private`/`protected` keywords

## Testing

* use a [new expectation syntax](http://myronmars.to/n/dev-blog/2012/06/rspecs-new-expectation-syntax) when using RSpec (see [issue](https://github.com/monterail/guidelines/issues/170)):

```ruby
it { expect(something).to be_valid }
```

* Use Node.js instead of therubyracer for execjs runtime
