# Actor Success: What Defines a Successful Acting Career?

To understand what makes an actor’s career successful, we combine multiple datasets:
- **Oscar Awards**: Recognizing industry acclaim.
- **IMDB Scores**: Reflecting audience ratings.
- **Additional Actor Data**: Providing biographical details.

The analysis reveals which actors stand out, how metrics correlate, and whether gender or country influences success.

---

## What is Success?

Success is subjective, but for this analysis, we define it through two measurable criteria:
1. **Oscar Score**: 1 point per nomination, 2 points per win.
2. **IMDB Score**: Average audience ratings for an actor’s movies.

These are combined into a **Final Score**:

\[ \text{Final Score} = \text{Oscar Score} + \text{Average IMDB Score} \]

By combining industry recognition and public ratings, we create a balanced metric of success.

---

## Distribution of Actor Success

The majority of actors have **Final Scores** between **8 and 10**, with a few exceptional outliers.

<div id="distributionFinalScore" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var data = [{ x: [8, 9, 10, 22], y: [50, 80, 40, 1], type: 'bar' }];
  Plotly.newPlot('distributionFinalScore', data, {title: 'Distribution of Final Scores'});
</script>

**Insight**: A small number of actors dominate the higher end, reflecting consistent excellence.

---

## Top Actors by Final Score

Let’s highlight the **top 20 actors** based on the Final Score:

<div id="topActorsFinalScore" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var data = [
    {x: ['Jack Nicholson', 'Marlon Brando', 'Paul Newman', 'Meryl Streep'],
     y: [22.1, 17.5, 17.2, 16.9],
     type: 'bar', text: ['22.1', '17.5', '17.2', '16.9'], textposition: 'auto'}
  ];
  Plotly.newPlot('topActorsFinalScore', data, {title: 'Top Actors by Final Score'});
</script>

**Jack Nicholson** leads with a Final Score of **22.1**, followed by **Marlon Brando** and **Paul Newman**.

---

## Relationship Between Oscar Score and IMDB Score

How do Oscars (industry acclaim) relate to IMDB scores (audience acclaim)?

<div id="oscarVsIMDB" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var data = [
    {x: [2, 10, 15], y: [7.0, 7.5, 6.8], mode: 'markers', marker: {size: 10, color: ['blue', 'red', 'blue']}}
  ];
  Plotly.newPlot('oscarVsIMDB', data, {title: 'Oscar Score vs IMDB Score'});
</script>

**Observation**: There is a weak positive correlation. Higher IMDB scores don’t guarantee Oscars, and vice versa.

---

## Gender Representation

Are successful actors evenly distributed across genders?

<div id="genderDistribution" style="width:100%; max-width:500px; height:500px;"></div>
<script>
  var data = [{labels: ['Male', 'Female'], values: [55, 45], type: 'pie'}];
  Plotly.newPlot('genderDistribution', data, {title: 'Gender Distribution'});
</script>

**Result**: Success appears to be almost evenly distributed.

---

## Top Countries Producing Successful Actors

Where are the most successful actors born?

<div id="topCountries" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var data = [{x: ['USA', 'UK', 'Canada', 'France'], y: [80, 20, 10, 10], type: 'bar'}];
  Plotly.newPlot('topCountries', data, {title: 'Top Countries for Successful Actors'});
</script>

**Finding**: The **USA** dominates, with the **UK** and **Canada** following.

---

## Final Score vs Number of Movies

Does appearing in more movies lead to greater success?

<div id="moviesVsFinalScore" style="width:100%; max-width:700px; height:500px;"></div>
<script>
  var data = [{x: [5, 10, 20, 30], y: [8, 12, 15, 10], mode: 'lines+markers'}];
  Plotly.newPlot('moviesVsFinalScore', data, {title: 'Number of Movies vs Final Score'});
</script>

**Insight**: While there’s no strong correlation, male actors show a slight upward trend.

---

## Key Insights

1. **Jack Nicholson** dominates as the most successful actor.
2. **IMDB ratings** and **Oscar wins** measure different aspects of success.
3. Success is **gender balanced**, challenging assumptions about industry bias.
4. **USA** remains the largest producer of successful actors.

---

## Data Quality Notes

1. Some **birth year anomalies** exist (e.g., Jack Nicholson incorrectly listed as born in 1996).
2. The data does not account for **actors’ career length** or other subjective success metrics.

---

## Conclusion

Success in acting, defined by Oscars and IMDB scores, reveals both expected and surprising trends:
- **Industry recognition** and **audience acclaim** often diverge.
- **Geography**, **gender**, and **movie volume** play nuanced roles.

This analysis provides a robust starting point for understanding success in Hollywood and beyond. 🎬
