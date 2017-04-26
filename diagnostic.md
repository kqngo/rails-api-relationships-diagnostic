# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
join tables prevents you from not having to duplicate your data in different tables

   ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
one to many:
  profile has many movies
  moveies belong to Profile

Many-to-many
  profile has many movies through favorites
  profile has many favorites
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base

  # one-to-many
  belongs_to :movies

  # many-to-many
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy

  # validation
  validates :title, presence: true
end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  # one-to-many
  belongs_to :profiles

  # many-to-many
  has_many :profiles, through: :favorites
  has_many :profiles, dependent: :destroy

  # validation
  validates :email, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :movie, inverse_of: :favorites
  belongs_to :profile, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
I like to think of serializers as a filter of results that you would get back from an SELECT * FROM <table>
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :given_name, :surname, :email
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  bin/rails generate scaffold movie given_name:string surname:string, email:string
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
If you have a dependency relationship, where a pair is made in the join table, deleting one would result in a deletion of the other. It makes for cleaner database table.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
 one-to-one example I can think of are:
  husband <--> wife

one-to-many examples:
  user <--> magezines
  customer <--> resturant
  students <--> teachers
  ```
