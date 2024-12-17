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

Oscar Score= [# of LAO]x5 + [# of SAO]x3 +  [# of LAN]x2+ [# of SAN]x1

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

<canvas id="imdbOscarScatterPlot" width="600" height="400"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-trendline"></script> <!-- Add regression line plugin -->

<script>
// IMDb Scores and Oscar Scores (Example Data)
const data = [
    { actor: 'Meryl Streep', imdb: 8.0, oscars: 46 },
    { actor: 'Katharine Hepburn', imdb: 7.8, oscars: 36 },
    { actor: 'Bette Davis', imdb: 7.5, oscars: 28 },
    { actor: 'Jack Nicholson', imdb: 7.7, oscars: 28 },
    { actor: 'Spencer Tracy', imdb: 7.6, oscars: 24 },
    { actor: 'Denzel Washington', imdb: 7.9, oscars: 21 },
    { actor: 'Ingrid Bergman', imdb: 7.4, oscars: 21 },
    { actor: 'Marlon Brando', imdb: 8.1, oscars: 21 },
    { actor: 'Jack Lemmon', imdb: 7.5, oscars: 20 },
    { actor: 'Paul Newman', imdb: 7.3, oscars: 20 }
];

// Format data for Chart.js scatter plot
const scatterData = data.map(item => ({
    x: item.oscars,  // Oscar Score (x-axis)
    y: item.imdb,    // IMDb Score (y-axis)
    label: item.actor
}));

const ctx = document.getElementById('imdbOscarScatterPlot').getContext('2d');
const scatterPlot = new Chart(ctx, {
    type: 'scatter',
    data: {
        datasets: [{
            label: 'IMDb Score vs Oscar Score',
            data: scatterData,
            backgroundColor: 'rgba(75, 192, 192, 0.7)',
            borderColor: 'rgba(75, 192, 192, 1)',
            pointRadius: 6,
            pointHoverRadius: 8,
            trendlineLinear: {  // Add regression line
                style: "rgba(255, 99, 132, 0.6)",
                lineStyle: "dotted",
                width: 2
            }
        }]
    },
    options: {
        responsive: true,
        plugins: {
            tooltip: {
                callbacks: {
                    label: function(context) {
                        const dataPoint = context.raw;
                        return `${dataPoint.label}: Oscar Score = ${dataPoint.x}, IMDb Score = ${dataPoint.y}`;
                    }
                }
            },
            title: {
                display: true,
                text: 'Scatter Plot: IMDb Score vs Oscar Score'
            }
        },
        scales: {
            x: {
                title: { display: true, text: 'Oscar Score' },
                beginAtZero: false
            },
            y: {
                title: { display: true, text: 'IMDb Score' },
                beginAtZero: false,
                suggestedMin: 7.0,
                suggestedMax: 8.5
            }
        }
    }
});
</script>


The plot of IMDb scores versus Oscar scores shows a slight correlation between the two variables, but the relationship is not strong as we can see by the regression line. While some actors with higher Oscar scores also tend to have higher IMDb scores, this pattern is not consistent across all data points. One reason for this is that the Oscars and IMDb ratings measure different aspects of an actor's career. 

Oscar scores are based on professional recognition for outstanding performances, typically within the context of prestigious award ceremonies, whereas IMDb scores reflect the general public's opinion of movies and performances. These two metrics are influenced by different factors—Oscar wins or nominations are often determined by industry professionals, while IMDb ratings are driven by broader audience sentiments. 

As a result, while some actors may appear in both highly rated Oscar-winning films and movies with high IMDb scores, the correlation between the two is weak. This is because each score is influenced by different factors and biases—Oscars are based on the opinions of industry professionals, while IMDb ratings reflect general public sentiment.

It is thus  useful to conduct further experiments and explore additional factors that could impact a movie's success. We should also consider incorporating other sources of data, such as Metacritic, which includes professional critics' ratings, and Rotten Tomatoes, to provide a more comprehensive view of a movie's reception across different audiences.