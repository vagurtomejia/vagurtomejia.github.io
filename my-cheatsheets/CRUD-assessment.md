
#Setting libraries
##Gemfile
    gem 'faker'
    gem 'bcrypt'
##Config/environment.rb
    require 'faker'
    require 'bcrypt'

#Setting migrations
##Users
###Generate migration
$ be rake generate:migration NAME=create_users

###Migration class (db/migrate/xxxx_create_users.rb)
```ruby
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name,           null:false
      t.string :email,          null:false,  unique: true
      t.string :password_hash,  null:false

      t.timestamps

      t.index :email,           unique: true
    end
  end
end
```

###Run migration
$ be rake db:migrate

##Other example
###Generate migration
$ be rake generate:migration NAME=create_entries
###Migration class (db/migrate/xxx_create_entries.rb)
```ruby
    class CreateEntries < ActiveRecord::Migration
      def change
        create_table :entries do |t|
          t.string    :title,   null: false, limit: 64
          t.string    :content, null:false
          t.string    :author

          t.timestamps(null: false)

        end
      end
    end
```
###Run migration
$ be rake db:migrate

#Setting models

##Model User

###Generate model
$ be rake generate:model NAME=user

###User class (app/models/user.rb)
```ruby
class User < ActiveRecord::Base

  validates :email, uniqueness: true
  validates_presence_of :email, :name, :password


  def self.authenticate(email, password)

    logging_user = User.find_by(email: email)
    if logging_user && logging_user.password == password
      return logging_user
    end
    nil
  end

  def password
    @password ||= BCrypt::Password.new(password_hash)
  end

  def password=(new_password)
    @password = BCrypt::Password.create(new_password)
    self.password_hash = @password
  end

end
```


#Examples of tables and models

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

##Useful shotcuts
- command + d para seleccionar varias palabras
- command + option + I para mostrar el inspector en el browser

##Problem solving
###Problems with the user cookies:
- open dev tools on the browser
- go to Application tab
- Storage
- Cookies
- click on localhost
- delete the rack.session cookie