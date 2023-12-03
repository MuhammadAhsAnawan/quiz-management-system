// Here we Include necessary header files
#include <stdio.h>
#include <string.h>

// Constants
#define MAX_QUESTIONS 50
#define MAX_USERS 100
#define MAX_QUESTION_LENGTH 100
#define MAX_ANSWER_LENGTH 50

// Structures
typedef struct
 	{
    char question[MAX_QUESTION_LENGTH];
    char correctAnswer[MAX_ANSWER_LENGTH];
    int points;
	} Question;

typedef struct 
	{
    char username[50];
    int score;
    int answeredQuestions[MAX_QUESTIONS]; 				// Track answered question indices
	} User;

// Global Variables
Question quizQuestions[MAX_QUESTIONS];
User users[MAX_USERS];
int questionCount = 0;
int userCount = 0;

// Function prototypes
void addQuestion();							//-Prototype for add questions
void displayQuestions();						//-Prototype for displayall questions 
void takeQuiz(int userId);						//-Prototype for take Quiz 
int findUserIndex(const char *username);				//-Prototype for  to Find User Index by Username
void createUser();							//- Prototype for to Create a New User
void displayUserProgress(const char *username);				//-Prototype for to Display User Progress

// Main function
int main()
{
int choice;

    		do 
    		{
        	// Display menu
        	printf("\nWelcome to the Quiz Management System\n");
       		printf("1. Add a question\n");
        	printf("2. Display all questions\n");
        	printf("3. Take the quiz\n");
        	printf("4. Create a user\n");
        	printf("5. Display user progress\n");
        	printf("6. Exit\n");
        	printf("Enter your choice: ");
        	scanf("%d", &choice);

        	// Switch statement to handle user's choice
        	switch (choice) 
        	{
            	case 1:							//choice is 1.
                addQuestion();						//call function for Add a question
                break;
            	case 2:							//choice is 2.
                displayQuestions();					//call function for display all questions.
                break;
           	case 3: 
           	{							//choice is 3.
char username[50];							// ask user for inter the user name
		printf("Enter your username: ");
		scanf("%s", username);
int userIndex = findUserIndex(username);
                	if (userIndex != -1) 
                	{						// condition when inder number is not 1
                    takeQuiz(userIndex);
               		} 	
               		else 
               		{
                    printf("User not found. Create a new user.\n");
                	}
               	break;
            	}
            	case 4:							//choice is 4.
                createUser();						//call function for create a new user 
                break;
            	case 5:
            	{							//choice is 5.
char username[50];							// ask user for enter user name 
                printf("Enter username to display progress: ");
                scanf("%s", username);
                displayUserProgress(username);				//call function for the display of user progress
                break;
            	}
            	case 6:							//choice is 6.
                printf("Exiting the system. Goodbye!\n");
                break;
            	default:
                printf("Invalid choice. Please enter a valid option.\n");
        	}
   		}		 
   		while (choice != 6);					// here condition when choice is not equal to six

return 0;
}

// Function to add a question to the quiz
void addQuestion()
{
    			if (questionCount < MAX_QUESTIONS) 
    			{
       		//for question and store in the quizQuestions array
        	printf("Enter the question: ");
       		getchar(); 						// Clear the input buffer
        	fgets(quizQuestions[questionCount].question, MAX_QUESTION_LENGTH, stdin);
        	quizQuestions[questionCount].question[strcspn(quizQuestions[questionCount].question, "\n")] = 0; 
		// Remove the newline character
        	//  for correct answer and points
        	printf("Enter the correct answer: ");
       		scanf("%s", quizQuestions[questionCount].correctAnswer);

        	printf("Enter the points for this question: ");
        	scanf("%d", &quizQuestions[questionCount].points);

        	questionCount++;
        	printf("Question added successfully!\n");
   			 } 
   			 else 
   			 {
        	printf("Maximum question limit reached.\n");
    			}
}

// Function to display all the questions in the quiz
void displayQuestions() 
{
   			 if (questionCount == 0) 
   			 {
        	printf("No questions available in the quiz.\n");
    			} 
    			else 
    			{
        	// Display all quiz questions
        	printf("Quiz Questions:\n");
        	for (int i = 0; i < questionCount; i++) {
        	printf("%d. %s\n", i + 1, quizQuestions[i].question);
        		}
    			}
}

// Function to take the quiz
void takeQuiz(int userId)
{
int userScore = 0;
    		for (int i = 0; i < questionCount; i++) 
    		{
        		if (users[userId].answeredQuestions[i] != 1) 
        		{
            	// ask user for an answer
char userAnswer[MAX_ANSWER_LENGTH];
            	printf("Question %d: %s\nYour answer: ", i + 1, quizQuestions[i].question);
            	scanf("%s", userAnswer);

            	// Check if the answer is correct and update user's score
            		if (strcmp(userAnswer, quizQuestions[i].correctAnswer) == 0) 
            		{
                userScore += quizQuestions[i].points;
            		}

            	// Mark question as answered
            	users[userId].answeredQuestions[i] = 1;
        		}
    		}

    		// Update user's score
    		users[userId].score = userScore;
    		printf("Quiz completed. Your score is: %d\n", userScore);
}

// Function to find the user index by username
int findUserIndex(const char *username)
{
    		for (int i = 0; i < userCount; i++) 
    		{
        	// Search for the user in the users array
        		if (strcmp(users[i].username, username) == 0) 
        		{
return i;
        		}
    		}	
return -1; 								// Return -1 if user not found
									
}

// Function to create a new user
void createUser() 
{
    			if (userCount < MAX_USERS) 
    			{
        								// Prompt for a new username
        	printf("Enter your username: ");
        	scanf("%s", users[userCount].username);

        	// Initialize answeredQuestions array to 0 (not answered)
        	for (int i = 0; i < questionCount; i++) 
        	{
            	users[userCount].answeredQuestions[i] = 0;
        	}

        	userCount++;
        	printf("User created successfully!\n");
    			} 
    			else 
    			{
        	printf("Maximum user limit reached.\n");
    			}
}

// Function to display user progress
void displayUserProgress(const char *username) 
{
int userIndex = findUserIndex(username);
    			if (userIndex != -1) 
    			{
        	// Display user's progress, score, and answered questions
        	printf("User: %s\nScore: %d\n", users[userIndex].username, users[userIndex].score);
        	printf("Questions answered:\n");
        	for (int i = 0; i < questionCount; i++) 
        	{
            		if (users[userIndex].answeredQuestions[i] == 1) 
            		{
               	printf("Question %d: Answered\n", i + 1);
            		} 
            		else 
            		{
               	printf("Question %d: Not Answered\n", i + 1);
            		}
        	}
    			} 
    			else 
    			{
        	printf("User not found.\n");
    			}
}

