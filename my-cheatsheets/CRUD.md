##Examples of tables and models

###Posts
```ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title, :content, :username
      t.integer :comment_count
      t.timestamps

    end
  end
end
```

```ruby
class Post < ActiveRecord::Base
  has_many :votes

  def points
    votes.sum(:value)
  end

  def time_since_creation
    ((Time.now - created_at) / 3600).round
  end
end
```
###Votes

```ruby
class CreateVotes < ActiveRecord::Migration
  def change
    create_table :votes do |t|
      t.integer :value
      t.belongs_to :post
    end
  end
end
```

```ruby
class Vote < ActiveRecord::Base
  belongs_to :post
end
```

##Examples of seed files
###For comments and votes
20.times do
  post = Post.create!( title: Faker::Company.catch_phrase,
               username: Faker::Internet.user_name,
               comment_count: rand(1000),
               created_at: Time.now - rand(20000))

  vote_count = rand(100)
  vote_count.times do
    post.votes.create!(value: 1)
  end
end