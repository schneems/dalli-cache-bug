## Setup

- git clone
- bundle install

## On the console

```
ModelWithKeyAndVersion = Struct.new(:cache_key, :cache_version)
model_1_cache_1 = ModelWithKeyAndVersion.new("model/1", "1")
model_1_cache_2 = ModelWithKeyAndVersion.new("model/1", "2")

Rails.cache.fetch(model_1_cache_1) { "woohoo" }
puts Rails.cache.read(model_1_cache_1) # => "woohoo"
puts Rails.cache.read(model_1_cache_2) # => "woohoo"
```

We expect different objects with a different cache_version to return different results.

## In the View

```
bundle exec rake db:migrate
rails s
```

Then visit: [http://localhost:3000/users](http://localhost:3000/users)

What does this page do? It creates an user object with name of "schneems" it renders a partial without cache, then renders the same partial twice with cache, note that the timestamp is exactly the same.

The view then modifies the object to have a name of "richard" and saves it. This should modify the `cache_version`. You can see the name change in the uncached partial render, however when the cache is used it displays "schneems" rather than "richard".

![](https://www.dropbox.com/s/ww0tm415d8y7sfu/Screenshot%202018-09-18%2009.21.41.png?raw=1)

In short modifying an object does not appear to clear it's cache when using dalli.
