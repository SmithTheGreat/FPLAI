This file documents my line of thinking while building and debugging this project.  
Each entry encases what I tried, what went wrong(if something was wrong), my reasoning, and the solution or next step.

## Log Entries:

### Entry #1 16/08/2025:

I noticed that the code is running based on the preseason_predicted_points method rather than the predicted_points_future_custom method that takes the factor weights(which varies based on the gameweek number). I tried to simply change the method called in the pipeline, however, I soon realized that the pred_points_future_cusotm method didn't accurately multiply the factor weights.
I changed the factor weight implementation, which began working.

### Entry #2 17/08/2025:

Yesterday, in the Manchester City vs Wolves game, 2 of the players the program picked, Ederson and Matheus Nunes didn't start, with the the former not even being in the squad(it was announced 2 days ago that he wouldn't come with the rest of the team). I needed to find a way to make a more robust availability checker. My two thoughts were using a verified lineup predictor, which needed a subscription, or I could only pick players that played atleast 60% of the team's total minutes to ensure that the players picked are starters and not substitutes. I have implemented the latter for the mean time, but I do recognize that it will be an issue for when a player is coming back from a long injury or when a player is either getting into favor or getting out of favor.

### Entry #3 18/08/2025:

Now that the first gameweek has officially ended, I have noticed that when I try to run the program, I recieve every prediction as near zero numbers. I was searching for the issue when I finally figured out that my data source, "https://fantasy.premierleague.com/api/bootstrap-static/" had updated to contain the new season's data, which meant that my reliance on last season's statistics was gone overnight. I found another section of the premier league website, "https://fantasy.premierleague.com/api/element-summary/{player_id}/", which gave me the previous season data. However, because the new source only takes 1 player_id at a time, to make it easier, I made the predict_points_future_custom method only take in one player_id and return only 1 point prediction.

### Entry #4 21/08/2025: 

Today I noticed that running the code simply gave me a completely new team when the previous gameweek passes, which makes it impossible to suggest transfers. So, I added a method to get transfers through inputting your current team. I noticed that the options I recieved were all overbudget, although i had already placed that as a strict parameter. So, for the mean time, I also added cell that finds the best player to buy under a certain budget in a certain position.


