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
    {"actor_name":"Joan Lorring","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"John Hawkes","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Grayson Hall","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Rock Hudson","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Luise Rainer","total_oscar_score":10,"imdb_score":null,"num_movies":0}
{"actor_name":"Eve Arden","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Virginia Madsen","total_oscar_score":1,"imdb_score":64.0,"num_movies":2}
{"actor_name":"Talia Shire","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Maria Ouspenskaya","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Richard Barthelmess","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Felicity Huffman","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Marie-Christine Barrault","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Danny Aiello","total_oscar_score":1,"imdb_score":78.0,"num_movies":1}
{"actor_name":"Gig Young","total_oscar_score":5,"imdb_score":null,"num_movies":0}
{"actor_name":"Hugh Griffith","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"John Gielgud","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"Marsha Mason","total_oscar_score":8,"imdb_score":null,"num_movies":0}
{"actor_name":"Richard Farnsworth","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"William Hurt","total_oscar_score":10,"imdb_score":69.0,"num_movies":4}
{"actor_name":"David Niven","total_oscar_score":5,"imdb_score":56.8333333333,"num_movies":6}
{"actor_name":"Burl Ives","total_oscar_score":3,"imdb_score":74.0,"num_movies":1}
{"actor_name":"Laura Dern","total_oscar_score":6,"imdb_score":71.0,"num_movies":1}
{"actor_name":"Leonard Frey","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"William Holden","total_oscar_score":9,"imdb_score":75.75,"num_movies":4}
{"actor_name":"Elisabeth Bergner","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Janet Leigh","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Nick Nolte","total_oscar_score":5,"imdb_score":63.0,"num_movies":4}
{"actor_name":"Diane Lane","total_oscar_score":2,"imdb_score":66.8,"num_movies":5}
{"actor_name":"Jane Darwell","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Noriyuki 'Pat' Morita","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Viggo Mortensen","total_oscar_score":6,"imdb_score":73.5,"num_movies":8}
{"actor_name":"Mary McDonnell","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Louis Gossett, Jr.","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Gary Cooper","total_oscar_score":16,"imdb_score":77.0,"num_movies":1}
{"actor_name":"Olivia de Havilland","total_oscar_score":15,"imdb_score":null,"num_movies":0}
{"actor_name":"Diana Ross","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"John Travolta","total_oscar_score":4,"imdb_score":62.75,"num_movies":20}
{"actor_name":"Mare Winningham","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Ernest Borgnine","total_oscar_score":5,"imdb_score":null,"num_movies":0}
{"actor_name":"Ida Kaminska","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Brian Aherne","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Dan O'Herlihy","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Lila Kedrova","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Maggie McNamara","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Valentina Cortese","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Leslie Browne","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"George Segal","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Diane Keaton","total_oscar_score":11,"imdb_score":59.6,"num_movies":5}
{"actor_name":"Lucile Watson","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Brendan Fraser","total_oscar_score":5,"imdb_score":61.1875,"num_movies":16}
{"actor_name":"Da'Vine Joy Randolph","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Sam Rockwell","total_oscar_score":4,"imdb_score":62.8333333333,"num_movies":6}
{"actor_name":"Dorothy Dandridge","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Kristin Scott Thomas","total_oscar_score":2,"imdb_score":73.0,"num_movies":1}
{"actor_name":"Hilary Swank","total_oscar_score":10,"imdb_score":67.875,"num_movies":8}
{"actor_name":"Kevin Spacey","total_oscar_score":8,"imdb_score":70.4,"num_movies":5}
{"actor_name":"Maggie Smith","total_oscar_score":13,"imdb_score":null,"num_movies":0}
{"actor_name":"Ralph Bellamy","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Red Buttons","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Gena Rowlands","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"Florence Pugh","total_oscar_score":1,"imdb_score":68.6,"num_movies":5}
{"actor_name":"Vivien Leigh","total_oscar_score":10,"imdb_score":78.5,"num_movies":2}
{"actor_name":"Lilia Skala","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"George C. Scott","total_oscar_score":9,"imdb_score":68.5,"num_movies":4}
{"actor_name":"Catherine Zeta-Jones","total_oscar_score":3,"imdb_score":64.0,"num_movies":3}
{"actor_name":"Angelina Jolie","total_oscar_score":5,"imdb_score":66.3,"num_movies":10}
{"actor_name":"Ray Milland","total_oscar_score":5,"imdb_score":78.5,"num_movies":2}
{"actor_name":"Elsa Lanchester","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Robert Shaw","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Tommy Lee Jones","total_oscar_score":7,"imdb_score":63.875,"num_movies":8}
{"actor_name":"Omar Sharif","total_oscar_score":1,"imdb_score":76.0,"num_movies":1}
{"actor_name":"Frank Finlay","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Juliette Binoche","total_oscar_score":5,"imdb_score":65.75,"num_movies":4}
{"actor_name":"Michelle Pfeiffer","total_oscar_score":5,"imdb_score":63.3333333333,"num_movies":6}
{"actor_name":"Peter Ustinov","total_oscar_score":7,"imdb_score":66.0,"num_movies":2}
{"actor_name":"Shohreh Aghdashloo","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Joanne Woodward","total_oscar_score":11,"imdb_score":null,"num_movies":0}
{"actor_name":"Chief Dan George","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Robert Forster","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Hermione Baddeley","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Valerie Perrine","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Marlee Matlin","total_oscar_score":5,"imdb_score":null,"num_movies":0}
{"actor_name":"Sandy Dennis","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Dan Aykroyd","total_oscar_score":1,"imdb_score":61.5,"num_movies":8}
{"actor_name":"Peggy Cass","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Laura Linney","total_oscar_score":5,"imdb_score":60.0,"num_movies":2}
{"actor_name":"Sam Shepard","total_oscar_score":1,"imdb_score":74.0,"num_movies":1}
{"actor_name":"Victor Buono","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Margaret Avery","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Lena Olin","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Claudette Colbert","total_oscar_score":9,"imdb_score":null,"num_movies":0}
{"actor_name":"Leslie Caron","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"Marina de Tavira","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Christopher Walken","total_oscar_score":4,"imdb_score":64.8,"num_movies":5}
{"actor_name":"William Bendix","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Dean Stockwell","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Basil Rathbone","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Gloria Swanson","total_oscar_score":6,"imdb_score":null,"num_movies":0}
{"actor_name":"Juanita Moore","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Lewis Stone","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Claude Rains","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"John Malkovich","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Burgess Meredith","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Ana de Armas","total_oscar_score":2,"imdb_score":31.0,"num_movies":2}
{"actor_name":"Cliff Robertson","total_oscar_score":5,"imdb_score":null,"num_movies":0}
{"actor_name":"William Demarest","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Emily Blunt","total_oscar_score":1,"imdb_score":68.0,"num_movies":5}
{"actor_name":"Candy Clark","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Lorraine Bracco","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Mildred Dunnock","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Gene Hackman","total_oscar_score":12,"imdb_score":71.5714285714,"num_movies":7}
{"actor_name":"James Franco","total_oscar_score":2,"imdb_score":60.7,"num_movies":10}
{"actor_name":"May Robson","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Lee Tracy","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Michael Keaton","total_oscar_score":2,"imdb_score":67.0,"num_movies":6}
{"actor_name":"Emma Thompson","total_oscar_score":10,"imdb_score":69.6666666667,"num_movies":6}
{"actor_name":"Claire Trevor","total_oscar_score":5,"imdb_score":null,"num_movies":0}
{"actor_name":"Gregory Peck","total_oscar_score":13,"imdb_score":73.5,"num_movies":8}
{"actor_name":"Sondra Locke","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Marlene Dietrich","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Shirley Knight","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Jackie Cooper","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Amy Madigan","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Clifton Webb","total_oscar_score":4,"imdb_score":66.0,"num_movies":1}
{"actor_name":"Angela Lansbury","total_oscar_score":3,"imdb_score":70.0,"num_movies":1}
{"actor_name":"Djimon Hounsou","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Shirley Jones","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Lee J. Cobb","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Barbara Hershey","total_oscar_score":1,"imdb_score":66.0,"num_movies":1}
{"actor_name":"Vanessa Kirby","total_oscar_score":2,"imdb_score":63.0,"num_movies":2}
{"actor_name":"John Wayne","total_oscar_score":7,"imdb_score":72.5833333333,"num_movies":12}
{"actor_name":"Michael Dunn","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Dame Edith Evans","total_oscar_score":4,"imdb_score":null,"num_movies":0}
{"actor_name":"Ned Beatty","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Lesley Manville","total_oscar_score":1,"imdb_score":73.0,"num_movies":1}
{"actor_name":"Melissa McCarthy","total_oscar_score":3,"imdb_score":61.0,"num_movies":8}
{"actor_name":"Jane Wyman","total_oscar_score":11,"imdb_score":null,"num_movies":0}
{"actor_name":"Austin Butler","total_oscar_score":2,"imdb_score":77.0,"num_movies":1}
{"actor_name":"Tom Courtenay","total_oscar_score":3,"imdb_score":null,"num_movies":0}
{"actor_name":"Brendan Gleeson","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Stephanie Hsu","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Jack Warden","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Tom Cruise","total_oscar_score":5,"imdb_score":65.6,"num_movies":35}
{"actor_name":"Carrie Snodgress","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Diahann Carroll","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Spring Byington","total_oscar_score":1,"imdb_score":null,"num_movies":0}
{"actor_name":"Sal Mineo","total_oscar_score":2,"imdb_score":null,"num_movies":0}
{"actor_name":"Woody Harrelson","total_oscar_score":4,"imdb_score":70.0,"num_movies":7}
{"actor_name":"Kathy Bates","total_oscar_score":8,"imdb_score":75.0,"num_movies":2}
{"actor_name":"Bessie Love","total_oscar_score":2,"imdb_score":null,"num_movies":0}


];

// Format data for Chart.js scatter plot
const scatterData = data.map(item => ({
    x: item.total_oscar_score,  // Oscar Score (x-axis)
    y: item.imdb_score,    // IMDb Score (y-axis)
    label: item.actor_name
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