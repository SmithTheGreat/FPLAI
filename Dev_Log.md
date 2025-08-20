This file documents my line of thinking while building and debugging this project.  
Each entry encases what I tried, what went wrong(if something was wrong), my reasoning, and the solution or next step.

## Log Entries:

### Entry #1 16/08/2025:

Problem: I noticed that the code is running based on the preseason_predicted_points method rather than the predicted_points_future_custom method that takes the factor weights(which varies based on the gameweek number)

Steps Tried: I tried to simply change the method called in the pipeline, however, I soon realized that the pred_points_future_cusotm method didn't accurately multiply the factor weights.
I changed the factor weight implementation, which began working.

### Entry #2 17/08/2025:

Problem: Yesterday, in the Manchester City vs Wolves game, 2 of the players the program picked, Ederson and Mataues Nunes didn't start, with the the former not even being in the squad(it was announced 2 days ago that he wouldn't come).

