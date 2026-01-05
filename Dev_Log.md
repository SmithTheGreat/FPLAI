This file documents my line of thinking while building and debugging this project.  
Each entry encases what I noticed was wrong, my reasoning for why my initial code was wrong, what I tried to fix it, what went wrong during my implementation(if something was wrong), and the solutions or next steps I am taking.

## Log Entries:

### Entry #1 07/15/2025:

I realized that predicted points for some players were extremely high when they had easy fixtures. After checking the calculations, I saw that the team strength ratio could get very large when the opponent’s strength was low. To fix this, I clipped the strength_factor so it stays between 0.5 and 1.5, which immediately made the predictions more stable and stopped certain players from being massively overvalued.

### Entry #2 07/26/2025:

Some players were getting unrealistically low predicted points, especially early in the season. After tracing the logic, I realized that base_ppg could end up being very small when both last season and current season data were limited. I fixed this by adding a lower bound so base_ppg never drops below 0.5, which kept active players from being unfairly punished.

### Entry #3 08/5/2025:

While testing players with no fixture in a given gameweek, I noticed that their predicted points were sometimes dropping to zero, which didn’t really make sense. I fixed this by adding a fallback in the prediction logic so that when no fixture is found, the code uses the player’s base_ppg instead of zero. This way, missing fixtures don’t completely remove a player’s value.

### Entry #4 08/13/2025:

I noticed that the factor weights weren’t always summing to 1, which made it hard to reason about how much each component was contributing. I fixed this by normalizing the weights inside the factor_weight function so they always add up to 1. This made the weighting behavior much easier to understand and debug.

### Entry #5 08/15/2025:

I noticed that the code is running based on the preseason_predicted_points method rather than the predicted_points_future_custom method that takes the factor weights (which varies based on the gameweek number). I tried to simply change the method called in the pipeline, however, I soon realized that the predicted_points_future_custom method didn't accurately multiply the factor weights. I changed the factor weight implementation, which began working.

### Entry #6 08/17/2025:

Yesterday, in the Manchester City vs Wolves game, 2 of the players the program picked, Ederson and Matheus Nunes didn’t start, with the former not even being in the squad (it was announced 2 days ago that he wouldn’t come with the rest of the team). I needed to find a way to make a more robust availability checker. My two thoughts were using a verified lineup predictor, which needed a subscription, or only picking players that played at least 60% of the team’s total minutes to ensure that the players picked are starters and not substitutes. I implemented the latter for the meantime, but I do recognize it will be an issue when a player is coming back from a long injury or gaining or losing favor.

### Entry #7 08/18/2025:

Now that the first gameweek has officially ended, I noticed that when I try to run the program, I receive predictions that are close to zero across the board. After searching for the issue, I realized that my data source, https://fantasy.premierleague.com/api/bootstrap-static/, had updated to the new season’s data, which meant my reliance on last season’s statistics disappeared overnight. I found another endpoint, https://fantasy.premierleague.com/api/element-summary/{player_id}/, which provided previous season data. Since this endpoint only works one player at a time, I adjusted the predict_points_future_custom method to operate on individual player IDs and return a single prediction.

### Entry #8 08/21/2025:

Today I noticed that when a new gameweek passes, the code generates a completely new team, which makes it impossible to suggest transfers. To fix this, I added logic to suggest transfers by taking the user’s current squad as input. While testing this, I noticed that many suggested transfers were still over budget, even though I had already set a budget constraint. As a temporary solution, I also added a separate function that finds the best player to buy under a given budget for a specific position.
