> Our quiz will ask questions until a user achieves mastery.

> Once we have templates that create questions, we can use them to build quizzes.

talk about our overall strategy.
We’ll start with a set of templates, organized by category. We’ll cycle through
the templates, one at a time. Once the user gets enough right in a row, we’ll
stop asking that question.
Given that set of directions, we’ll need to keep track of the following.
For the overall quiz, we’ll need to name the quiz, and we’ll need to let the user
specify how many answers a user will need to get correct before we finish
asking the question:


```elixir
alias Mastery.Core.{Template, Quiz, Response}

generator = %{left: [1,2], right: [1,2]}

checker = fn(sub, answer) -> sub[:left] + sub[:right] == String.to_integer(answer) end

quiz = 
  Quiz.new(title: "Addition", mastery: 2) |> Quiz.add_template(
    name: :single_digit_addition,
    category: :addition,
    instructions: "Add the numbers",
    raw: "<%= @left %> + <%= @right %>",
    generators: generator,
    checker: checker 
  ) |> Quiz.select_question

# Creating an incorrect response

email = "jill@example.com"

response = Response.new(quiz, email, "2")

quiz = Quiz.answer_question(quiz, response)

quiz.record

# Creating a correct response

quiz = Quiz.select_question(quiz)

quiz.current_question.asked

response = Response.new(quiz, email, "3")

quiz = Quiz.answer_question(quiz, response)

quiz.record
```