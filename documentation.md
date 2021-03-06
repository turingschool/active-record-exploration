## Active Record Documentation

Ben

  * AR Method: 
  * Description:
  * Example:
  * Anything else?:

Brendan

  * AR Method: `average`
  * Description: Given one column on a table, `average` finds the mean of the values in that column.
  * Example: `Item.average(:price)`
  * Anything else?: Conditions for what values to average can be passed in as an argument, like
   + `Item.average(:price, conditions:["price < 50"])`

Brian

  * AR Method: Sum
  * Description: Sum calculates the sum of all values on a given column, and will return the same datatype of these values 
  * Example: Company.sum(:revenue) => 100000
  * Anything else?: If there is no row in the column (i.e. no data), the method will return 0

Goss

  * AR Method: Select  
  * Description: Select allows us to return an active record Relation object with data from one or more specific columns from a table. This is a collection of rows from the table, where each object has the data specified in the Select for the row it represents. Any columns excluded from the Select will not be included in the return value, they will be set to nil; even the id will be nil unless you include it in the Select. It can accept symbols as arguments, an it will return columns with the same name as the symbols. Alternatively, you can pass a string which will be embeded in a SQL Select statement. This is useful for doing calculations or manipulation on each row returned by the select. Select is usually the second method in a chain of Active Record methods, after the table or collection and before methods that transform or limit the result.  
  * Example: Students.select(:name, :email).order(:name); Teachers.select('average(Time.now - hire_date) as seniority');  
  * Anything else?: Select only populates the data you ask for. A collection built from a select method will not include any of the columns not listed.  

Chase 

  * AR Method: Group
  * Description: Returns an array of unique objects based on whatever attribute you have passed it.  
  * Example: In a scenario where there are many users named Chase, and many users named Bob, Users.group(:name) will return [#<User id: 1, name: bob, state: Texas> #<User id:2, name:Chase, state: Texas>].  
  * Anything else?: Group is almost always follwed by a .count.   I THINK this is because grouping them all together isn't very useful unless we are trying to find the number of each identical attribute that exists.  

Calaway
  * AR Method: having
    * Description: `having` is always used in conjuction with `group`. Think of it as "group where." (Note that `where` cannot be called after `group`, hence the need for a different method).
  * Example: 
    * Find all products for which the price is the same as at least 4 other products. Said another way, group all products by those that have the same prices, then count how many items share the same prices, then return only the collections of products for which that count is 5 or more. So group by price where that count is greater than 4.
    * `Product.group(:price).having("count(price) > 5")`
  * Anything else?:
    * Input: result of a group query (an ActiveRecord object)
    * Output: new ActiveRecord object with a collection of elements matching the group/having combination.

Dan

  * AR Method: Count
  * Description: There are three ways to use .count: count all, count by column, and count with options.<br />

	**Count All:**  If you don’t pass any parameters, count will return a count of all the rows in a model.<br />
	**Count by Column:** You can pass a column name to count, and it will count all the rows where that column is present.<br />
	**Count with Group or Select:** You can call count after .group or .select to get more specific counts.<br />
  * Example:<br />
	**Count All:**  <br />
		Car.count<br />
		# -> The count of all the cars<br />

	**Count by Column:** <br />
		Car.count(:sunroof)<br />
		# -> All the cars that have a value in the sunroof column<br />

	**Count with Group or Select:** <br />
		Car.group(:make).count<br />
		# -> { ’Nissan’ => 80 , ‘Mazda’ => 53 , ’Tesla’ => 1337 }<br />
		You can also group multiple columns and then get the count:<br />
		Car.group(:model, :color)<br />
		# -> { [‘Model S’, ‘red’] => 74, [‘GTR’, ‘silver’] => 55 }<br />
		Car.select(:year).count<br />
		# -> Count of the different year values in the year column.<br />
  * Anything else?:  Not all valid select methods will work with count.  If it is invalid an error will be thrown

David Davydov

  * AR Method: 	`save`
  * Description: This method creates an already existing object in the database. It returns false if it wasn't able to be created, possibly because of failed validations like `presence: true`.
  * Example: 
  
  ```ruby
  Student.all.count => 0
  s1 = Student.new(name: 'bob')
  s1.save => true
  s2 = Student.new
  s2.save => false
  bob = Student.new; bob.save
   ```
   
  * Anything else?: `create` is the same as `new` and `save`. `new` makes the object, and `save` saves it in the actual database.

Jasmin

  * AR Method: LIMIT
  * Description: This class method allows you to control the number of records you wish to retrieve from a table. 
  * Example: `Student.limit(2) #=> Only retrieves the first two records in the Students table`
  * Anything else?: In conjunction with `#limit`, you can also use `#offset` to specify the number of records to skip before starting to return the records. Example: `Student.limit(3).offset(30) #=> Grabs the first 3 records after skipping the first 30`

Jean

  * AR Method: Order
  * Description: Order will order a collection according to the value of the specified attribute
  * Example:
   *  `Student.order('name')` is the same as `Student.order(:name)`, both order by the value of name
   *   `Student.order('created_at DESC')` is the same as `Student.order(created_at: :desc)`, both order by the value in the specified ascending/descending order
  * Anything else?:
   *  ordering by letters will order alphabetically, while ordering numbers will order numerically. You can also chain ordering - e.g. `Student.order(:name).order(updated_at: :asc)` will order first alphabetically by name then by ascending update time

Jesse Spevack

  * AR Method: PLUCK
  * Description: Pluck can be used to query one or more columns from the underlying table of a model. It accepts a list of column names as argument and returns an array of values contained in those columns.
  * Example: Course.pluck(:name) => [American History, Ancient History]; Course.pluck(:name, :instructor) =[[American History, Smith], [Ancient History, Jones]]
  * Anything else?: Pluck directly converts a database result into a Ruby Array, without constructing ActiveRecord objects. This can mean better performance for a large or often-running query. It also means that one can not use ActiveRecord methods on the array or array elements returned by a pluck call. In the above example, Course.pluck(:name) => [American History, Ancient History]; We could not then treat American History as an ActiveRecord object, it is just the data from the Name column of the Course table.

Matt

	* AR Method: `find_or_create_by(attributes, &block)`
	
	* Description: Finds the first record with the given attributes or creates the record with the attributes if the record is not found
	
	* Example:

	#### Say we have a table of singers with columns first_name, last_name, and genre

    Singer.find_or_create_by(first_name: 'Taylor', genre: 'Pop')
      # => #<Singer id: 1, first_name: 'Taylor', last_name: nil, genre: 'Pop'>
    Singer.find_or_create_by(first_name: 'Elvis')
      # => #<Singer id: 2, first_name: 'Elvis', last_name: nil, genre: nil>

	#### Will create a new singer if you do not validate the fields

    Singer.find_or_create_by(genre: 'Pop')
      # => #<Singer id: 1, first_name: 'Taylor', last_name: nil, genre: 'Pop'>
    Singer.find_or_create_by(genre: 'Rock')
      # => #<Singer id: 3, first_name: nil, last_name: nil, genre: 'Rock'>
      
	#### Can run a block as well

    Singer.find_or_create_by(first_name: 'Justin') do |singer|
        singer.last_name = 'Bieber'
        singer.genre = 'Bad'
    end
      # => #<Singer id: 4, first_name: 'Justin', last_name: 'Bieber', genre: 'Bad'>


  * Anything Else?: If you don't have your fields validated on creation, it will create attributes with nil values as long as it doesn't find the attributes you pass in. This method only returns one object with the specific matching attributes.

Nate

  * AR Method: Distinct
  * Description: Specifies whether the record returned should be unique or not.
  * Example: Teacher.select(:name) <= Might return two records with the same name
             Teacher.select(:name).distinct <= Returns one record per distinct name
  * Anything else?: You can also remove the uniqueness by placing two distincts consecutively and placing false in parens 
  at the end. ex. Teacher.select(:name).distinct.distinct(false)

Raphael

  * AR Method: Offset
  * Description: Essentially it specifies the number of rows that needs to be skipped before returning rows.
  * Example: Artist.offset(10)
  * Anything else?: Let's say we have a total Artist.count => 12. When I call the ActiveRecord method "offset" with a given value of 10, it will return the following:  Artist.offset(10) => #<ActiveRecord::Relation [#<Artist id: 11, name: "Tommy Guerrero", created_at: "2016-08-31 15:51:27", updated_at: "2016-08-31 15:51:27">, #<Artist id: 12, name: "Ray Barbee", created_at: "2016-08-31 15:51:27", updated_at: "2016-08-31 15:51:27">]>.

Ryan

  * AR Method: includes
  * Description: Class method allowing a user to call only the columns with a specified attribute.
  * Example: 
```ruby
@companies = Company.includes(:persons).where(:persons => { active: true } ).all
 
@companies.each do |company|
     company.person.name
end
```
  * Anything else?: This method is similar to joins, but as the Rails docs states, includes loads the specified associations using a minimal amount of queries.  This, in turn, leads to better performance than joins.

Sonia

  * AR Method: take
  * Description: It returns an array containing a specificed number or default number of objects from a class. The array is not necessarily in a particular order.
  * Example: `PayloadRequest.take` returns an array containing one `PayloadRequest` object, the first one created. `PayloadRequest.take(5)` returns the first five objects in the collection, but not necessarily in order.
  * Anything else?: It is important to distinguish between .take and .limit. The latter returns an ActiveRecord relation which means you can chain more methods to it, unlike with .take.

Susi

  * AR Method: `.create!`
  * Description: This will create (meaning, perform both new and save in the database) an object or multiple objects. The difference between between create and create! is that create! will raise a "RecordInvalid" error if the validations for the object fail, whereas create on its own does not do this.
  * Example: If you want to create a new student in the database and it requires first name and last name, you might want to record an error if a field is left blank rather than only not save the object.
  * `student = Student.create!(last_name = "Smith")`
 the return would be something like:
  `puts invalid.record.errors`
 It would return to you an ActiveRecord::RecordInvalid error and completely halt your application
  * Anything else?:  This is very similar to the way save! operates. Bang methods should really only be used in development processes. Production should be user-freindly as far as errors and usability go.
  
