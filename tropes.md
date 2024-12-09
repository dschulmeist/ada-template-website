# TV Tropes
To maximize your chances of creating a successful movie, it's worth looking at what kind of characters should appear in a movie. But how can one compactly describe a character? There is so many options! Thankfully, the dataset **TV tropes** provides just what we need. Now we should be ready to determine whether a movie with *absent minded professor* or an *arrogant kungfu guy* is more likely to succeed.

## What is success?
Before performing the analysis, we must establish what the ideal success metric is. You might want to aim for a movie which makes a lot of money or a movie which is rated well by the viewers, or perhaps a movie well rated by the critics? Ideally all at the same time, but we need to see if there is an overlap first. Therefore, our chosen success metrics are:

1. Box Office Revenue
2. IMDb Rating 
3. Metacritic Score

## Box Office Revenue Perspective
Since the TV tropes dataset contains a mapping from each trope cluster to a set of characters, we can look at the Box Office Revenue of the corresponding movie and identify the ranking of the tropes.

The below plot demonstrates the average Box Office Revenue of each trope
<div id="allTropesBoxOfficeRevenuePlot" style="width:100%; max-width:700px; height:500px;"></div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  // Data processed in Python (example data below)
  // var xData = ['Trope A', 'Trope B', 'Trope C']; // Tropes
  // var yData = [150, 350, 400]; // Box office revenue per trope
  var rawData = [{'trope': 'gadgeteer_genius', 'success_metric': 647179205.4444444}, {'trope': 'grumpy_old_man', 'success_metric': 539195924.6666666}, {'trope': 'charmer', 'success_metric': 530069451.90909094}, {'trope': 'child_prodigy', 'success_metric': 519998348.0}, {'trope': 'bruiser_with_a_soft_center', 'success_metric': 491920000.0}, {'trope': 'adventurer_archaeologist', 'success_metric': 468307217.4}, {'trope': 'broken_bird', 'success_metric': 419649591.0}, {'trope': 'egomaniac_hunter', 'success_metric': 414209401.12903225}, {'trope': 'doormat', 'success_metric': 373276774.4}, {'trope': 'trickster', 'success_metric': 362858355.6666667}, {'trope': 'pupil_turned_to_evil', 'success_metric': 347447431.0}, {'trope': 'jerk_jock', 'success_metric': 328653453.90625}, {'trope': 'byronic_hero', 'success_metric': 325700482.04761904}, {'trope': 'heartbroken_badass', 'success_metric': 323444962.4285714}, {'trope': 'crazy_survivalist', 'success_metric': 315947885.0}, {'trope': 'loveable_rogue', 'success_metric': 298233643.14285713}, {'trope': 'cultured_badass', 'success_metric': 293522889.3636364}, {'trope': 'master_swordsman', 'success_metric': 291984626.1764706}, {'trope': 'romantic_runnerup', 'success_metric': 282056295.75}, {'trope': 'eccentric_mentor', 'success_metric': 281563895.1666667}, {'trope': 'coward', 'success_metric': 251913361.42857143}, {'trope': 'bully', 'success_metric': 248325981.0}, {'trope': 'arrogant_kungfu_guy', 'success_metric': 239406978.45454547}, {'trope': 'warrior_poet', 'success_metric': 216645852.0}, {'trope': 'consummate_professional', 'success_metric': 209398228.5}, {'trope': 'junkie_prophet', 'success_metric': 203590570.5}, {'trope': 'gentleman_thief', 'success_metric': 202018815.5}, {'trope': 'casanova', 'success_metric': 191592846.625}, {'trope': 'corrupt_corporate_executive', 'success_metric': 180529639.0}, {'trope': 'father_to_his_men', 'success_metric': 179369424.3125}, {'trope': 'chanteuse', 'success_metric': 177277937.1}, {'trope': 'the_chief', 'success_metric': 170395945.0}, {'trope': 'playful_hacker', 'success_metric': 163146946.0}, {'trope': 'fastest_gun_in_the_west', 'success_metric': 160984130.4}, {'trope': 'dean_bitterman', 'success_metric': 157870668.75}, {'trope': 'henpecked_husband', 'success_metric': 151848917.4}, {'trope': 'evil_prince', 'success_metric': 144823493.0}, {'trope': 'hardboiled_detective', 'success_metric': 144395545.0}, {'trope': 'ditz', 'success_metric': 126584388.125}, {'trope': 'psycho_for_hire', 'success_metric': 111343329.875}, {'trope': 'retired_outlaw', 'success_metric': 111058424.83333333}, {'trope': 'officer_and_a_gentleman', 'success_metric': 110681820.5}, {'trope': 'hitman_with_a_heart', 'success_metric': 107333666.8888889}, {'trope': 'stupid_crooks', 'success_metric': 105042870.6}, {'trope': 'storyteller', 'success_metric': 103692910.75}, {'trope': 'morally_bankrupt_banker', 'success_metric': 98430091.5}, {'trope': 'crazy_jealous_guy', 'success_metric': 98010318.86956522}, {'trope': 'absent_minded_professor', 'success_metric': 92579135.8}, {'trope': 'tranquil_fury', 'success_metric': 91995016.36363636}, {'trope': 'dirty_cop', 'success_metric': 90979714.33333333}, {'trope': 'final_girl', 'success_metric': 86648773.70588236}, {'trope': 'drill_sargeant_nasty', 'success_metric': 82004147.6}, {'trope': 'klutz', 'success_metric': 81943981.0}, {'trope': 'young_gun', 'success_metric': 78026562.25}, {'trope': 'loser_protagonist', 'success_metric': 74395358.0}, {'trope': 'the_editor', 'success_metric': 70600000.0}, {'trope': 'slacker', 'success_metric': 65943944.72727273}, {'trope': 'surfer_dude', 'success_metric': 65428949.6}, {'trope': 'bounty_hunter', 'success_metric': 62675618.375}, {'trope': 'valley_girl', 'success_metric': 56005675.28571428}, {'trope': 'ophelia', 'success_metric': 53974270.0}, {'trope': 'stoner', 'success_metric': 51767929.461538464}, {'trope': 'granola_person', 'success_metric': 47637333.25}, {'trope': 'dumb_muscle', 'success_metric': 46205587.71428572}, {'trope': 'dumb_blonde', 'success_metric': 45809676.333333336}, {'trope': 'brainless_beauty', 'success_metric': 42365757.90909091}, {'trope': 'big_man_on_campus', 'success_metric': 39926288.166666664}, {'trope': 'revenge', 'success_metric': 37064736.875}, {'trope': 'prima_donna', 'success_metric': 36563647.125}, {'trope': 'bromantic_foil', 'success_metric': 17392920.5}, {'trope': 'self_made_man', 'success_metric': 10244990.4}, {'trope': 'classy_cat_burglar', 'success_metric': 4500000.0}]


  var top5xData = rawData.slice(0, 5).map(entry => entry.trope)
  var top5yData = rawData.slice(0, 5).map(entry => entry.success_metric)

  var bottom5xData = rawData.slice(-5).map(entry => entry.trope)
  var bottom5yData = rawData.slice(-5).map(entry => entry.success_metric)

  var xData = top5xData.concat(bottom5xData)
  var yData = top5yData.concat(bottom5yData)


  // Create the plot
  var data = [
    {
      x: xData,
      y: yData,
      type: 'bar',
      text: yData.map(value => (value / 1e6).toFixed(1) + 'M'), // Show values on bars
      textposition: 'auto',
      marker: {
        color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
        line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
      }
    }
  ];

  var layout = {
    title: 'Total Box Office Revenue per Trope (Top 5 vs Bottom 5)',
    xaxis: {
      title: 'Trope',
      tickangle: -45
    },
    yaxis: {
      title: 'Box Office Revenue',
      tickformat: '$.3s'
    },
    margin: {
      b: 150 // Increase bottom margin to give more space for x-axis labels
    },
    template: 'plotly_white'
  };

  // Render the plot
  Plotly.newPlot('allTropesBoxOfficeRevenuePlot', data, layout);
</script>


## IMDb Rating Perspective
Similarly to the Box Office Revenue, let us look at the top 5 and bottom 5 tropes based on the IMDb Rating!

<div id="allTropesIMDbRating" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var rawData = [{'trope': 'morally_bankrupt_banker', 'success_metric': 84.0}, {'trope': 'stupid_crooks', 'success_metric': 83.2}, {'trope': 'doormat', 'success_metric': 81.0}, {'trope': 'charmer', 'success_metric': 80.33333333333333}, {'trope': 'coward', 'success_metric': 80.0}, {'trope': 'dumb_muscle', 'success_metric': 78.85714285714286}, {'trope': 'consummate_professional', 'success_metric': 78.75}, {'trope': 'prima_donna', 'success_metric': 78.75}, {'trope': 'revenge', 'success_metric': 78.55555555555556}, {'trope': 'loveable_rogue', 'success_metric': 78.5}, {'trope': 'the_editor', 'success_metric': 78.25}, {'trope': 'hardboiled_detective', 'success_metric': 77.8125}, {'trope': 'eccentric_mentor', 'success_metric': 77.5}, {'trope': 'bruiser_with_a_soft_center', 'success_metric': 77.4}, {'trope': 'fastest_gun_in_the_west', 'success_metric': 77.4}, {'trope': 'trickster', 'success_metric': 77.4}, {'trope': 'warrior_poet', 'success_metric': 77.35714285714286}, {'trope': 'psycho_for_hire', 'success_metric': 77.11764705882354}, {'trope': 'bully', 'success_metric': 77.0}, {'trope': 'tranquil_fury', 'success_metric': 76.27272727272727}, {'trope': 'chanteuse', 'success_metric': 76.0909090909091}, {'trope': 'grumpy_old_man', 'success_metric': 76.0}, {'trope': 'master_swordsman', 'success_metric': 75.94117647058823}, {'trope': 'retired_outlaw', 'success_metric': 75.83333333333333}, {'trope': 'byronic_hero', 'success_metric': 75.63636363636364}, {'trope': 'final_girl', 'success_metric': 75.58823529411765}, {'trope': 'officer_and_a_gentleman', 'success_metric': 75.4}, {'trope': 'cultured_badass', 'success_metric': 75.16666666666667}, {'trope': 'bromantic_foil', 'success_metric': 75.0}, {'trope': 'father_to_his_men', 'success_metric': 74.83333333333333}, {'trope': 'young_gun', 'success_metric': 74.63636363636364}, {'trope': 'arrogant_kungfu_guy', 'success_metric': 74.25}, {'trope': 'classy_cat_burglar', 'success_metric': 74.0}, {'trope': 'crazy_jealous_guy', 'success_metric': 73.89285714285714}, {'trope': 'ophelia', 'success_metric': 73.8}, {'trope': 'self_made_man', 'success_metric': 73.6}, {'trope': 'evil_prince', 'success_metric': 73.53846153846153}, {'trope': 'dean_bitterman', 'success_metric': 73.5}, {'trope': 'storyteller', 'success_metric': 73.4}, {'trope': 'dirty_cop', 'success_metric': 73.33333333333333}, {'trope': 'gadgeteer_genius', 'success_metric': 73.33333333333333}, {'trope': 'gentleman_thief', 'success_metric': 72.875}, {'trope': 'heartbroken_badass', 'success_metric': 72.85714285714286}, {'trope': 'adventurer_archaeologist', 'success_metric': 72.6}, {'trope': 'playful_hacker', 'success_metric': 71.83333333333333}, {'trope': 'drill_sargeant_nasty', 'success_metric': 71.8}, {'trope': 'romantic_runnerup', 'success_metric': 71.71428571428571}, {'trope': 'bounty_hunter', 'success_metric': 70.9}, {'trope': 'hitman_with_a_heart', 'success_metric': 70.7}, {'trope': 'jerk_jock', 'success_metric': 70.63636363636364}, {'trope': 'corrupt_corporate_executive', 'success_metric': 70.44117647058823}, {'trope': 'the_chief', 'success_metric': 69.33333333333333}, {'trope': 'slacker', 'success_metric': 69.16666666666667}, {'trope': 'egomaniac_hunter', 'success_metric': 68.66666666666667}, {'trope': 'klutz', 'success_metric': 68.66666666666667}, {'trope': 'junkie_prophet', 'success_metric': 68.5}, {'trope': 'granola_person', 'success_metric': 68.0}, {'trope': 'casanova', 'success_metric': 67.875}, {'trope': 'stoner', 'success_metric': 67.71428571428571}, {'trope': 'crazy_survivalist', 'success_metric': 67.66666666666667}, {'trope': 'dumb_blonde', 'success_metric': 67.42857142857143}, {'trope': 'surfer_dude', 'success_metric': 67.2}, {'trope': 'child_prodigy', 'success_metric': 67.0}, {'trope': 'broken_bird', 'success_metric': 65.5}, {'trope': 'loser_protagonist', 'success_metric': 65.33333333333333}, {'trope': 'pupil_turned_to_evil', 'success_metric': 63.5}, {'trope': 'big_man_on_campus', 'success_metric': 63.333333333333336}, {'trope': 'henpecked_husband', 'success_metric': 63.2}, {'trope': 'valley_girl', 'success_metric': 62.857142857142854}, {'trope': 'ditz', 'success_metric': 62.375}, {'trope': 'absent_minded_professor', 'success_metric': 61.8}, {'trope': 'brainless_beauty', 'success_metric': 58.583333333333336}]


  var top5xData = rawData.slice(0, 5).map(entry => entry.trope)
  var top5yData = rawData.slice(0, 5).map(entry => entry.success_metric)

  var bottom5xData = rawData.slice(-5).map(entry => entry.trope)
  var bottom5yData = rawData.slice(-5).map(entry => entry.success_metric)

  var xData = top5xData.concat(bottom5xData)
  var yData = top5yData.concat(bottom5yData)


  // Create the plot
  var data = [
    {
      x: xData,
      y: yData.map(entry => entry / 10),
      type: 'bar',
      text: yData.map(value => (value / 10).toFixed(1)), // Show values on bars
      textposition: 'auto',
      marker: {
        color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
        line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
      }
    }
  ];

  var layout = {
    title: 'IMDb Rating (Top 5 vs Bottom 5)',
    xaxis: {
      title: 'Trope',
      tickangle: -45
    },
    yaxis: {
      title: 'IMDb Rating',
    },
    margin: {
      b: 150 // Increase bottom margin to give more space for x-axis labels
    },
    template: 'plotly_white'
  };

  // Render the plot
  Plotly.newPlot('allTropesIMDbRating', data, layout);
</script>

## Metacritic Score Perspective
Finally, let us look at the top 5 and bottom 5 tropes based on the Metacritic Score!

<div id="allTropesMetacriticRating" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var rawData = [{'trope': 'stupid_crooks', 'success_metric': 88.75}, {'trope': 'prima_donna', 'success_metric': 87.0}, {'trope': 'doormat', 'success_metric': 84.875}, {'trope': 'the_editor', 'success_metric': 83.0}, {'trope': 'morally_bankrupt_banker', 'success_metric': 82.83333333333333}, {'trope': 'hardboiled_detective', 'success_metric': 81.73333333333333}, {'trope': 'chanteuse', 'success_metric': 81.72727272727273}, {'trope': 'grumpy_old_man', 'success_metric': 79.1875}, {'trope': 'psycho_for_hire', 'success_metric': 79.08333333333333}, {'trope': 'bully', 'success_metric': 77.8}, {'trope': 'fastest_gun_in_the_west', 'success_metric': 77.75}, {'trope': 'dumb_muscle', 'success_metric': 77.64285714285714}, {'trope': 'retired_outlaw', 'success_metric': 77.5}, {'trope': 'loveable_rogue', 'success_metric': 77.44444444444444}, {'trope': 'revenge', 'success_metric': 77.0625}, {'trope': 'warrior_poet', 'success_metric': 76.58333333333333}, {'trope': 'dumb_blonde', 'success_metric': 76.18181818181819}, {'trope': 'classy_cat_burglar', 'success_metric': 76.0}, {'trope': 'coward', 'success_metric': 75.45}, {'trope': 'klutz', 'success_metric': 74.66666666666667}, {'trope': 'father_to_his_men', 'success_metric': 74.26666666666667}, {'trope': 'gadgeteer_genius', 'success_metric': 74.125}, {'trope': 'dirty_cop', 'success_metric': 73.83333333333333}, {'trope': 'consummate_professional', 'success_metric': 73.75}, {'trope': 'dean_bitterman', 'success_metric': 73.75}, {'trope': 'bromantic_foil', 'success_metric': 73.7}, {'trope': 'tranquil_fury', 'success_metric': 73.5}, {'trope': 'slacker', 'success_metric': 73.36363636363636}, {'trope': 'evil_prince', 'success_metric': 73.31818181818181}, {'trope': 'final_girl', 'success_metric': 73.05882352941177}, {'trope': 'child_prodigy', 'success_metric': 73.0}, {'trope': 'master_swordsman', 'success_metric': 72.38461538461539}, {'trope': 'charmer', 'success_metric': 72.33333333333333}, {'trope': 'byronic_hero', 'success_metric': 71.55}, {'trope': 'egomaniac_hunter', 'success_metric': 71.38333333333334}, {'trope': 'officer_and_a_gentleman', 'success_metric': 71.3}, {'trope': 'arrogant_kungfu_guy', 'success_metric': 71.2}, {'trope': 'eccentric_mentor', 'success_metric': 71.2}, {'trope': 'ophelia', 'success_metric': 71.2}, {'trope': 'bounty_hunter', 'success_metric': 70.61111111111111}, {'trope': 'drill_sargeant_nasty', 'success_metric': 70.5}, {'trope': 'crazy_jealous_guy', 'success_metric': 70.3}, {'trope': 'young_gun', 'success_metric': 70.0}, {'trope': 'the_chief', 'success_metric': 69.66666666666667}, {'trope': 'hitman_with_a_heart', 'success_metric': 69.38888888888889}, {'trope': 'heartbroken_badass', 'success_metric': 68.83333333333333}, {'trope': 'corrupt_corporate_executive', 'success_metric': 68.0625}, {'trope': 'storyteller', 'success_metric': 67.9}, {'trope': 'trickster', 'success_metric': 67.83333333333333}, {'trope': 'jerk_jock', 'success_metric': 67.734375}, {'trope': 'big_man_on_campus', 'success_metric': 67.0}, {'trope': 'crazy_survivalist', 'success_metric': 67.0}, {'trope': 'self_made_man', 'success_metric': 67.0}, {'trope': 'romantic_runnerup', 'success_metric': 66.875}, {'trope': 'surfer_dude', 'success_metric': 66.7}, {'trope': 'gentleman_thief', 'success_metric': 66.6875}, {'trope': 'granola_person', 'success_metric': 66.4375}, {'trope': 'stoner', 'success_metric': 66.17857142857143}, {'trope': 'cultured_badass', 'success_metric': 66.16666666666667}, {'trope': 'valley_girl', 'success_metric': 64.71428571428571}, {'trope': 'broken_bird', 'success_metric': 64.33333333333333}, {'trope': 'casanova', 'success_metric': 63.785714285714285}, {'trope': 'junkie_prophet', 'success_metric': 63.75}, {'trope': 'loser_protagonist', 'success_metric': 63.6}, {'trope': 'playful_hacker', 'success_metric': 63.375}, {'trope': 'brainless_beauty', 'success_metric': 61.72727272727273}, {'trope': 'pupil_turned_to_evil', 'success_metric': 61.5}, {'trope': 'bruiser_with_a_soft_center', 'success_metric': 61.0}, {'trope': 'ditz', 'success_metric': 59.166666666666664}, {'trope': 'adventurer_archaeologist', 'success_metric': 58.625}, {'trope': 'henpecked_husband', 'success_metric': 56.875}, {'trope': 'absent_minded_professor', 'success_metric': 52.0}]



  var top5xData = rawData.slice(0, 5).map(entry => entry.trope)
  var top5yData = rawData.slice(0, 5).map(entry => entry.success_metric)

  var bottom5xData = rawData.slice(-5).map(entry => entry.trope)
  var bottom5yData = rawData.slice(-5).map(entry => entry.success_metric)

  var xData = top5xData.concat(bottom5xData)
  var yData = top5yData.concat(bottom5yData)


  // Create the plot
  var data = [
    {
      x: xData,
      y: yData.map(entry => (entry / 10).toFixed(1)),
      type: 'bar',
      text: yData.map(value => (value / 10).toFixed(1)), // Show values on bars
      textposition: 'auto',
      marker: {
        color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
        line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
      }
    }
  ];

  var layout = {
    title: 'Metacritic Score (Top 5 vs Bottom 5)',
    xaxis: {
      title: 'Trope',
      tickangle: -45
    },
    yaxis: {
      title: 'Metacritic Score',
    },
    margin: {
      b: 150 // Increase bottom margin to give more space for x-axis labels
    },
    template: 'plotly_white'
  };

  // Render the plot
  Plotly.newPlot('allTropesMetacriticRating', data, layout);
</script>


## Remarks about the data used
Before we conclude the section, it is worth noting that the tropes we selected are not the best representation of the ground truth. They were only selected from around 500 successful movies and the corresponding characters were disproportionally represented by mostly men. The distribution can be clearly visible in the pie chart.

<div id="genderDistributionPie" style="width:100%; max-width:500px; height:500px;"></div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  // Data for pie chart (processed from Python)
  var pieLabels = ["Male", "Female"]; // Example labels
  var pieValues = [599, 102]; // Example counts

  var pieData = [
    {
      labels: pieLabels,
      values: pieValues,
      type: 'pie',
      textinfo: 'label+percent',
      hoverinfo: 'label+percent',
      marker: {
        colors: ['#EE', '#EF553B'] // Example colors
      }
    }
  ];

  var pieLayout = {
    title: 'Gender Distribution in the Dataset',
    template: 'plotly_white'
  };

  Plotly.newPlot('genderDistributionPie', pieData, pieLayout);
</script>

Now let us dive into analysing which TV trope maximizes a movie success. 

## My Plot Section

Below is an interactive plot rendered using JavaScript:

<div id="myPlot" style="width:100%; max-width:700px; height:500px;"></div>

<script>
  var data = [{ x: [1, 2, 3], y: [10, 15, 13], type: 'scatter' }];
  Plotly.newPlot('myPlot', data);
</script>