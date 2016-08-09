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

###Run migration
$ be rake db:migrate

##Other example
###Generate migration
$ be rake generate:migration NAME=create_entries
###Migration class (db/migrate/xxx_create_entries.rb)
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
###Run migration
$ be rake db:migrate

#Setting models

##Model User

###Generate model
$ be rake generate:model NAME=user

###User class (app/models/user.rb)
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