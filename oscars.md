# Oscars

When people think about a movie's success, the first thing that often comes to mind is the actors. A well-known actor can elevate a movie, making it more popular, drawing larger audiences, and ultimately contributing to its commercial success. Conversely, an unknown actor may not have the same impact. In this section, we aim to explore whether there is a relationship between an actor's success and the average movie ratings they appear in.

## What does it mean for an actor to be successful?
Before diving into the analysis, we must first define what constitutes a "successful actor." For many in the industry, winning an Oscar is the pinnacle of achievement. While an actor's success can be measured in many ways, for the purposes of this milestone, we will define a successful actor as one who has won the highest number of Oscars.

However, Best Actor and Best Actress aren’t the only possible Oscar wins; actors can also receive recognition in Supporting Actor and Supporting Actress categories. Additionally, Oscar nominations play a significant role, as even a nomination can enhance an actor’s reputation and attract audiences.

However, lead roles generally have a larger impact on a movie’s success, as these actors often drive the story and have more screen time.  For this reason, we are going to build an Oscar score that applies weights to different award categories, giving greater significance to lead roles and including nominations to reflect their influence. We have decided to apply the following weights:

- Lead Actor Oscar Win (LAO) has weight 4. 

- Supporting Actor Oscar Win (SAO) has weight 3.

- Lead Actor Oscar Nomination (LAN) has weight 2. 

- Supporting Actor Oscar (SAN) Nomination has weight 1.

Oscar Score= [# of LAO]*5 + [# of SAO]*3 +  [# of LAN]*2+ [# of SAN]×1

By applying this score in our data, we get the following top 10 most successful actors:

| Actor Name         | Oscar Score |
|--------------------|-------------|
| Meryl Streep       | 46          |
| Katharine Hepburn  | 36          |
| Bette Davis        | 28          |
| Jack Nicholson     | 28          |
| Spencer Tracy      | 24          |
| Denzel Washington  | 21          |
| Ingrid Bergman     | 21          |
| Marlon Brando      | 21          |
| Jack Lemmon        | 20          |
| Paul Newman        | 20          |


# IMDb Score vs Oscar Score

This scatter plot shows the relationship between IMDb scores and total Oscar scores of various movies.

<div id="scatter-plot"></div>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  // Fetching data from the oscars_section.json file
  fetch('json_data/oscars_section.json')
    .then(response => response.text())  // Get raw text response
    .then(text => {
      let validData = [];

      // Split the JSON text into individual lines (if it's a line-by-line JSON file)
      const lines = text.split('\n');

      // Loop through each line and try to parse it as JSON
      lines.forEach(line => {
        try {
          // Parse the line as JSON and push to validData array
          const item = JSON.parse(line);
          
          // Ensure it contains valid fields before adding it to the plot data arrays
          if (item.imdb_score && item.total_oscar_score && item.actor_name) {
            validData.push(item);
          }
        } catch (e) {
          // Skip lines with errors
          console.warn('Skipping invalid JSON line:', e);
        }
      });

      // Initialize arrays for imdb_scores, oscar_scores, and actor names
      let imdb_scores = [];
      let oscar_scores = [];
      let actor_names = [];

      // Extract imdb_scores, oscar_scores, and actor names from the valid data
      validData.forEach(item => {
        imdb_scores.push(item.imdb_score);
        oscar_scores.push(item.total_oscar_score);
        actor_names.push(item.actor_name); // Add actor name
      });

      // Creating the scatter plot with Plotly
      var trace = {
        x: imdb_scores,
        y: oscar_scores,
        mode: 'markers',
        type: 'scatter',
        marker: { color: 'blue', symbol: 'circle' },
        text: actor_names.map((name, index) => `${name}<br>Oscar Score: ${oscar_scores[index]}`), // Tooltip text with actor name and Oscar score
        hoverinfo: 'text' // Show the 'text' information on hover
      };

      var layout = {
        title: 'IMDb Score vs Oscar Score',
        xaxis: { title: 'Average IMDb Score' },
        yaxis: { title: 'Total Oscar Score', rangemode: 'tozero' },
        hovermode: 'closest', // Ensure tooltip follows the closest point
        hoverdistance: 100, // Optional: adjust the hover distance
        showlegend: false,
        hoverlabel: {
          bgcolor: 'white', // Tooltip background color
          bordercolor: 'black', // Tooltip border color
          font: { size: 14, color: 'black' } // Adjust font size and color
        }
      };

      // Plotting the graph in the div with id 'scatter-plot'
      Plotly.newPlot('scatter-plot', [trace], layout);
    })
    .catch(error => {
      console.error('Error loading the JSON file:', error);
    });
</script>


The plot of IMDb scores versus Oscar scores shows a slight correlation between the two variables, but the relationship is not strong as we can see by the regression line. While some actors with higher Oscar scores also tend to have higher IMDb scores, this pattern is not consistent across all data points. One reason for this is that the Oscars and IMDb ratings measure different aspects of an actor's career. 

Oscar scores are based on professional recognition for outstanding performances, typically within the context of prestigious award ceremonies, whereas IMDb scores reflect the general public's opinion of movies and performances. These two metrics are influenced by different factors—Oscar wins or nominations are often determined by industry professionals, while IMDb ratings are driven by broader audience sentiments. 

As a result, while some actors may appear in both highly rated Oscar-winning films and movies with high IMDb scores, the correlation between the two is weak. This is because each score is influenced by different factors and biases—Oscars are based on the opinions of industry professionals, while IMDb ratings reflect general public sentiment.

It is thus  useful to conduct further experiments and explore additional factors that could impact a movie's success. We should also consider incorporating other sources of data, such as Metacritic, which includes professional critics' ratings, and Rotten Tomatoes, to provide a more comprehensive view of a movie's reception across different audiences.