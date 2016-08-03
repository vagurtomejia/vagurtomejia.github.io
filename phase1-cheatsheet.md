
#Classes

##Class variables
These are variables whose scope is limited to the class that they are defined in. Class variables are defined with @@ at the beginning of the name of the variable.

##Instance variables
These are variables whose scope is limited to one particular instance of a class. They are defined with @ at the beginning of the name of the variable.

##Accessors
###Short hand accessors
```ruby
attr_accessor :name, :position
attr_reader :id
attr_writer :other
```
###Long hand accessors

    def name=(new_name)
      @ = new_name
    end

##Inheritance

###How to define inheritance?

    Therapist < HealthProfessional
    Dentist < HealthProfessional

###Initialize method
We do not need to re-define the initialize method in Therapist and Dentist unless they need to do something specific and in that case we can :

    class Therapist < HealthProfessional
        def initialize
            super
        end
    end
super finds the method of the same name on the parent class and runs that, so we can use it any method, not just in initialize

#Modules
##Initialize Instance Variables Defined by a Module
ex: Here's a Timeable module that tracks when objects are created and how old they are:

    module Timeable
     attr_reader :time_created
     def initialize
       @time_created = Time.now
     end
     def age #in seconds
       Time.now - @time_created
     end
    end

Timeable has an instance variable time_created, and an initialize method that assigns Time.now (the current time) to the instance variable. Now let's mix Timeable into another class that also defines an initialize method:

    class Character
      include Timeable
      attr_reader :name
      def initialize( name )
        @name = name
        super() #calls Timeable's initialize
      end
    end
  
    c = Character.new "Fred"
    c.time_created
    # => Mon Mar 27 18:34:31 EST 2006

#Blocks

## Example of method that accepts a block and returns how many seconds it takes to execute the code in the block:

    def benchmark
      start_time = Time.now
      yield
      end_time = Time.now
      end_time-start_time
    end

##Example of use:
    long_string = "abcde" * 5000000
    reverse_run_time = benchmark { long_string.reverse }
    puts "The #reverse method took #{reverse_run_time} seconds to run."

#Regular expressions
##Examples of use
### Determine whether a string contains a Social Security Number.
def has_ssn?(string)
  string.match(/\d{3}-\d{2}-\d{4}/) != nil
end

### Find and return a Social Security Number.
def grab_ssn(string)
    string.match(/\d{3}-\d{2}-\d{4}/).to_s
end

### Find and return all Social Security Numbers.
def grab_all_ssns(string)
  string.scan(/\d{3}-\d{2}-\d{4}/)
end

#Errors
    class OrangeTree
      attr_reader :age, :height, :oranges
      # Define a custom exception class
      class NoOrangesError < StandardError
      end
      .
      .
      .
      # Returns an Orange if there are any
      # Raises a NoOrangesError otherwise
      def pick_an_orange
        raise NoOrangesError, "This tree has no oranges" unless self.has_oranges?
        # orange-picking logic goes here
        @oranges.pop
      end
    end

#RSpec


##Matchers
###Predicate matchers
Ruby objects commonly provide predicate methods:

    7.zero?                  # => false
    0.zero?                  # => true
    [1].empty?               # => false
    [].empty?                # => true
    { :a => 5 }.has_key?(:b) # => false
    { :b => 5 }.has_key?(:b) # => true
You could use a basic equality matcher to set expectations on these:

    expect(7.zero?).to eq true # fails with "expected true, got false (using ==)"
...but RSpec provides dynamic predicate matchers that are more readable and provide better failure output.

For any predicate method, RSpec gives you a corresponding matcher. Simply prefix the method with be_ and remove the question mark. Examples:

    expect(7).not_to be_zero       # calls 7.zero?
    expect([]).to be_empty         # calls [].empty?
    expect(x).to be_multiple_of(3) # calls x.multiple_of?(3)

Alternately, for a predicate method that begins with has_ like Hash#has_key?, RSpec allows you to use an alternate form since be_has_key makes no sense.

    expect(hash).to have_key(:foo)       # calls hash.has_key?(:foo)
    expect(array).not_to have_odd_values # calls array.has_odd_values?
In either case, RSpec provides nice, clear error messages, such as:

    expected zero? to return true, got false

Calling private methods will also fail:

    expected private_method? to return true but it's a private method

Any arguments passed to the matcher will be passed on to the predicate method.

[Reference](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/predicate-matchers)

###Collection membership 
    expect(actual).to include(expected)
    expect(array).to match_array(expected_array)
    # ...which is the same as:
    expect(array).to contain_exactly(individual, elements)

##Examples
    it "has a readable name" do
      expect(company.company_name).to eq("Monsters Inc.")
    end

    it "has a writable name" do
      expect { company.company_name = "Monsters Incorporated" }.to change { company.company_name }.to "Monsters Incorporated"
    end

    it "can add employees" do
      expect {company.add_employee("James P. Sullivan") }.to change { company.employees }.to match_array(["Mike Wasawsky", "James P. Sullivan"])
    end

    it "has an active status" do
      expect(house_listing.active?).to be false
    end

    it 'can read created_at' do
      expect(person.created_at).to eq DateTime.iso8601('2012-05-10T03:53:40-07:00')
    end

    it 'created_at is a DateTime object' do
      expect(person.created_at).to be_kind_of(DateTime)
    end

    it 'people returns an array of persons' do
      expect(parser.people).to be_kind_of(Array)
    end

    it "has a diameter between 2.5 and 3.2" do
      desired_diameter_range = (2.5..3.2)
      expect(desired_diameter_range).to cover orange.diameter
    end

    it "should change the age" do
      expect {tree.pass_growing_season}.to change {tree.age}.by(1)
    end

    it "should raise an error if the tree has no oranges" do
      expect{tree.pick_an_orange}.to raise_error("This tree has no oranges")
    end

    it 'returns a number from one to the number of sides' do
      die = Die.new
      expect((1..die.side_count)).to cover die.roll
    end

    context "the tree is old enough to bear fruit" do
      it "should cause the tree to produce oranges" do
        5.times { tree.pass_growing_season }
        oranges_unmature = tree.oranges.length
        tree.pass_growing_season
        oranges_mature = tree.oranges.length
        expect(oranges_unmature).to eq(0)
        expect(oranges_mature).not_to eq(0)
      end
    end



#Data structures
##String
[Ruby doc](http://ruby-doc.org/core-2.1.5/String.html)

##Arrays
[Ruby doc](http://ruby-doc.org/core-2.1.5/Array.html#method-i-delete_if)

###Examples of manipulation
    #échantillon
    [2.5, 2.6, 2.7, 2.8, 2.9, 3.0, 3.1, 3.2].sample

    #remove item that contains 'word'
    items.delete_if { |item| item.description.include?(word) }
    #or items.reject! { |item| item.description.include?(word) }

    people_alphabetically = people.sort{ |person2, person1| person2.first_name <=> person1.first_name  }

    people_with_area_code_419 = people.select { |person| /\A1-419/.match person.phone }

##Enumerables
[Ruby doc](http://ruby-doc.org/core-2.1.5/Enumerable.html)

    all? [{ |obj| block } ] → true or false

    any? [{ |obj| block }] → true or false

    find(ifnone = nil) { |obj| block } → obj or nil
      (1..100).find    { |i| i % 5 == 0 and i % 7 == 0 }   #=> 35

    find_all { |obj| block } → array
    Returns an array containing all elements of enum for which the given block returns a true value.
      (1..10).find_all { |i|  i % 3 == 0 }   #=> [3, 6, 9]

    find_index { |obj| block } → int or nil
      (1..100).find_index { |i| i % 5 == 0 and i % 7 == 0 }  #=> 34


#Arguments
##Named arguments for methods
One way to implement this design pattern is for our methods to expect a hash rather than individual arguments. The individual arguments become expected keys in the hash.

    album_details = {
      :title   => 'Kidz Bop 31',
      :artist  => 'Kidz Bop Kids',
      :color   => 'red',
    }
    Album.new(album_details)

And for retrieveing the values we use .fetch rather tan [] to avoid errors (in case the key is not present into the hash).
Examples of use:

    color = options.fetch(:color, "black")
    color = options.fetch(:color) { raise "You must supply a :color option!" }
    color = options.fetch(:color){ "blue" }

##Command line arguments
Accessible via the Array ARGV[]
To test if any command line arguments were given, use ARGV.any?. Example:

    if ARGV.any?
      collection = ARGV[0]
      item       = ARGV[1]
      option     = ARGV[2]


#Others
##Time
    require 'date'
    @created_at = DateTime.parse('2012-05-10T03:53:40-07:00')


#Persistance
##Databases
###SQLite
#### Create a database
Open the SQLite shell, connecting to a database as we do so:
```bash
$ sqlite3 database.db
```

This will take us to a SQLite shell prompt where we can execute SQL statements:

```text
SQLite version 3.8.10.2 2015-05-20 18:17:19
Enter ".help" for usage hints.
sqlite> 
```

#### Create a table
When we create a table, we declare the name of the table and each of the columns that we want to include in the table.  For each column we specify the column's name, [data type](http://www.sqlite.org/datatype3.html), and any [constraints](http://www.w3schools.com/sql/sql_constraints.asp) on the data.

Create the `users` table by entering the following SQL statement:

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  first_name VARCHAR(64) NOT NULL,
  last_name  VARCHAR(64) NOT NULL,
  email VARCHAR(128) UNIQUE NOT NULL,
  created_at DATETIME NOT NULL,
  updated_at DATETIME NOT NULL
);  /* Don't forget the semicolon */
```
[More on SQL CREATE TABLE statement](http://www.w3schools.com/sql/sql_create_table.asp)
[More on DATETIME function](http://www.techonthenet.com/sqlite/functions/datetime.php)


#### Check the Schema
When we're in the SQLite shell, we can check on the current schema of our database by entering `.schema`.  

#### Insert User Data

```sql
INSERT INTO users
(first_name, last_name, email, created_at, updated_at)
VALUES
('Kevin', 'Solorio', 'kevin@devbootcamp.com', DATETIME('now'), DATETIME('now')),
('Alyssa', 'Diaz', 'alyssa@devbootcamp.com', DATETIME('now'), DATETIME('now'));
```
[More on INSERT INTO statement](http://www.w3schools.com/sql/sql_insert.asp)

#### Select Records from a Table
```sql
SELECT * FROM users;
```
or
```sql
SELECT first_name,last_name FROM users;
```
[More on SELECT statement](http://www.w3schools.com/sql/sql_select.asp)

#### Alter a Table
```sql
ALTER TABLE users ADD nickname VARCHAR(64);
```
[More on SQL ALTER TABLE statement](http://www.w3schools.com/sql/sql_alter.asp)

#### Update a Record
```sql
UPDATE users
SET nickname='ksolo', updated_at=DATETIME('now')
WHERE id=1;
```
[More on SQL UPDATE TABLE statement](http://www.w3schools.com/sql/sql_update.asp)



##CSV
[Ruby docs - CSV library](http://ruby-doc.org/stdlib-2.1.0/libdoc/csv/rdoc/CSV.html)
###Read
    require 'csv'
    csv = CSV.read(@file, :headers => true, :header_converters => :symbol)
            {|row| row.to_hash}
OR

    require 'csv'
    csv = CSV.read(@file, :headers => true, :header_converters => :symbol)
    csv.each do |row|
       my_class_list << MyClass.new(row.to_hash) #or cal method add_my_class
    end
    csv.close

###Write
    csv = CSV.open(@file, "w+") #or a+
    #csv writer needs an array of strings per row
    csv << ["first_name", "last_name", "email", "phone", "created_at"] #headers
    new_people.each do |person|
        csv << person.to_a
    end
OR

    CSV.open("....", "wb") do |csv|
        csv << ["name", "created_at"]
        @things.each do |thing|
          csv << [thing.id, thing.name, thing.created_at]
        end
    end
[Complete list of open modes](http://ruby-doc.org/core-2.0.0/IO.html#method-c-new-label-IO+Open+Mode)

##TXT








[Phase 1 guide](https://github.com/chi-red-pandas-2016/phase-1-guide)
