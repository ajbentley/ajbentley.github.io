---

title: Surviving the Titanic Disaster

---

This week's project was to utilize logistic regression to determine what factors would be most descriptive of a Titanic passenger's likelihood of survival.

If you read my previous post, A Titanic Travesty, you'll know that the first question I asked myself when looking at this project was whether the data was reliable. It was a century old and records weren't kept as strictly or preserved as well as they are now. I was also sensitive to the fact that a private enterprise making significant income from the emigration business might find itself with a number of unticketed passengers. Then there was the matter of the hundreds of crew members unaccounted for in the tally. This was really terrible data.

Then I remembered that this was a training exercise and to stop overthinking the source of the data. That could wait for the real world.

The data included

- PassengerId - Identifier of individual passenger
- Survived - Conditional identifier (0 = fatality, 1 = survivor)
- Pclass - Passenger class (1 = 1st, 2 = 2nd, 3 = 3rd(aka steerage))
- Name - Passenger name
- Sex - Gender of passenger
- Age - Age of passenger
- SibSp - Number of passenger's Siblings and/or Spouses aboard
- Parch - Number of passenger's Parents and/or Children aboard
- Ticket - Ticket number
- Fare - Amount paid for ticket
- Cabin - Deck (letter) and room number
- Embarked - Port at which passenger boarded

A number of potential analyses presented themselves immediately:

- Pclass: Were passengers of one class more/less likely to survive (MLL) than other classes?
- Name/SipSp/Parch: were people with families MLL than people who were traveling alone?
- Name: Is it possible to make assumptions of ethnicity based on family name? If so, were people of an ethnicity MLL?
- Sex: Were men/women MLL?
- Sex / Age: Were boys/girls MLL? Possibly bin ages
- Age: Did age play any part in MLL? Possibly bin ages
- Age/SipSp/Parch: Were people of X age MLL if they were there with family?
- SipSp/Parch: If unable to derive with Name/SipSp/Parch MLL than people who were traveling alone?
- Cabin: Did deck and/or cabin number impact MLL? This seemed unlikely since people could have been anywhere on the ship so the hypothesis was discarded.
- Embarked/cabin: Did port of boarding impact cabin and, by extension, cabin analysis?
- Fare/cabin: Did cost of ticket impact cabin and, by extension, cabin analysis?
- Embarked/fare/cabin: Did a combination of where passenger boarded and fare paid impact cabin and, by extension, cabin analysis?

To keep things focused and keep to my deadline I kept with fairly straightforward options, avoiding any cross-categorical calculations.

The question I ended up with was, "to what extent did gender, age, number of siblings, spouses, parents, or children, or port of embarkment impact?

Since age was likely to be a significant indicator of survival I looked at the data and found that about 1/5 of the records lacked this data point. Luckily there were sources online for these passengers' ages, though they required manual imputation. There were still 26 passengers whose ages I could not get. I also decided that the data would be easier to manipulate as integers rather than floats so I converted all of the children under a year old to 1 thinking that in this particular situation there would be no need to differentiate.

My next step was to dummy out some of the data. I did this with gender, class, and port. I then binned the ages into 10 year groups and dummied them out, as well.

The logistic regression showed coefficients and correlations which confirmed that women and children went first (though if you were sailing 1st class that helped a lot, too).

A holdout-train test confirmed the model's results while cross-validation showed a mean score of 0.79. The classification report showed precision, recall, and f1 all to be a 0.84 and the confusion matrix was strong.

Given those numbers, an ROC-AUC of 0.85 wasn't surprising.

![](https://ajbentley.github.io/assets/images/titanic/Titanic_ROC.png?raw=true)

I proceeded to fine-tune the model with GridSearchCV and  GridSearch with KNN. These additional steps failed to improve upon the original model's performance.
