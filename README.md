# InstacartChallenge

Instacart Challenge 


Problem Statement :
 - Parse a JSON file saved in the assets folder.
 - Create a "Quiz" which consists of a question and 4 images.
 - One image out of the 4 is the correct answer while the others are wrong.
 - Have a 30 second countdown timer to remind the user time remaining to answer the question.



Solution:

The way I structured the code is :

- Using the MVP pattern architecture for implementing this.
- Having the business logic happen in the presenter class "QuizPresenter" and its interface "QuizFetchPresenter"
- Having a view called "QuizView" handle all the UI/View related stuff.
- Have the main activity i.e "QuizActivity" implement this view interface to act like the connection between the presenter and the view.
- Have a Background AsyncTask i.e "ParseFoodItemsTask" run in the background to parse the JSON from the assets folder and save it into the Model Feed 	  called "QuizFeed". This Feed itself holds a list of Quizzes. We only require one quiz to be used for the purpose of the quiz.
- "FoodItem" is the individual Data model holding the image itself.
- Each Quiz determines which one is the winner among its list.
- The "VolleySingleton" class as the name suggests uses Volley to create a ImageLoader to be used for downloading the image and showing it in the image view itself.
- The "QuizItemsAdapter" is a simple RecyclerView Adapter which holds the content and the custom ViewHolder.

The reason I chose the MVP pattern was to make writing unit tests easier. Since all the business logic is held by the presenter, all we would need to do is write a Unit test for the Presenter class and have it test all the functionality out such as showing/hiding the countdown timer, having the wrong image selected and verifying a toast message is shown or not, making sure the images are displayed correctly, etc. I did not get the time to finish writing the tests so i wrote the skeleton code in the "QuizPresenterTest" class.
When the app starts , it creates a Volley instance and parses the json in the background. After the JSON is fully parsed, we pick at random one quiz to be used, we set the adapter to the recyclerView and let the viewholder take care of the image loading.
After the images have been loaded we start the countdown timer for 30s. 
If you select the wrong image then I overlay the image with a semi-transparent Red color overlay to justify the answer selected was wrong and a toast message is shown which says that this is not the right answer and the timer continues counting down.
After the right answer is selected I overlay the answer with a semi-transparent green color overlay to signify the right answer was selected as a visual cue to the user.
If you choose to Retake the quiz , I reset the countdown timer and the color filters are removed from the images.
If you choose to submit the quiz then for now i just store the answer you selected into SharedPreferences. In future iteration/work ,I would find a better way to store/post this (maybe an API call?)
If you hit the home button and the app gets backgrounded then I use the activity's onPause method as a entry to save the remaning time in the countdown timer and the current time in the sharedpreferences so that the next time you start up the app again and onResume is called I try to get the last saved time and see if there is enough time for me to continue the timer or create a new one.


TODO things left to do:
- Instead of using SharedPreferences to save the time and check the next time the app resumes for the countdown timer, have a background service do it.
- Avoid passing context objects around and implement a WeakReference model for passing context around.
- Finish writing the Unit Tests for the presenter. 

Feedback point :
The JSON needed to be fixed since the title Cottonelle Clean Care bathroom tissue needed to be surrounded by quotes since its a String. I had to manually add the quotes in to make the JSON being parsed cleanly. 
