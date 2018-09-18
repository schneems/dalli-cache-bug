##

- git clone
- bundle install
- rails console

```
ModelWithKeyAndVersion = Struct.new(:cache_key, :cache_version)
model_1_cache_1 = ModelWithKeyAndVersion.new("model/1", "1")
model_1_cache_2 = ModelWithKeyAndVersion.new("model/1", "2")

Rails.cache.fetch(model_1_cache_1) { "woohoo" }
puts Rails.cache.read(model_1_cache_1) # => "woohoo"
puts Rails.cache.read(model_1_cache_2) # => "woohoo"
```