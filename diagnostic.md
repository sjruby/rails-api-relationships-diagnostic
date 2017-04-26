# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
Lets you avoid storing data in more than one place, and is semantically eaiser to understand
than one giant flat file?
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  A profile has many favorites and movie is favorited many times? t
    Profiles - id given_name surname email
    Movies - id title relase_date length
    Favorites - id movie_id profile_id
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :favorites
    has_many :movies, through: :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :favorites
    has_many :profiles, through: :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movie
    belongs_to :profile
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
      They control what information is accessiable to the front end view, so in the below it is the "def" of title that is going to allow us to access the movies in the front end view associated witha  profile(b/c that's the JSON!?!?!?!)  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :email, :title

    def title
      object.movies.pluck(:id)
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    rails generate scaffold Favorites movie_id:reference pofiles:reference
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    A dependent destroy is when the delation of a a value in one table deletes all it's related records.
    For the netflix example above, when a user deltes a profile we don't want to store that profiles "favorites" anymore so we would add a dependent destroy
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    In a university setting a professor might have a one to many
    relation ship to students as an advisor.  One professor has many advisees.  They would have a many to many relationship to students as an instructor, one professor has many classes and classes have many students
  ```
