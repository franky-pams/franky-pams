# Data storage for quizzes and scores
quizzes = {}
user_scores = {}

def create_quiz():
    """Create and store a new quiz with multiple choice questions."""
    quiz_name = input("Enter the quiz name: ")
    quejstions = []
    num_of_questions = int(input("Enter the number of questions: "))
    
    for i in range(num_of_questions):
        print(f"\nCreating question {i + 1}")
        question_text = input("Enter the question text: ")
        options = []
        for j in range(4):  # Assuming 4 options per question
            option = input(f"Enter option {chr(65 + j)}: ")  # Use letters A-D for options
            options.append(option)
        correct_option = input("Enter the correct option (A-D): ").upper()
        
        # Convert letter to index (A=0, B=1, C=2, D=3)
        question = {
            "question": question_text,
            "options": options,
            "correct_option": ord(correct_option) - 65  # Convert to index
        }
        questions.append(question)
    
    quizzes[quiz_name] = questions
    print(f"Quiz '{quiz_name}' created successfully!")

# Initial quiz data
quizzes['general knowledge'] = [
    {
        "question": "What is the capital of France?",
        "options": ["Berlin", "Madrid", "Paris", "Rome"],
        "correct_option": 2  # C -> 3, but index is 2
    },
    {
        "question": "Which planet is known as the Red Planet?",
        "options": ["Earth", "Mars", "Jupiter", "Saturn"],
        "correct_option": 1  # B -> 2
    },
    {
        "question": "What is the largest ocean on Earth?",
        "options": ["Atlantic Ocean", "Indian Ocean", "Arctic Ocean", "Pacific Ocean"],
        "correct_option": 3  # D -> 4
    },
    {
        "question": "Who wrote 'Hamlet'?",
        "options": ["Charles Dickens", "William Shakespeare", "Mark Twain", "Jane Austen"],
        "correct_option": 1  # B -> 2
    }
]


def take_quiz():
    """Allow a user to take a quiz and record their score."""
    username = input("Enter your username: ")

    print("Available Quizzes:")
    for quiz in quizzes.keys():
        print(f"- {quiz}")
    quiz_name = input("Enter the quiz name you want to take: ")

    if quiz_name not in quizzes:
        print("Quiz not found!")
        return

    score = 0
    questions = quizzes[quiz_name]

    for i, question in enumerate(questions):
        print(f"\nQuestion {i + 1}: {question['question']}")
        for j, option in enumerate(question['options']):
            print(f"{chr(65 + j)}. {option}")  # Use letters A-D for options
        answer = input("Enter your answer (A-D): ").upper()
        
        if answer in ['A', 'B', 'C', 'D']:  # Check if input is valid
            answer_index = ord(answer) - 65  # Convert letter to index
            if answer_index == question["correct_option"]:
                print("Correct!")
                score += 1
            else:
                correct_answer = question["options"][question["correct_option"]]
                print(f"Wrong! The correct answer was: {correct_answer}")
        else:
            print("Invalid answer! Please enter A, B, C, or D.")
    
    print(f"\nYour score for '{quiz_name}' is {score}/{len(questions)}.")
    
    # Save the user's score for the quiz
    if username not in user_scores:
        user_scores[username] = {}
    user_scores[username][quiz_name] = score

def view_scores():
    """Display scores across different quizzes for a user."""
    print("Available users:")
    for user in user_scores.keys():
        print(f"- {user}")
    username = input("Enter your username: ")
    
    if username not in user_scores:
        print(f"No scores found for user '{username}'.")
        return

    print(f"\nScores for {username}:")
    for quiz, score in user_scores[username].items():
        max_score = len(quizzes[quiz])  # Calculate max score based on quiz length
        print(f"{quiz}: {score}/{max_score} points")

def main():
    """Main interface for the Quiz Management System."""
    
    while True:
        print("\n--- Quiz Management System ---")
        print("1. Create a Quiz")
        print("2. Take a Quiz")
        print("3. View Scores")
        print("4. Exit")

        choice = input("Enter your choice (1-4): ")

        if choice == '1':
            create_quiz()
        elif choice == '2':
            take_quiz()
        elif choice == '3':
            view_scores()
        elif choice == '4':
            print("Exiting Quiz Management System.")
            break
        else:
            print("Invalid choice! Please try again.")

main()
