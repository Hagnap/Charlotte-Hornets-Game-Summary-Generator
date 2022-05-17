# CharlotteHornetsGameSummaryGenerator

## Brief Summary
This project uses a RNN+LSTM to generate summaries about Charlotte Hornets basketball games.
When building the model, I experimented with our Baseline Model’s structure and ended up utilizing a
“Stacked LSTM” within the model and it led to getting promising results. I wanted to generate
summaries because they are something that is typically written by humans, sports
journalism has been around for a long time but I thought it would be an interesting
thing to automate.

While working on this project I elected to
generate a summary for an Atlanta Hawks vs Charlotte Hornets game. I chose the
Hornets since they play in Charlotte and the Hawks were selected because they are in
the same division as the Hornets so they play three or four games a season. This allows
for a deeper corpus when compared to teams like the Phoenix Suns who are not
in their division.

## Data Used
To do this project I had used to the NBA_Data provided by Rotowire. The dataset
can be found in this repo. The dataset has information about every basketball from
the 2015-2017 seasons.

## The Approach Taken
### Software Used
To do generate a summary I had utilized the Tensorflow module to construct a 
Recurrent Neural Network with a Long Short Term Memory Component. To manipulate 
the data, Pandas was used. To conduct tokenization, lemmatization, getting the BLEU Score,
and utilizing smoothing methods; NLTK was used. To get the ROUGE Score, the PyPi module was
used.

### Techniques Used
To generate text for this task a RNN+LSTM model was used. Prior to generating the
task I first had to manipulate the data and prepare for it being inputted into the model.
To do this I created a corpus for each team and stored their respective summaries into
their corresponding corpus. After that I took two corpus, one for each team in a game
(two teams in a game at the same time) and created two mappings. One mapping for
indexes to characters and the other for characters to indexes. After that I took the
corpus containing a text for each of the team teams and mapped the letters in their
respective corpus to indices and characters respectively. This allowed the model to process the
text in a numerical form.
Once the text was processed, it was passed into our RNN+LSTM model. Within the
model I used the Adam optimizer algorithm and a Dropout layer. The Dropout layer was
placed in the middle of the model, I found that this yielded the best results when
experimenting with various placements of it. I also feature a Stacked LSTM
methodology within our model, this was very impactful to the B.L.E.U. score.

### About the Model
Here is an image showcasing the archicture of the model:


![RNN+LSTM Model's Architecture](https://github.com/jhagg26/Charlotte-Hornets-Game-Summary-Generator/blob/main/ModelArchitecture.PNG?raw=true)

Some of the hyperparameters for this model are:
  - Number of Epochs (2)
  - Text Diversity (0.25)
  - Step Size (2)
  - Window Size (350)
  - Range for the Starting Point ([0, ((length of the corpus) - (average length of a summary) - 1)])

The model uses the window size value decide how big the splits are when it has to split the data into partitions while the it is training.
The model samples an ‘index’ and uses that to generate a probability as its probability. The model uses the given probability to generate an index that it thinks fits the summary, the index is then converted to a character and added to the summary.


## Measuring Performance
For this model I used two metrics to evaluate the performance of the mode B.L.E.U.
and R.O.U.G.E.. I selected these two metrics because they are referred to as quality
metrics for Natural Language Processing tasks.

### Scores
**BLEU:** 0.38625/1.

**ROUGE:**  
  - Precision: 0.4820477/1
  - Recall: 0.106626/1
  - FMeasure: 0.1723767/1

According
to the Google Cloud Platform, the score of 0.38625 can mean that our model is
able to generate “Understandable to good translations”

### An Example a Generated Summary
*"cent from the free - throw line . The team 's best
scorer Kemba Walker had an off night , going just 3 -
for - 14 for nine points . Forward Marvin Williams was
one of the few bright spots , contributing a game -
high 16 points and pulling down nine rebounds . The
Hornets will host the sliding Phoeni"*

### Performance Over Time (ie Performance During Development)

| **Metrics**            | **Baseline Model** | **Final Model** |
| :----------------------| :------------------|:----------------|
| *B.L.E.U.*             | 7.9857408e-232     | 0.38625326      |
| *R.O.U.G.E. Precision* | 0.1251216569       | 0.48204773      |
| *R.O.U.G.E. Recall*    | 0.1068117968       | 0.1066263       |
| *R.O.U.G.E. FMeasure*  | 0.1131416947       | 0.1723767       |




Here is an image to display these results over project iterations:


![RNN+LSTM Model's Performance](https://github.com/jhagg26/Charlotte-Hornets-Game-Summary-Generator/blob/main/ModelPerformance.PNG?raw=true)

### Observations Made During Development
- The largest improvements were implementing multiple LSTM layers and adjusting value of the Dropout layer to 0.5
- Text diversity values greater than 0.35 generated gibberish and values smaller than 0.1 generated repetitive summaries.
- Having more than 2 epochs did barely improved performance in some cases and in other cases it caused our model to have a decreased performance. 
- Like the number of epochs, altering the value for the batch size had to little to no impact on the model’s performance. 
- Window sizes that were too small created the same exact summary multiple times with little to no variation
- When summaries generated had over three hundred characters it caused the model to generate more white space than text after generating about three hundred characters.
