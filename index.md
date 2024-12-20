---
layout: full
---

Welcome to the BlueSweater Lab, where we treat movies as complex chemical reactions—combining various elements that can either explode into blockbuster success or fizzle into obscurity!

<p style="font-weight: 700"> Our goal is to identify the key ingredients that contribute to a movie’s success and uncover what truly makes a film resonate with audiences.</p>

In this data story, we'll examine how different elements such as ratings, star power, Oscar wins, genre, and production country interact and influence a movie’s journey to the top. But first, meet the team :)

<h2> Meet Our BlueSweater Lab Members 🔬🧪🥽 </h2>

<div class="team-container">
    <div class="team-member">
        <img src="scientists_pics/ivan.png" alt="Ivan"/>
        <p class="name">Ivan</p>
        <p class="title">MSc in MTE</p>
    </div>
    <div class="team-member">
        <img src="scientists_pics/david.jpg" alt="David"/>
        <p class="name">David</p>
        <p class="title">MSc in CS</p>
    </div>
    <div class="team-member">
        <img src="scientists_pics/adam.jpg" alt="Adam"/>
        <p class="name">Adam</p>
        <p class="title">MSc in CS</p>
    </div>
    <div class="team-member">
        <img src="scientists_pics/ali.png" alt="Ali"/>
        <p class="name">Ali</p>
        <p class="title">MSc in EEE</p>
    </div>
    <div class="team-member">
        <img src="scientists_pics/dana.png" alt="Dana"/>
        <p class="name">Dana</p>
        <p class="title">MSc in NX</p>
    </div>
</div>

<style>
  /* Container for the team members */
  .team-container {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    justify-content: center;
  }

  /* Team member card */
  .team-member {
    text-align: center;
    flex: 1 1 150px; /* Adjusts to screen size with min-width */
    max-width: 180px; 
  }

  /* Images for team members */
  .team-member img {
    width: 100%; 
    max-width: 160px; 
    height: auto; 
    border-radius: 5px; 
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.1); 
    transition: all 0.3s ease;
  }

  /* Image hover effects */
  .team-member img:hover {
    transform: scale(1.1) rotate(3deg); 
    box-shadow: 4px 4px 12px rgba(0, 0, 0, 0.2); 
    opacity: 0.9; 
  }

  /* Name and title styles */
  .team-member .name {
    margin-bottom: 5px;
    font-weight: bold;
  }

  .team-member .title {
    margin-top: 0; 
    margin-bottom: 0; 
    font-size: 0.9rem;
  }

  /* Responsive Design */
  @media (max-width: 768px) {
    .team-member {
      flex: 1 1 120px; 
      max-width: 120px; 
    }

    .team-member img {
      max-width: 120px;
    }

    .team-member .name {
      font-size: 0.9rem;
    }

    .team-member .title {
      font-size: 0.8rem;
    }
  }

  @media (max-width: 480px) {
    .team-container {
      flex-direction: column; 
      align-items: center; 
    }

    .team-member {
      width: 100%;
      max-width: 100px; 
    }

    .team-member img {
      max-width: 100px;
    }

    .team-member .name {
      font-size: 0.85rem;
    }

    .team-member .title {
      font-size: 0.75rem;
    }
  }
</style>

<h2> Let’s get started by deriving our ingredients... </h2>

The primary dataset we'll use for this analysis is the **CMU Movie Dataset**, which contains key information like:

- Movie ID (Wikipedia, Freebase)
- Movie Name
- Release Date
- Box Office Revenue
- Runtime
- Language & Country Information
- Genres

To enrich our analysis, we’ll integrate several additional datasets:

- **TMDB Data**: Offers movie popularity, release data, and box office details.
- **IMDB Data**: Provides movie ratings, genres, and detailed information.
- **Oscars Data**: Includes nominations, wins, and key statistics about the Academy Awards.
- **Metacritic Data**: Aggregates critics' reviews and scores.

With our enriched data, our master dataset now includes:
- 81,855 rows
- 24 columns (including IMDb ratings, budget, production companies, and more)

However, not all columns will be used in the analysis, as we’ll focus on the most relevant factors for our success prediction model.

<h2> Addressing key issues with our dataset 🌪️ 🌀</h2>

<h3> 1) Duplicates </h3>
There are 6,377 duplicates in the dataset. Initially, we considered dropping movies with the same title, but this approach could remove important variations—such as different release years or production countries. 

For example, "100 Days" appears twice: once in 1991 and again in 2001, representing two different adaptations of the same storyline. Therefore, we will keep duplicates to preserve the richness and diversity of the data.

<h3> 2) Nulls</h3>

<div id="null-chart"></div>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  var data = [{
    type: 'bar',
    x: [
      'Box Office Revenue', 'Producers', 'Production Companies', 'Writers', 
      'IMDb Votes', 'IMDb Rating', 'Director', 'Vote Average', 'Popularity', 
      'Budget', 'Revenue', 'Vote Count', 'Runtime', 'IMDb ID', 'Release Date', 
      'Freebase Movie ID', 'Genre Name', 'Genre Freebase ID', 'Country Name', 
      'Country Freebase ID', 'Language Name', 'Language Freebase ID', 'Movie Name', 
      'Wikipedia Movie ID'
    ],
    y: [
      89.73, 51.26, 41.17, 36.16, 30.26, 30.26, 28.55, 27.43, 27.43, 27.43, 27.43, 
      27.43, 25.04, 22.61, 8.46, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 
      0.00, 0.00
    ],
    marker: { color: 'dark blue' },
    hovertemplate: '%{y}%<extra></extra>' 
  }];
  
  var layout = {
    title: 'Percentage of Null Values in Movie Dataset Columns',
    xaxis: {
      title: 'Dataset Columns',
      tickangle: 45
    },
    yaxis: {
      title: 'Null Percentage (%)'
    },
    margin: {
      b: 200
    }
  };
  
  Plotly.newPlot('null-chart', data, layout);
</script>

Our dataset contains numerous missing values, particularly in the Box Office Revenue column. This creates challenges for standard data-cleaning methods, such as removing rows with null values or imputing them with the mean, as these approaches could lead to significant data loss. However, since Box Office Revenue is a component of Total Movie Revenue—which has fewer missing values—we can prioritize using both Box Office Revenue and Total Revenue for analysis.

For other columns with missing data—such as Producers, Production Company, Writers, and Director—we've decided not to prioritize them in our analysis. Additional details on this decision can be found below.

To deal with the rest of the nulls, we employed two strategies:

- **Helper Datasets (TMDB, IMDB, Metacritic)**: These datasets help fill in missing data points with simple data frame merges. 
- **OMDB API**: In a bid to tackle the remaining nulls, we purchased an API key to pull additional information and fill gaps, particularly for Box Office Revenue, Runtime, and IMDb Vote Averages. 



| Column             | Before OMDB API (Nulls) | After OMDB API (Nulls) |
|--------------------|-------------------------|------------------------|
| Box Office Revenue | 73,452                  | 68,270                 |
| Runtime            | 20,498                  | 11,642                 |
| IMDB Votes         | 22,452                  | 21,044                 |

The OMDB API successfully replaced hundreds of null values, improving the dataset compared to its previous state.

<h2>  Derived chemical movie ingredients 🧪</h2>

From the above dataset, its helpers and api, we have derived the following chemical movie ingredietns that we will be exploring in our project:

<h3> Section 1: Oscars </h3>
- How does the Oscar Score correlate with performance metrics such as audience ratings?

<h3> Section 2: Budget and Success Correlation </h3>
- To what extent does a high budget predict commercial success?

<h3> Section 3: Timing and Audience Reception</h3>
- How do seasonal and holiday release periods impact a film’s box office performance and audience reception?

<h3> Section 4: Global Influence in Cinema  </h3>
- Which countries and languages dominate the global film industry, and how do they influence critical metrics such as budget and revenue?

 
<h3> Section 5: Tropes and their influence on Movie Success</h3>
- Which character tropes and gender pairings have the greatest impact on a film’s success, measured by box office revenue, critics’ ratings, and audience ratings? 
- How do genre-specific archetypes and lead dynamics contribute to commercial and critical acclaim?

<h3> Section 6: Regression analysis </h3>
- In the regression settings, which factors can help predict success of the released movie? How do they interact?

<br>
<!-- OSCARS -->
<h1> Oscars 🏆</h1>

When people think about a movie's success, the first thing that often comes to mind is the actors. A well-known actor can elevate a movie, making it more popular, drawing larger audiences, and ultimately contributing to its commercial success. Conversely, an unknown actor may not have the same impact. In this section, we aim to explore whether there is a relationship between an actor's success and the average movie ratings they appear in.

<h2> What does it mean for an actor to be successful?</h2>
Before diving into the analysis, we must first define what constitutes a "successful actor." For many in the industry, winning an Oscar is the pinnacle of achievement. While an actor's success can be measured in many ways, we will define a successful actor as one who has won the highest number of Oscars.

However, Best Actor and Best Actress aren’t the only possible Oscar wins; actors can also receive recognition in Supporting Actor and Supporting Actress categories. Additionally, Oscar nominations play a significant role, as even a nomination can enhance an actor’s reputation and attract audiences.

However, lead roles generally have a larger impact on a movie’s success, as these actors often drive the story and have more screen time.  For this reason, we are going to build an Oscar score that applies weights to different award categories, giving greater significance to lead roles and including nominations to reflect their influence. We have decided to apply the following weights:

<h2> Oscar Score Formula,</h2>

- **LAO**: *Lead Actor Oscar Win* (**5 points**)
- **SAO**: *Supporting Actor Oscar Win* (**3 points**)
- **LAN**: *Lead Actor Oscar Nomination* (**2 points**)
- **SAN**: *Supporting Actor Oscar Nomination* (**1 point**)

**Oscar Score** = **(#LAO × 5)** + **(#SAO × 3)** + **(#LAN × 2)** + **(#SAN × 1)**

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


<h3>Find your favourite actor's oscar score! </h3>

Type in the name of an actor in the search bar below to see their IMDb score, Oscar score, and number of movies they have appeared in.
<!-- Embed the data -->
<script id="actor-data" type="application/json">
  {{ site.data.oscars | jsonify }}
</script>
{% raw %}
<div id="search-container">
    <input type="text" id="actor-search" placeholder="Enter an actor's name..." style="width: 300px; padding: 10px;">
</div>
<div id="result" style="margin-top: 20px; "></div>

<script>
const actorDataScript = document.getElementById('actor-data').textContent;
  const actors = JSON.parse(actorDataScript);

document.getElementById('actor-search').addEventListener('input', function() {
    const query = this.value.toLowerCase();
    const resultContainer = document.getElementById('result');
    resultContainer.innerHTML = '';

    if (query === '') return;

    const foundActor = actors.find(actor => actor.actor_name.toLowerCase().includes(query));
    if (foundActor) {
        resultContainer.innerHTML = `
            <h4>⭐ ${foundActor.actor_name}</h4>
            <p>📚 IMDb Score: ${foundActor.imdb_score.toFixed(2)}</p>
            <p>🏆 Oscar Score: ${foundActor.total_oscar_score}</p>
            <p>🎥 Number of Movies: ${foundActor.num_movies}</p>
        `;
    } else {
        resultContainer.innerHTML = `<p style="color:red;">❌ Actor "${this.value}" has no Score.</p>`;
    }
});
</script>

{% endraw %}

Did you find who you were looking for? If not, try searching for another actor! 🎬


<h2> IMDb Score vs Oscar Score</h2>

Now that we have calculated the oscar score, let us see if it has a correlation with IMDb movie ratings:

<canvas id="imdbOscarScatterPlot" width="600" height="400"></canvas>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-trendline"></script> 

<script>
// Corrected IMDb Scores and Oscar Scores (Real Data)
const imdbData = [{"actor_name":"Meryl Streep","total_oscar_score":46,"imdb_score":67.375,"num_movies":8},{"actor_name":"Katharine Hepburn","total_oscar_score":36,"imdb_score":76.0,"num_movies":1},{"actor_name":"Bette Davis","total_oscar_score":28,"imdb_score":80.5,"num_movies":2},{"actor_name":"Jack Nicholson","total_oscar_score":28,"imdb_score":73.3,"num_movies":10},{"actor_name":"Spencer Tracy","total_oscar_score":24,"imdb_score":75.0,"num_movies":2},{"actor_name":"Denzel Washington","total_oscar_score":21,"imdb_score":65.8387096774,"num_movies":31},{"actor_name":"Marlon Brando","total_oscar_score":21,"imdb_score":78.2,"num_movies":5},{"actor_name":"Jack Lemmon","total_oscar_score":20,"imdb_score":69.0,"num_movies":2},{"actor_name":"Paul Newman","total_oscar_score":20,"imdb_score":74.8,"num_movies":5},{"actor_name":"Laurence Olivier","total_oscar_score":20,"imdb_score":79.0,"num_movies":1},{"actor_name":"Dustin Hoffman","total_oscar_score":20,"imdb_score":70.4,"num_movies":10},{"actor_name":"Jane Fonda","total_oscar_score":19,"imdb_score":60.0,"num_movies":1},{"actor_name":"Robert De Niro","total_oscar_score":18,"imdb_score":69.3666666667,"num_movies":30},{"actor_name":"Cate Blanchett","total_oscar_score":18,"imdb_score":67.6666666667,"num_movies":6},{"actor_name":"Frances McDormand","total_oscar_score":18,"imdb_score":75.0,"num_movies":4},{"actor_name":"Al Pacino","total_oscar_score":17,"imdb_score":73.0,"num_movies":13},{"actor_name":"Tom Hanks","total_oscar_score":17,"imdb_score":71.2307692308,"num_movies":39},{"actor_name":"Anthony Hopkins","total_oscar_score":16,"imdb_score":67.75,"num_movies":12},{"actor_name":"Daniel Day-Lewis","total_oscar_score":16,"imdb_score":73.6666666667,"num_movies":6},{"actor_name":"Peter O'Toole","total_oscar_score":16,"imdb_score":80.0,"num_movies":1},{"actor_name":"Gary Cooper","total_oscar_score":16,"imdb_score":77.0,"num_movies":1},{"actor_name":"Sean Penn","total_oscar_score":16,"imdb_score":70.5555555556,"num_movies":9},{"actor_name":"Elizabeth Taylor","total_oscar_score":16,"imdb_score":74.6666666667,"num_movies":3},{"actor_name":"Paul Muni","total_oscar_score":15,"imdb_score":75.0,"num_movies":1},{"actor_name":"Sissy Spacek","total_oscar_score":15,"imdb_score":73.0,"num_movies":2},{"actor_name":"Judi Dench","total_oscar_score":15,"imdb_score":70.5,"num_movies":2},{"actor_name":"Ellen Burstyn","total_oscar_score":14,"imdb_score":78.5,"num_movies":2},{"actor_name":"Jodie Foster","total_oscar_score":14,"imdb_score":69.3333333333,"num_movies":6},{"actor_name":"Leonardo DiCaprio","total_oscar_score":14,"imdb_score":70.1428571429,"num_movies":21},{"actor_name":"Michael Caine","total_oscar_score":14,"imdb_score":64.375,"num_movies":8},{"actor_name":"Kate Winslet","total_oscar_score":14,"imdb_score":70.625,"num_movies":8},{"actor_name":"Richard Burton","total_oscar_score":13,"imdb_score":63.3333333333,"num_movies":3},{"actor_name":"Robert Duvall","total_oscar_score":13,"imdb_score":63.5,"num_movies":2},{"actor_name":"Anne Bancroft","total_oscar_score":13,"imdb_score":77.0,"num_movies":1},{"actor_name":"Audrey Hepburn","total_oscar_score":13,"imdb_score":75.1666666667,"num_movies":6},{"actor_name":"Gregory Peck","total_oscar_score":13,"imdb_score":73.5,"num_movies":8},{"actor_name":"Jeff Bridges","total_oscar_score":13,"imdb_score":66.2352941176,"num_movies":17},{"actor_name":"Susan Sarandon","total_oscar_score":13,"imdb_score":70.6666666667,"num_movies":3},{"actor_name":"Shirley MacLaine","total_oscar_score":13,"imdb_score":70.5,"num_movies":2},{"actor_name":"James Stewart","total_oscar_score":13,"imdb_score":79.4,"num_movies":5},{"actor_name":"Ren\u00e9e Zellweger","total_oscar_score":12,"imdb_score":62.3333333333,"num_movies":6},{"actor_name":"Emma Stone","total_oscar_score":12,"imdb_score":69.0,"num_movies":5},{"actor_name":"Glenn Close","total_oscar_score":12,"imdb_score":65.2,"num_movies":5},{"actor_name":"Gene Hackman","total_oscar_score":12,"imdb_score":71.5714285714,"num_movies":7},{"actor_name":"Nicole Kidman","total_oscar_score":12,"imdb_score":65.125,"num_movies":16},{"actor_name":"Burt Lancaster","total_oscar_score":11,"imdb_score":67.75,"num_movies":4},{"actor_name":"Diane Keaton","total_oscar_score":11,"imdb_score":59.6,"num_movies":5},{"actor_name":"Sally Field","total_oscar_score":11,"imdb_score":66.6666666667,"num_movies":3},{"actor_name":"Julianne Moore","total_oscar_score":11,"imdb_score":63.0,"num_movies":4},{"actor_name":"Jennifer Lawrence","total_oscar_score":10,"imdb_score":61.8181818182,"num_movies":11},{"actor_name":"William Hurt","total_oscar_score":10,"imdb_score":69.0,"num_movies":4},{"actor_name":"Jon Voight","total_oscar_score":10,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"Hilary Swank","total_oscar_score":10,"imdb_score":67.875,"num_movies":8},{"actor_name":"Anthony Quinn","total_oscar_score":10,"imdb_score":72.5,"num_movies":2},{"actor_name":"Emma Thompson","total_oscar_score":10,"imdb_score":69.6666666667,"num_movies":6},{"actor_name":"Morgan Freeman","total_oscar_score":10,"imdb_score":61.9,"num_movies":10},{"actor_name":"Joaquin Phoenix","total_oscar_score":10,"imdb_score":65.2727272727,"num_movies":11},{"actor_name":"Vivien Leigh","total_oscar_score":10,"imdb_score":78.5,"num_movies":2},{"actor_name":"Clark Gable","total_oscar_score":9,"imdb_score":79.0,"num_movies":1},{"actor_name":"Charles Laughton","total_oscar_score":9,"imdb_score":76.3333333333,"num_movies":3},{"actor_name":"Charlize Theron","total_oscar_score":9,"imdb_score":65.4444444444,"num_movies":9},{"actor_name":"Javier Bardem","total_oscar_score":9,"imdb_score":64.0,"num_movies":3},{"actor_name":"Ben Kingsley","total_oscar_score":9,"imdb_score":67.375,"num_movies":8},{"actor_name":"Bing Crosby","total_oscar_score":9,"imdb_score":65.0,"num_movies":1},{"actor_name":"Bradley Cooper","total_oscar_score":9,"imdb_score":67.5454545455,"num_movies":11},{"actor_name":"Annette Bening","total_oscar_score":9,"imdb_score":73.0,"num_movies":1},{"actor_name":"William Holden","total_oscar_score":9,"imdb_score":75.75,"num_movies":4},{"actor_name":"Will Smith","total_oscar_score":9,"imdb_score":68.6538461538,"num_movies":26},{"actor_name":"Albert Finney","total_oscar_score":9,"imdb_score":72.0,"num_movies":1},{"actor_name":"Gary Oldman","total_oscar_score":9,"imdb_score":65.7142857143,"num_movies":7},{"actor_name":"Geoffrey Rush","total_oscar_score":9,"imdb_score":67.3333333333,"num_movies":3},{"actor_name":"Julie Andrews","total_oscar_score":9,"imdb_score":76.5,"num_movies":2},{"actor_name":"George C. Scott","total_oscar_score":9,"imdb_score":68.5,"num_movies":4},{"actor_name":"George Clooney","total_oscar_score":9,"imdb_score":64.380952381,"num_movies":21},{"actor_name":"Russell Crowe","total_oscar_score":9,"imdb_score":69.875,"num_movies":16},{"actor_name":"Robin Williams","total_oscar_score":9,"imdb_score":68.4666666667,"num_movies":15},{"actor_name":"Julia Roberts","total_oscar_score":9,"imdb_score":66.1538461538,"num_movies":13},{"actor_name":"Helen Mirren","total_oscar_score":9,"imdb_score":71.0,"num_movies":3},{"actor_name":"Holly Hunter","total_oscar_score":9,"imdb_score":74.0,"num_movies":1},{"actor_name":"Humphrey Bogart","total_oscar_score":9,"imdb_score":78.2,"num_movies":5},{"actor_name":"Charles Boyer","total_oscar_score":8,"imdb_score":75.0,"num_movies":1},{"actor_name":"Kathy Bates","total_oscar_score":8,"imdb_score":75.0,"num_movies":2},{"actor_name":"Jessica Chastain","total_oscar_score":8,"imdb_score":67.75,"num_movies":8},{"actor_name":"Pen\u00e9lope Cruz","total_oscar_score":8,"imdb_score":68.0,"num_movies":4},{"actor_name":"Alan Arkin","total_oscar_score":8,"imdb_score":72.5,"num_movies":2},{"actor_name":"Warren Beatty","total_oscar_score":8,"imdb_score":67.0,"num_movies":3},{"actor_name":"Viola Davis","total_oscar_score":8,"imdb_score":71.5,"num_movies":2},{"actor_name":"Rod Steiger","total_oscar_score":8,"imdb_score":77.0,"num_movies":1},{"actor_name":"Helen Hayes","total_oscar_score":8,"imdb_score":61.0,"num_movies":1},{"actor_name":"Kevin Spacey","total_oscar_score":8,"imdb_score":70.4,"num_movies":5},{"actor_name":"Philip Seymour Hoffman","total_oscar_score":8,"imdb_score":68.0,"num_movies":2},{"actor_name":"Olivia Colman","total_oscar_score":8,"imdb_score":69.3333333333,"num_movies":3},{"actor_name":"Natalie Portman","total_oscar_score":8,"imdb_score":67.75,"num_movies":8},{"actor_name":"Michelle Williams","total_oscar_score":8,"imdb_score":69.8,"num_movies":5},{"actor_name":"Christian Bale","total_oscar_score":8,"imdb_score":70.2727272727,"num_movies":22},{"actor_name":"Brad Pitt","total_oscar_score":8,"imdb_score":70.4782608696,"num_movies":23},{"actor_name":"Colin Firth","total_oscar_score":7,"imdb_score":65.7142857143,"num_movies":7},{"actor_name":"Henry Fonda","total_oscar_score":7,"imdb_score":78.0,"num_movies":2},{"actor_name":"John Wayne","total_oscar_score":7,"imdb_score":72.5833333333,"num_movies":12},{"actor_name":"Eddie Redmayne","total_oscar_score":7,"imdb_score":71.5,"num_movies":8},{"actor_name":"Sidney Poitier","total_oscar_score":7,"imdb_score":77.0,"num_movies":1},{"actor_name":"Tommy Lee Jones","total_oscar_score":7,"imdb_score":63.875,"num_movies":8},{"actor_name":"Sandra Bullock","total_oscar_score":7,"imdb_score":65.9130434783,"num_movies":23},{"actor_name":"Saoirse Ronan","total_oscar_score":7,"imdb_score":70.3333333333,"num_movies":9},{"actor_name":"Barbra Streisand","total_oscar_score":7,"imdb_score":65.6666666667,"num_movies":3},{"actor_name":"Sophia Loren","total_oscar_score":7,"imdb_score":65.0,"num_movies":1},{"actor_name":"Marion Cotillard","total_oscar_score":7,"imdb_score":66.0,"num_movies":1},{"actor_name":"Richard Dreyfuss","total_oscar_score":7,"imdb_score":72.0,"num_movies":2},{"actor_name":"Rex Harrison","total_oscar_score":7,"imdb_score":62.0,"num_movies":1},{"actor_name":"Amy Adams","total_oscar_score":7,"imdb_score":69.7142857143,"num_movies":7},{"actor_name":"Reese Witherspoon","total_oscar_score":7,"imdb_score":62.7,"num_movies":10},{"actor_name":"Liza Minnelli","total_oscar_score":7,"imdb_score":74.0,"num_movies":1},{"actor_name":"Walter Matthau","total_oscar_score":7,"imdb_score":58.0,"num_movies":1},{"actor_name":"Peter Ustinov","total_oscar_score":7,"imdb_score":66.0,"num_movies":2},{"actor_name":"Nicolas Cage","total_oscar_score":7,"imdb_score":59.3442622951,"num_movies":61},{"actor_name":"Peter Finch","total_oscar_score":7,"imdb_score":79.0,"num_movies":1},{"actor_name":"Kirk Douglas","total_oscar_score":6,"imdb_score":73.4,"num_movies":5},{"actor_name":"Helen Hunt","total_oscar_score":6,"imdb_score":67.5,"num_movies":2},{"actor_name":"Robert Downey Jr.","total_oscar_score":6,"imdb_score":72.8823529412,"num_movies":17},{"actor_name":"Casey Affleck","total_oscar_score":6,"imdb_score":69.75,"num_movies":4},{"actor_name":"Anjelica Huston","total_oscar_score":6,"imdb_score":70.0,"num_movies":1},{"actor_name":"Viggo Mortensen","total_oscar_score":6,"imdb_score":73.5,"num_movies":8},{"actor_name":"Cher","total_oscar_score":6,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Christoph Waltz","total_oscar_score":6,"imdb_score":57.0,"num_movies":1},{"actor_name":"Laura Dern","total_oscar_score":6,"imdb_score":71.0,"num_movies":1},{"actor_name":"Carey Mulligan","total_oscar_score":6,"imdb_score":71.3333333333,"num_movies":6},{"actor_name":"Marcello Mastroianni","total_oscar_score":6,"imdb_score":82.0,"num_movies":1},{"actor_name":"Johnny Depp","total_oscar_score":6,"imdb_score":67.8620689655,"num_movies":29},{"actor_name":"Jamie Foxx","total_oscar_score":6,"imdb_score":69.0,"num_movies":11},{"actor_name":"Mahershala Ali","total_oscar_score":6,"imdb_score":73.0,"num_movies":1},{"actor_name":"Mickey Rooney","total_oscar_score":6,"imdb_score":63.0,"num_movies":2},{"actor_name":"Michelle Yeoh","total_oscar_score":5,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Octavia Spencer","total_oscar_score":5,"imdb_score":58.0,"num_movies":1},{"actor_name":"Geena Davis","total_oscar_score":5,"imdb_score":58.0,"num_movies":1},{"actor_name":"Nick Nolte","total_oscar_score":5,"imdb_score":63.0,"num_movies":4},{"actor_name":"Tom Cruise","total_oscar_score":5,"imdb_score":65.6,"num_movies":35},{"actor_name":"Ed Harris","total_oscar_score":5,"imdb_score":64.5,"num_movies":4},{"actor_name":"Louise Fletcher","total_oscar_score":5,"imdb_score":62.0,"num_movies":1},{"actor_name":"Frank Sinatra","total_oscar_score":5,"imdb_score":64.0,"num_movies":1},{"actor_name":"Forest Whitaker","total_oscar_score":5,"imdb_score":71.3333333333,"num_movies":3},{"actor_name":"F. Murray Abraham","total_oscar_score":5,"imdb_score":80.0,"num_movies":1},{"actor_name":"Lee Marvin","total_oscar_score":5,"imdb_score":76.0,"num_movies":1},{"actor_name":"Joe Pesci","total_oscar_score":5,"imdb_score":75.0,"num_movies":1},{"actor_name":"Juliette Binoche","total_oscar_score":5,"imdb_score":65.75,"num_movies":4},{"actor_name":"Whoopi Goldberg","total_oscar_score":5,"imdb_score":64.6666666667,"num_movies":3},{"actor_name":"Willem Dafoe","total_oscar_score":5,"imdb_score":70.5714285714,"num_movies":7},{"actor_name":"Laura Linney","total_oscar_score":5,"imdb_score":60.0,"num_movies":2},{"actor_name":"Jack Palance","total_oscar_score":5,"imdb_score":66.0,"num_movies":1},{"actor_name":"Rami Malek","total_oscar_score":5,"imdb_score":80.0,"num_movies":1},{"actor_name":"Marisa Tomei","total_oscar_score":5,"imdb_score":60.0,"num_movies":1},{"actor_name":"Jean Dujardin","total_oscar_score":5,"imdb_score":72.75,"num_movies":4},{"actor_name":"Goldie Hawn","total_oscar_score":5,"imdb_score":67.6666666667,"num_movies":3},{"actor_name":"Michelle Pfeiffer","total_oscar_score":5,"imdb_score":63.3333333333,"num_movies":6},{"actor_name":"Michael Douglas","total_oscar_score":5,"imdb_score":66.4666666667,"num_movies":15},{"actor_name":"Heath Ledger","total_oscar_score":5,"imdb_score":67.25,"num_movies":4},{"actor_name":"Halle Berry","total_oscar_score":5,"imdb_score":61.1428571429,"num_movies":7},{"actor_name":"Gwyneth Paltrow","total_oscar_score":5,"imdb_score":62.0,"num_movies":1},{"actor_name":"Roberto Benigni","total_oscar_score":5,"imdb_score":64.75,"num_movies":4},{"actor_name":"Ryan Gosling","total_oscar_score":5,"imdb_score":71.9,"num_movies":10},{"actor_name":"Jeremy Irons","total_oscar_score":5,"imdb_score":70.0,"num_movies":4},{"actor_name":"Matthew McConaughey","total_oscar_score":5,"imdb_score":66.2380952381,"num_movies":21},{"actor_name":"Natalie Wood","total_oscar_score":5,"imdb_score":73.0,"num_movies":2},{"actor_name":"Matt Damon","total_oscar_score":5,"imdb_score":68.3181818182,"num_movies":22},{"actor_name":"Ray Milland","total_oscar_score":5,"imdb_score":78.5,"num_movies":2},{"actor_name":"Martin Landau","total_oscar_score":5,"imdb_score":53.0,"num_movies":1},{"actor_name":"Sigourney Weaver","total_oscar_score":5,"imdb_score":64.5714285714,"num_movies":7},{"actor_name":"Yul Brynner","total_oscar_score":5,"imdb_score":67.25,"num_movies":4},{"actor_name":"Anne Hathaway","total_oscar_score":5,"imdb_score":63.5714285714,"num_movies":14},{"actor_name":"Brendan Fraser","total_oscar_score":5,"imdb_score":61.1875,"num_movies":16},{"actor_name":"Cillian Murphy","total_oscar_score":5,"imdb_score":54.25,"num_movies":4},{"actor_name":"Christopher Plummer","total_oscar_score":5,"imdb_score":70.5,"num_movies":2},{"actor_name":"Adrien Brody","total_oscar_score":5,"imdb_score":68.625,"num_movies":8},{"actor_name":"Charlton Heston","total_oscar_score":5,"imdb_score":71.4285714286,"num_movies":14},{"actor_name":"Brie Larson","total_oscar_score":5,"imdb_score":71.4,"num_movies":5},{"actor_name":"Daniel Kaluuya","total_oscar_score":5,"imdb_score":72.5,"num_movies":2},{"actor_name":"David Niven","total_oscar_score":5,"imdb_score":56.8333333333,"num_movies":6},{"actor_name":"Angelina Jolie","total_oscar_score":5,"imdb_score":66.3,"num_movies":10},{"actor_name":"Benedict Cumberbatch","total_oscar_score":4,"imdb_score":72.875,"num_movies":8},{"actor_name":"James Mason","total_oscar_score":4,"imdb_score":74.0,"num_movies":2},{"actor_name":"Clint Eastwood","total_oscar_score":4,"imdb_score":70.1666666667,"num_movies":30},{"actor_name":"Bette Midler","total_oscar_score":4,"imdb_score":72.5,"num_movies":2},{"actor_name":"Walter Pidgeon","total_oscar_score":4,"imdb_score":73.0,"num_movies":1},{"actor_name":"Naomi Watts","total_oscar_score":4,"imdb_score":64.7647058824,"num_movies":17},{"actor_name":"Sam Rockwell","total_oscar_score":4,"imdb_score":62.8333333333,"num_movies":6},{"actor_name":"Clifton Webb","total_oscar_score":4,"imdb_score":66.0,"num_movies":1},{"actor_name":"Peter Sellers","total_oscar_score":4,"imdb_score":72.5,"num_movies":4},{"actor_name":"Jill Clayburgh","total_oscar_score":4,"imdb_score":65.0,"num_movies":1},{"actor_name":"Mark Ruffalo","total_oscar_score":4,"imdb_score":75.3333333333,"num_movies":3},{"actor_name":"Isabelle Adjani","total_oscar_score":4,"imdb_score":72.3333333333,"num_movies":3},{"actor_name":"Richard Harris","total_oscar_score":4,"imdb_score":54.5,"num_movies":4},{"actor_name":"Andrew Garfield","total_oscar_score":4,"imdb_score":70.5,"num_movies":8},{"actor_name":"Cary Grant","total_oscar_score":4,"imdb_score":76.2,"num_movies":5},{"actor_name":"Rachel Weisz","total_oscar_score":4,"imdb_score":65.75,"num_movies":4},{"actor_name":"Christopher Walken","total_oscar_score":4,"imdb_score":64.8,"num_movies":5},{"actor_name":"Emily Watson","total_oscar_score":4,"imdb_score":66.3333333333,"num_movies":3},{"actor_name":"Woody Harrelson","total_oscar_score":4,"imdb_score":70.0,"num_movies":7},{"actor_name":"Edward Norton","total_oscar_score":4,"imdb_score":72.375,"num_movies":8},{"actor_name":"John Travolta","total_oscar_score":4,"imdb_score":62.75,"num_movies":20},{"actor_name":"James Dean","total_oscar_score":4,"imdb_score":76.0,"num_movies":1},{"actor_name":"J.K. Simmons","total_oscar_score":4,"imdb_score":57.0,"num_movies":2},{"actor_name":"Burl Ives","total_oscar_score":3,"imdb_score":74.0,"num_movies":1},{"actor_name":"Jamie Lee Curtis","total_oscar_score":3,"imdb_score":62.3,"num_movies":10},{"actor_name":"James Woods","total_oscar_score":3,"imdb_score":68.0,"num_movies":2},{"actor_name":"James Whitmore","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Jared Leto","total_oscar_score":3,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Rooney Mara","total_oscar_score":3,"imdb_score":70.25,"num_movies":4},{"actor_name":"Margot Robbie","total_oscar_score":3,"imdb_score":55.2,"num_movies":5},{"actor_name":"Tilda Swinton","total_oscar_score":3,"imdb_score":73.3333333333,"num_movies":3},{"actor_name":"Tim Robbins","total_oscar_score":3,"imdb_score":77.3333333333,"num_movies":3},{"actor_name":"Bruce Dern","total_oscar_score":3,"imdb_score":63.5,"num_movies":2},{"actor_name":"Cloris Leachman","total_oscar_score":3,"imdb_score":53.0,"num_movies":1},{"actor_name":"Roy Scheider","total_oscar_score":3,"imdb_score":66.0,"num_movies":3},{"actor_name":"Jude Law","total_oscar_score":3,"imdb_score":67.0,"num_movies":3},{"actor_name":"Scarlett Johansson","total_oscar_score":3,"imdb_score":64.375,"num_movies":8},{"actor_name":"Sean Connery","total_oscar_score":3,"imdb_score":66.0,"num_movies":15},{"actor_name":"Judy Garland","total_oscar_score":3,"imdb_score":76.0,"num_movies":1},{"actor_name":"Catherine Zeta-Jones","total_oscar_score":3,"imdb_score":64.0,"num_movies":3},{"actor_name":"Lupita Nyong'o","total_oscar_score":3,"imdb_score":70.0,"num_movies":1},{"actor_name":"Kim Basinger","total_oscar_score":3,"imdb_score":56.0,"num_movies":1},{"actor_name":"Kevin Kline","total_oscar_score":3,"imdb_score":69.0,"num_movies":3},{"actor_name":"Angela Bassett","total_oscar_score":3,"imdb_score":50.0,"num_movies":1},{"actor_name":"Angela Lansbury","total_oscar_score":3,"imdb_score":70.0,"num_movies":1},{"actor_name":"Kenneth Branagh","total_oscar_score":3,"imdb_score":68.0,"num_movies":3},{"actor_name":"Keira Knightley","total_oscar_score":3,"imdb_score":67.8181818182,"num_movies":11},{"actor_name":"Adam Driver","total_oscar_score":3,"imdb_score":67.6666666667,"num_movies":6},{"actor_name":"John Mills","total_oscar_score":3,"imdb_score":72.0,"num_movies":1},{"actor_name":"John Hurt","total_oscar_score":3,"imdb_score":66.0,"num_movies":2},{"actor_name":"Jennifer Connelly","total_oscar_score":3,"imdb_score":60.0,"num_movies":4},{"actor_name":"Jennifer Hudson","total_oscar_score":3,"imdb_score":68.0,"num_movies":1},{"actor_name":"Anna Paquin","total_oscar_score":3,"imdb_score":73.0,"num_movies":1},{"actor_name":"Sylvester Stallone","total_oscar_score":3,"imdb_score":62.1081081081,"num_movies":37},{"actor_name":"Jeremy Renner","total_oscar_score":3,"imdb_score":64.6666666667,"num_movies":6},{"actor_name":"Sally Hawkins","total_oscar_score":3,"imdb_score":70.0,"num_movies":3},{"actor_name":"Mark Rylance","total_oscar_score":3,"imdb_score":67.3333333333,"num_movies":3},{"actor_name":"Alicia Vikander","total_oscar_score":3,"imdb_score":64.6666666667,"num_movies":3},{"actor_name":"Richard Jenkins","total_oscar_score":3,"imdb_score":55.0,"num_movies":1},{"actor_name":"Billy Bob Thornton","total_oscar_score":3,"imdb_score":63.75,"num_movies":4},{"actor_name":"Michael Fassbender","total_oscar_score":3,"imdb_score":61.6666666667,"num_movies":6},{"actor_name":"Patricia Arquette","total_oscar_score":3,"imdb_score":63.0,"num_movies":1},{"actor_name":"Melissa McCarthy","total_oscar_score":3,"imdb_score":61.0,"num_movies":8},{"actor_name":"Ralph Fiennes","total_oscar_score":3,"imdb_score":63.4444444444,"num_movies":9},{"actor_name":"Mo'Nique","total_oscar_score":3,"imdb_score":57.0,"num_movies":2},{"actor_name":"Don Ameche","total_oscar_score":3,"imdb_score":66.0,"num_movies":1},{"actor_name":"George Kennedy","total_oscar_score":3,"imdb_score":58.5,"num_movies":2},{"actor_name":"Miranda Richardson","total_oscar_score":3,"imdb_score":59.0,"num_movies":1},{"actor_name":"Mira Sorvino","total_oscar_score":3,"imdb_score":68.5,"num_movies":2},{"actor_name":"Paul Giamatti","total_oscar_score":3,"imdb_score":71.0,"num_movies":1},{"actor_name":"Martin Balsam","total_oscar_score":3,"imdb_score":85.0,"num_movies":1},{"actor_name":"Allison Janney","total_oscar_score":3,"imdb_score":67.0,"num_movies":1},{"actor_name":"Max von Sydow","total_oscar_score":3,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Mary Steenburgen","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Tom Wilkinson","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Winona Ryder","total_oscar_score":3,"imdb_score":70.4,"num_movies":5},{"actor_name":"Ian McKellen","total_oscar_score":3,"imdb_score":71.75,"num_movies":4},{"actor_name":"Helena Bonham Carter","total_oscar_score":3,"imdb_score":72.0,"num_movies":1},{"actor_name":"Laurence Fishburne","total_oscar_score":2,"imdb_score":66.4,"num_movies":5},{"actor_name":"Sandra H\u00fcller","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Austin Butler","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Keisha Castle-Hughes","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Paul Mescal","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Ralph Richardson","total_oscar_score":2,"imdb_score":64.0,"num_movies":1},{"actor_name":"Carroll Baker","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Orson Welles","total_oscar_score":2,"imdb_score":80.0,"num_movies":1},{"actor_name":"Quvenzhan\u00e9 Wallis","total_oscar_score":2,"imdb_score":62.0,"num_movies":1},{"actor_name":"Kristen Stewart","total_oscar_score":2,"imdb_score":63.5714285714,"num_movies":14},{"actor_name":"Kristin Scott Thomas","total_oscar_score":2,"imdb_score":73.0,"num_movies":1},{"actor_name":"Lady Gaga","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Kevin Costner","total_oscar_score":2,"imdb_score":69.1875,"num_movies":16},{"actor_name":"Rosamund Pike","total_oscar_score":2,"imdb_score":64.0,"num_movies":3},{"actor_name":"Bob Hoskins","total_oscar_score":2,"imdb_score":62.3333333333,"num_movies":3},{"actor_name":"Riz Ahmed","total_oscar_score":2,"imdb_score":70.5,"num_movies":2},{"actor_name":"Anthony Franciosa","total_oscar_score":2,"imdb_score":68.0,"num_movies":1},{"actor_name":"Bryan Cranston","total_oscar_score":2,"imdb_score":73.3333333333,"num_movies":6},{"actor_name":"Melina Mercouri","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Melanie Griffith","total_oscar_score":2,"imdb_score":65.0,"num_movies":2},{"actor_name":"Margaret Sullavan","total_oscar_score":2,"imdb_score":83.0,"num_movies":1},{"actor_name":"Robert Redford","total_oscar_score":2,"imdb_score":67.4,"num_movies":10},{"actor_name":"Sharon Stone","total_oscar_score":2,"imdb_score":56.6666666667,"num_movies":3},{"actor_name":"Michael Keaton","total_oscar_score":2,"imdb_score":67.0,"num_movies":6},{"actor_name":"Antonio Banderas","total_oscar_score":2,"imdb_score":66.1666666667,"num_movies":18},{"actor_name":"Sam Waterston","total_oscar_score":2,"imdb_score":75.0,"num_movies":1},{"actor_name":"Salma Hayek","total_oscar_score":2,"imdb_score":64.3333333333,"num_movies":3},{"actor_name":"Liam Neeson","total_oscar_score":2,"imdb_score":65.1153846154,"num_movies":26},{"actor_name":"Michael Shannon","total_oscar_score":2,"imdb_score":63.5,"num_movies":2},{"actor_name":"Ryan O'Neal","total_oscar_score":2,"imdb_score":80.0,"num_movies":1},{"actor_name":"Mickey Rourke","total_oscar_score":2,"imdb_score":65.8333333333,"num_movies":6},{"actor_name":"Bill Murray","total_oscar_score":2,"imdb_score":68.4615384615,"num_movies":13},{"actor_name":"Bill Nighy","total_oscar_score":2,"imdb_score":66.5,"num_movies":4},{"actor_name":"Felicity Jones","total_oscar_score":2,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"James Franco","total_oscar_score":2,"imdb_score":60.7,"num_movies":10},{"actor_name":"James Garner","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Topol","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Terrence Howard","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Diane Lane","total_oscar_score":2,"imdb_score":66.8,"num_movies":5},{"actor_name":"Tony Curtis","total_oscar_score":2,"imdb_score":74.5,"num_movies":2},{"actor_name":"Don Cheadle","total_oscar_score":2,"imdb_score":70.5,"num_movies":2},{"actor_name":"Ali MacGraw","total_oscar_score":2,"imdb_score":68.0,"num_movies":1},{"actor_name":"Jeffrey Wright","total_oscar_score":2,"imdb_score":54.0,"num_movies":1},{"actor_name":"Harrison Ford","total_oscar_score":2,"imdb_score":63.5263157895,"num_movies":19},{"actor_name":"Jesse Eisenberg","total_oscar_score":2,"imdb_score":65.75,"num_movies":8},{"actor_name":"Hugh Jackman","total_oscar_score":2,"imdb_score":69.8235294118,"num_movies":17},{"actor_name":"Cynthia Erivo","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Catalina Sandino Moreno","total_oscar_score":2,"imdb_score":72.0,"num_movies":1},{"actor_name":"Gene Kelly","total_oscar_score":2,"imdb_score":82.0,"num_movies":1},{"actor_name":"Imelda Staunton","total_oscar_score":2,"imdb_score":73.0,"num_movies":1},{"actor_name":"Gabourey Sidibe","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Isabelle Huppert","total_oscar_score":2,"imdb_score":59.6666666667,"num_movies":3},{"actor_name":"Colin Farrell","total_oscar_score":2,"imdb_score":65.1111111111,"num_movies":18},{"actor_name":"Timoth\u00e9e Chalamet","total_oscar_score":2,"imdb_score":73.75,"num_movies":4},{"actor_name":"Gary Busey","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Tom Hulce","total_oscar_score":2,"imdb_score":62.5,"num_movies":2},{"actor_name":"Demi\u00e1n Bichir","total_oscar_score":2,"imdb_score":68.5,"num_movies":2},{"actor_name":"Ethan Hawke","total_oscar_score":2,"imdb_score":66.5625,"num_movies":16},{"actor_name":"Dudley Moore","total_oscar_score":2,"imdb_score":58.75,"num_movies":4},{"actor_name":"Elisabeth Shue","total_oscar_score":2,"imdb_score":61.5,"num_movies":2},{"actor_name":"Jonah Hill","total_oscar_score":2,"imdb_score":67.1666666667,"num_movies":6},{"actor_name":"Jonathan Pryce","total_oscar_score":2,"imdb_score":76.0,"num_movies":2},{"actor_name":"Peter Fonda","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Chadwick Boseman","total_oscar_score":2,"imdb_score":71.5,"num_movies":4},{"actor_name":"Celia Johnson","total_oscar_score":2,"imdb_score":78.0,"num_movies":1},{"actor_name":"Andrea Riseborough","total_oscar_score":2,"imdb_score":61.25,"num_movies":4},{"actor_name":"Catherine Keener","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Edward James Olmos","total_oscar_score":2,"imdb_score":75.0,"num_movies":2},{"actor_name":"Yalitza Aparicio","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Catherine Deneuve","total_oscar_score":2,"imdb_score":72.4,"num_movies":5},{"actor_name":"Eddie Albert","total_oscar_score":2,"imdb_score":76.0,"num_movies":1},{"actor_name":"Kathleen Turner","total_oscar_score":2,"imdb_score":58.0,"num_movies":3},{"actor_name":"John Lithgow","total_oscar_score":2,"imdb_score":61.0,"num_movies":1},{"actor_name":"Woody Allen","total_oscar_score":2,"imdb_score":66.1666666667,"num_movies":6},{"actor_name":"Chiwetel Ejiofor","total_oscar_score":2,"imdb_score":70.0,"num_movies":4},{"actor_name":"Ana de Armas","total_oscar_score":2,"imdb_score":31.0,"num_movies":2},{"actor_name":"Vanessa Kirby","total_oscar_score":2,"imdb_score":63.0,"num_movies":2},{"actor_name":"Steven Yeun","total_oscar_score":2,"imdb_score":68.5,"num_movies":2},{"actor_name":"Steve McQueen","total_oscar_score":2,"imdb_score":73.3333333333,"num_movies":6},{"actor_name":"Charlotte Rampling","total_oscar_score":2,"imdb_score":65.0,"num_movies":1},{"actor_name":"Steve Carell","total_oscar_score":2,"imdb_score":64.6666666667,"num_movies":12},{"actor_name":"Stephen Rea","total_oscar_score":2,"imdb_score":69.0,"num_movies":1},{"actor_name":"Virginia Madsen","total_oscar_score":1,"imdb_score":64.0,"num_movies":2},{"actor_name":"Richard Widmark","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"William H. Macy","total_oscar_score":1,"imdb_score":58.0,"num_movies":1},{"actor_name":"Queen Latifah","total_oscar_score":1,"imdb_score":63.6,"num_movies":5},{"actor_name":"Rachel McAdams","total_oscar_score":1,"imdb_score":69.5,"num_movies":4},{"actor_name":"Adriana Barraza","total_oscar_score":1,"imdb_score":49.0,"num_movies":1},{"actor_name":"Albert Brooks","total_oscar_score":1,"imdb_score":74.0,"num_movies":2},{"actor_name":"Alec Baldwin","total_oscar_score":1,"imdb_score":63.8333333333,"num_movies":6},{"actor_name":"Amy Ryan","total_oscar_score":1,"imdb_score":61.0,"num_movies":1},{"actor_name":"Anna Kendrick","total_oscar_score":1,"imdb_score":66.6363636364,"num_movies":11},{"actor_name":"Sacha Baron Cohen","total_oscar_score":1,"imdb_score":61.6666666667,"num_movies":6},{"actor_name":"Taraji P. Henson","total_oscar_score":1,"imdb_score":56.5,"num_movies":6},{"actor_name":"Vera Farmiga","total_oscar_score":1,"imdb_score":67.0,"num_movies":2},{"actor_name":"Stanley Tucci","total_oscar_score":1,"imdb_score":60.0,"num_movies":1},{"actor_name":"Samuel L. Jackson","total_oscar_score":1,"imdb_score":63.5,"num_movies":18},{"actor_name":"Sam Shepard","total_oscar_score":1,"imdb_score":74.0,"num_movies":1},{"actor_name":"Terence Stamp","total_oscar_score":1,"imdb_score":68.0,"num_movies":1},{"actor_name":"Tom Hardy","total_oscar_score":1,"imdb_score":67.3636363636,"num_movies":11},{"actor_name":"Toni Collette","total_oscar_score":1,"imdb_score":54.4,"num_movies":5},{"actor_name":"Robert Mitchum","total_oscar_score":1,"imdb_score":76.0,"num_movies":1},{"actor_name":"Uma Thurman","total_oscar_score":1,"imdb_score":64.3333333333,"num_movies":6},{"actor_name":"Anthony Perkins","total_oscar_score":1,"imdb_score":70.0,"num_movies":2},{"actor_name":"River Phoenix","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Amanda Seyfried","total_oscar_score":1,"imdb_score":65.3,"num_movies":10},{"actor_name":"Thomas Haden Church","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Tim Roth","total_oscar_score":1,"imdb_score":70.5,"num_movies":2},{"actor_name":"Tom Berenger","total_oscar_score":1,"imdb_score":60.125,"num_movies":8},{"actor_name":"Peter Firth","total_oscar_score":1,"imdb_score":61.0,"num_movies":1},{"actor_name":"Jennifer Tilly","total_oscar_score":1,"imdb_score":66.0,"num_movies":2},{"actor_name":"Chris Sarandon","total_oscar_score":1,"imdb_score":74.0,"num_movies":2},{"actor_name":"Jennifer Jason Leigh","total_oscar_score":1,"imdb_score":60.5,"num_movies":2},{"actor_name":"James Cromwell","total_oscar_score":1,"imdb_score":59.0,"num_movies":1},{"actor_name":"Clive Owen","total_oscar_score":1,"imdb_score":63.0,"num_movies":8},{"actor_name":"James Caan","total_oscar_score":1,"imdb_score":78.0,"num_movies":1},{"actor_name":"Jake Gyllenhaal","total_oscar_score":1,"imdb_score":65.5882352941,"num_movies":17},{"actor_name":"Jackie Earle Haley","total_oscar_score":1,"imdb_score":55.0,"num_movies":2},{"actor_name":"Jesse Plemons","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Kate Hudson","total_oscar_score":1,"imdb_score":65.0,"num_movies":3},{"actor_name":"Josh Brolin","total_oscar_score":1,"imdb_score":64.0,"num_movies":6},{"actor_name":"Jessie Buckley","total_oscar_score":1,"imdb_score":63.0,"num_movies":1},{"actor_name":"John C. Reilly","total_oscar_score":1,"imdb_score":69.75,"num_movies":4},{"actor_name":"Chazz Palminteri","total_oscar_score":1,"imdb_score":57.0,"num_movies":1},{"actor_name":"Gene Wilder","total_oscar_score":1,"imdb_score":70.75,"num_movies":4},{"actor_name":"Gary Sinise","total_oscar_score":1,"imdb_score":60.0,"num_movies":1},{"actor_name":"Florence Pugh","total_oscar_score":1,"imdb_score":68.6,"num_movies":5},{"actor_name":"Dennis Hopper","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Emily Blunt","total_oscar_score":1,"imdb_score":68.0,"num_movies":5},{"actor_name":"Elliott Gould","total_oscar_score":1,"imdb_score":62.0,"num_movies":2},{"actor_name":"Eddie Murphy","total_oscar_score":1,"imdb_score":59.95,"num_movies":20},{"actor_name":"Dev Patel","total_oscar_score":1,"imdb_score":73.0,"num_movies":5},{"actor_name":"Hume Cronyn","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"Hailee Steinfeld","total_oscar_score":1,"imdb_score":69.5,"num_movies":2},{"actor_name":"Dan Aykroyd","total_oscar_score":1,"imdb_score":61.5,"num_movies":8},{"actor_name":"Harvey Keitel","total_oscar_score":1,"imdb_score":72.3333333333,"num_movies":3},{"actor_name":"Haley Joel Osment","total_oscar_score":1,"imdb_score":64.5,"num_movies":2},{"actor_name":"Hal Holbrook","total_oscar_score":1,"imdb_score":69.0,"num_movies":1},{"actor_name":"Greg Kinnear","total_oscar_score":1,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"Danny Aiello","total_oscar_score":1,"imdb_score":78.0,"num_movies":1},{"actor_name":"Brad Dourif","total_oscar_score":1,"imdb_score":54.0,"num_movies":1},{"actor_name":"Matt Dillon","total_oscar_score":1,"imdb_score":66.0,"num_movies":3},{"actor_name":"Mary J. Blige","total_oscar_score":1,"imdb_score":63.0,"num_movies":1},{"actor_name":"Naomie Harris","total_oscar_score":1,"imdb_score":68.0,"num_movies":1},{"actor_name":"Barbara Hershey","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Omar Sharif","total_oscar_score":1,"imdb_score":76.0,"num_movies":1},{"actor_name":"Lesley Manville","total_oscar_score":1,"imdb_score":73.0,"num_movies":1},{"actor_name":"Lily Tomlin","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Lauren Bacall","total_oscar_score":1,"imdb_score":69.0,"num_movies":1},{"actor_name":"Lakeith Stanfield","total_oscar_score":1,"imdb_score":71.0,"num_movies":2},{"actor_name":"Kodi Smit-McPhee","total_oscar_score":1,"imdb_score":64.0,"num_movies":4},{"actor_name":"Klaus Maria Brandauer","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"Kirsten Dunst","total_oscar_score":1,"imdb_score":63.0,"num_movies":8},{"actor_name":"Ken Watanabe","total_oscar_score":1,"imdb_score":75.0,"num_movies":1},{"actor_name":"Lillian Gish","total_oscar_score":1,"imdb_score":61.0,"num_movies":2},{"actor_name":"Linda Blair","total_oscar_score":1,"imdb_score":51.5,"num_movies":2},{"actor_name":"Mark Wahlberg","total_oscar_score":1,"imdb_score":64.9285714286,"num_movies":28},{"actor_name":"Marianne Jean-Baptiste","total_oscar_score":1,"imdb_score":57.0,"num_movies":1},{"actor_name":"Maggie Gyllenhaal","total_oscar_score":1,"imdb_score":67.5,"num_movies":2},{"actor_name":"Burt Reynolds","total_oscar_score":1,"imdb_score":63.0,"num_movies":5},{"actor_name":"Abigail Breslin","total_oscar_score":1,"imdb_score":61.0,"num_movies":4}];

// Filter out actors with null IMDb scores
const scatterData = imdbData
  .filter(item => item.imdb_score !== null && item.imdb_score !== undefined)
  .map(item => ({
    x: item.imdb_score,  // Swap x and y values
    y: item.total_oscar_score,  // Swap x and y values
    label: item.actor_name
  }));

// Calculate the regression line
const calculateRegression = (data) => {
  const n = data.length;
  const sumX = data.reduce((sum, point) => sum + point.x, 0);
  const sumY = data.reduce((sum, point) => sum + point.y, 0);
  const sumXY = data.reduce((sum, point) => sum + point.x * point.y, 0);
  const sumX2 = data.reduce((sum, point) => sum + point.x * point.x, 0);

  // Calculate slope (m) and intercept (b)
  const m = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
  const b = (sumY - m * sumX) / n;

  return { m, b };
};

// Calculate the regression line points
const regression = calculateRegression(scatterData);
const regressionLine = scatterData.map(point => ({
  x: point.x,
  y: regression.m * point.x + regression.b
}));

// Create the chart
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
        }, {
            label: 'Regression Line',
            data: regressionLine,
            type: 'line',
            borderColor: 'rgba(255, 99, 132, 1)',
            borderWidth: 2,
            fill: false
        }]
    },
    options: {
        responsive: true,
        plugins: {
            tooltip: {
                callbacks: {
                    label: function(context) {
                        const dataPoint = context.raw;
                        return `${dataPoint.label}: IMDb Score = ${dataPoint.x}, Oscar Score = ${dataPoint.y}`;
                    }
                }
            },
            title: {
                display: true,
                text: 'IMDb Score vs Oscar Score'
            },
            legend: {
                display: true // Show the legend for both datasets
            }
        },
        scales: {
            x: {
                title: { display: true, text: 'IMDb Score' },
                beginAtZero: false
            },
            y: {
                title: { display: true, text: 'Oscar Score' },
                beginAtZero: true, // Ensure the y-axis starts at 0
                min: 0,  // Optional: Ensure the minimum is 0
                suggestedMax: 100 
            }
        }
    }
});

</script>



The plot of IMDb scores versus Oscar scores shows a slight correlation between the two, but the relationship is not strong, as indicated by the regression line. While some actors with higher Oscar scores tend to have higher IMDb scores, this pattern is not consistent across all data points.

The reason for this weak correlation lies in the fact that the two scores measure different aspects of an actor's career. Oscar scores are based on professional recognition for outstanding performances, typically awarded by industry experts. In contrast, IMDb scores reflect the general public's opinion of movies and performances, meaning they are influenced by a broader range of factors and biases.

Therefore, the Oscar Score does not strongly correlate with audience ratings, and we will explore other potential factors influencing success below.

<br>
<!-- BUDGET -->

<h1> Budget 💰 </h1>

Does a higher budget imply better revenue, ratings, and overall movie success? At first glance, it seems logical—like adding more ingredients to a chemical formula to achieve a better reaction. However, as revealed in our second milestone, the relationship between financial investment and movie performance is far more complex. To untangle this, we use two tools from our statistical lab: *Pearson and Spearman correlations*, which allow us to examine both linear trends and rank-based patterns.

What do these tools measure?

- Pearson Correlation tests the linear relationship between two variables—like how two chemicals react in perfect proportion.
- Spearman Correlation looks at the rank of the data, revealing patterns even when the relationship isn’t perfectly straight, making it robust against outliers or unexpected reactions.


By testing both, we uncover not just visible trends but hidden relationships, helping us determine whether budget truly acts as a key catalyst for a movie’s success. More speciically, if Spearman is higher than Pearson, it indicates a stronger rank-based relationship, suggesting subtle, non-linear patterns. Conversely, if Pearson is higher, the variables exhibit a clearer linear trend.


<h2> Budget vs Revenue </h2>

- Pearson Correlation: 0.769
- Spearman Correlation: 0.71


The results are clear! Budget and revenue show a strong positive relationship, much like increasing the concentration of a reagent leading to a stronger chemical yield. A higher budget enables grander production, wider distribution, and aggressive marketing—ingredients that often boost box office earnings.


<p style="font-weight: 700"> But here’s the catch:</p> 

Revenue is not profit. Just as an expensive experiment might fail to yield a breakthrough, movies with massive budgets can still flop if audience reception or competition acts as a limiting factor.

<h2> Budget vs Average Movie Score </h2>

- Pearson Correlation: 0.097
- Spearman Correlation: 0.15

The weak correlations suggest that budget has minimal influence on movie quality, much like adding more expensive compounds without improving the end result. A big budget might produce spectacle, but it doesn’t necessarily generate a masterpiece. It’s like a flashy formula that works but doesn’t impress the critics—expensive, but ineffective at winning accolades. 

<h2> Revenue vs Average Movie Score</h2>

- Pearson Correlation: 0.112
- Spearman Correlation: 0.21

Here, we observe a faint positive relationship—like a slight chemical reaction occurring under ideal conditions. Successful movies in terms of revenue sometimes rank higher in scores, but the relationship is weak. This suggests that external factors, such as hype, franchises, or marketing, often drive revenue more than the movie’s artistic or critical merit.

<h2> Box Office vs Average Movie Score</h2>

- Pearson Correlation: 0.183
- Spearman Correlation: 0.12 

The results show a mild connection... box office performance and movie scores occasionally align, but not reliably. It’s like a formula producing small sparks of success—a positive reaction, but not a breakthrough. While higher scores can nudge box office earnings upward, other variables, like star power and audience trends, dominate the equation.

Success, much like in the lab, is not defined by a single variable. A blockbuster might generate impressive box office "yields" without winning critical acclaim, while a critically adored film may struggle financially. True success lies in understanding this multi-dimensional formula—balancing financial outcomes, audience reactions, and artistic merit. For this reason, let us continue our analysis even further...

<br>


<!-- TIMING -->
<h1> Release Date Timing ⏰ </h1>

The timing of a movie's release plays a crucial role in its success. Do films released during specific periods, such as summer or the winter holidays, tend to perform better? Let's explore the data below!

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>

Overall, our data shows that more movies are released in the fall than in any other season

<div id="movies_count_graph" style="height: 400px;"></div>

Sheer quantity isn't everything.  Let us see how well people and professionals like the movies in each season.

<div id="audience_timing" style="height: 400px;"></div>

The audience perception surpringly does not change across seasons! And across all the most popular critique sites like IMDb, TMDb, Rotten Tomato, and Metacritic. But what about revenue and box office?

<div id="revenue_graph" style="height: 400px;"></div>

<div id="box_office_graph" style="height: 400px;"></div>

Oh, the blockbuster effect !  Movie studios often release their most anticipated films during the summer, when school is out and families are more likely to go to the theater. This results in higher audience turnout and larger revenue.

Spring also sees a fair amount of success due to less competition, as some studios release films earlier in the year to capture the attention of moviegoers before the summer rush. 

Winter sees a decline in revenue, likely because audiences are more focused on the holidays and other festive activities, and fewer major releases are scheduled. 

Fall typically has the lowest revenue as movie releases tend to be fewer, with studios holding back major films for the end of the year to align with award seasons. 

Now, let’s break it down even further. How does movie revenue fluctuate week by week throughout the year. I would like to call that "the Christmas peak".

<div id="weekly_revenue_graph" style="height: 400px;"></div>

Wow, if we break down the movie revenue by week, we can really see how major holidays cause those big spikes. For instance,

-  around week 8 (February), we see a rise, probably thanks to Valentine’s Day and all the romance-themed movies. 
- in week 30 (late July), there's another boost with summer break and more people heading to the theaters.
-  a dip in week 35 when school starts up again.
- week 44 sees a peak around Halloween, with horror and thriller movies doing well.  
- 46–48, there's a slight uptick for Thanksgiving as families gather, and of course, a huge jump in December with Christmas and New Year’s. 

So, while summer is usually the top performer, these holiday spikes really show how releasing movies around specific times can tap into different audiences.

<script>
  var seasons = ['Fall', 'Spring', 'Summer','Winter'];

  var movies_count = [11648, 10270, 10007, 8876];
  var avg_scores = [57.635928, 57.439815, 57.000971, 57.611792];
  var revenue = [1.155038e+07, 1.485746e+07, 2.119703e+07, 1.383414e+07];
  var box_office = [3.837366e+07, 4.630044e+07, 6.473062e+07, 3.955709e+07];

  // Data for Weekly Movie Revenue
  var weeks = Array.from({ length: 53 }, (_, i) => i + 1);
  var weekly_revenue = [
    3.488478e+06, 7.168681e+06, 5.735429e+06, 5.872685e+06, 7.245216e+06,
    1.842523e+07, 1.038769e+07, 5.135188e+06, 1.438110e+07, 1.049354e+07,
    1.474977e+07, 1.038972e+07, 1.120941e+07, 1.094972e+07, 1.473026e+07,
    9.257742e+06, 1.256392e+07, 1.106167e+07, 1.584672e+07, 2.161837e+07,
    3.448425e+07, 1.049265e+07, 3.936867e+07, 2.699497e+07, 3.769338e+07,
    2.277869e+07, 3.147838e+07, 2.364209e+07, 2.705743e+07, 2.328511e+07,
    1.694422e+07, 1.285691e+07, 8.540282e+06, 5.838918e+06, 1.073097e+07,
    7.405663e+06, 6.518756e+06, 9.832472e+06, 7.179045e+06, 6.890894e+06,
    6.845307e+06, 9.457242e+06, 1.604099e+07, 1.487406e+07, 1.188627e+07,
    2.514698e+07, 2.168965e+07, 1.220306e+07, 2.251107e+07, 4.199028e+07,
    1.941219e+07, 1.115324e+07, 1.264843e+06
  ];

  // Number of Movies in Each Season
var movies_count_trace = {
    x: seasons,
    y: movies_count,
    type: 'bar',
    marker: { color: '#8F00FF' },
};


  // Average Movie Score for Each Season
var revenue_trace = {
    x: seasons,
    y: revenue,
    type: 'bar',
    marker: { color: '#FF7300' },  
};

var audience_trace = {
    x: seasons,
    y: avg_scores,
    type: 'bar',
    marker: { color: '#blue' },  
};


  // Average Box Office for Each Season
  var box_office_trace = {
    x: seasons,
    y: box_office,
    type: 'bar',
    marker: { color: '#39FF14' },
  };

  // Weekly Movie Revenue Line Plot
  var weekly_revenue_trace = {
    x: weeks,
    y: weekly_revenue,
    mode: 'lines+markers',
    line: { color: '#CC5500', shape: 'spline' },
    name: 'Weekly Revenue',
  };

  // Layout for each graph
  var movies_count_layout = {
    title: 'Number of Movies Per Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Number of Movies' },
  };

  var revenue_layout = {
    title: 'Revenue Per Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Revenue (in USD)' },
  };

  var box_office_layout = {
    title: 'Average Box Office Per Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Box Office (in USD)' },
  };

  var weekly_revenue_layout = {
    title: 'Weekly Movie Revenue',
    xaxis: { title: 'Week of the Year' },
    yaxis: { title: 'Average Revenue (in USD)' },
  };

  var aud_time_layout = {
    title: 'Average Movie Score per Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Aeverage Movie Score' },
  };


  // Create the plots with specific layouts
  Plotly.newPlot('movies_count_graph', [movies_count_trace], movies_count_layout);
  Plotly.newPlot('revenue_graph', [revenue_trace], revenue_layout);
  Plotly.newPlot('box_office_graph', [box_office_trace], box_office_layout);
  Plotly.newPlot('weekly_revenue_graph', [weekly_revenue_trace], weekly_revenue_layout);
  Plotly.newPlot('audience_timing', [audience_trace], aud_time_layout);

</script>
<br>
<!-- COUNTRY -->
<h1> Production Country 🌏</h1>

Know that we know that budget, oscar, and movie timing arent'the only factors that contribute to a movie's success, let us take a look at the global landscape of the movie industry. 

<canvas id="countryBarChart" width="400" height="200"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
const ctxCountry = document.getElementById('countryBarChart').getContext('2d');
const countryBarChart = new Chart(ctxCountry, {
    type: 'bar',
    data: {
        labels: [
            'United States of America', 'India', 'United Kingdom', 
            'France', 'Italy', 'Japan', 'Canada', 'Germany', 
            'Argentina', 'Hong Kong', 'Australia', 'Spain', 
            'South Korea', 'Mexico', 'Netherlands', 'Sweden', 
            'China', 'West Germany', 'Denmark', 'Soviet Union'
        ],
        datasets: [{
            label: 'Number of Movies',
            data: [
                35770, 8491, 8195, 4557, 3229, 2768, 2593, 2519, 
                1470, 1264, 1166, 1160, 909, 879, 850, 675, 669, 
                662, 641, 580
            ],
            backgroundColor: 'rgba(54, 162, 235, 0.6)',
            borderColor: 'rgba(54, 162, 235, 1)',
            borderWidth: 1
        }]
    },
    options: {
        responsive: true,
        plugins: {
            legend: {
                display: false,
                position: 'top'
            },
            tooltip: {
                enabled: true
            },
            title: {
                display: true,
                text: 'Top 20 Countries by Number of Movies Produced'
            }
        },
        scales: {
            x: {
                title: {
                    display: true,
                    text: 'Country'
                }
            },
            y: {
                title: {
                    display: true,
                    text: 'Number of Movies'
                },
                beginAtZero: true
            }
        }
    }
});
</script>


These results can be explained by the unique strengths and historical factors that shape each country’s film industry, almost like the specific conditions in a lab experiment that determine the outcome:

- Hollywood is the powerhouse of global cinema, driven by its extensive infrastructure, immense funding, and deep ties to international markets. Thus, the U.S.A. films a dominant force in international box offices.

- Bollywood thrives on the vast, culturally diverse audience of India. With a focus on genres like musicals and romantic dramas, Bollywood taps into the unique tastes of its domestic market. The sheer demand from its massive population fuels this powerhouse, making Bollywood a global contender.

- United Kingdom, France, and Italy: These countries have long-standing film traditions, backed by government support and policies that protect and promote local cinema. France, for example, with its historic roots in the early days of cinema, provides an environment where films are carefully nurtured to achieve both high quality and international recognition. 


<canvas id="boxOfficeBarChart" width="400" height="200"></canvas>

<script>
const ctx2 = document.getElementById('boxOfficeBarChart').getContext('2d');
const boxOfficeBarChart = new Chart(ctx2, {
    type: 'bar',
    data: {
        labels: [
            'United States of America', 'United Kingdom', 'Germany', 
            'France', 'Canada', 'Australia', 'New Zealand', 
            'Japan', 'Italy', 'Czech Republic', 'Hong Kong', 
            'China', 'Spain', 'South Korea', 'Bahamas', 
            'Ireland', 'India', 'South Africa', 'Sweden', 'Mexico'
        ],
        datasets: [{
            label: 'Box Office Revenue (USD)',
            data: [
                4.956604e+11, 7.266183e+10, 4.231105e+10, 
                2.043164e+10, 1.664309e+10, 1.472764e+10, 
                1.185262e+10, 1.128609e+10, 4.715304e+09, 
                4.625749e+09, 4.471787e+09, 4.413012e+09, 
                3.781186e+09, 3.117088e+09, 2.385600e+09, 
                1.559834e+09, 1.421484e+09, 1.021853e+09, 
                1.000060e+09, 9.831357e+08
            ],
            backgroundColor: 'rgba(75, 192, 192, 0.6)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
        }]
    },
    options: {
        responsive: true,
        plugins: {
            legend: {
                display: false,
                position: 'top'
            },
            tooltip: {
                enabled: true
            },
            title: {
                display: true,
                text: 'Top 20 Countries by Revenue'
            }
        },
        scales: {
            x: {
                title: {
                    display: true,
                    text: 'Country'
                }
            },
            y: {
                title: {
                    display: true,
                    text: 'Revenue (USD)'
                },
                beginAtZero: true
            }
        }
    }
});
</script>

Alas not every experiment can yield perfect results. It’s important to note that the CMU Movies dataset contains a significant amount of missing data, particularly in the box office revenue field.  As a result, we observe a trend that largely reflects the movie count by country, with the USA and UK occupying the top spots. However, a key anomaly emerges in the form of Bollywood’s financial contribution, which is noticeably absent. 


<canvas id="languageBarChart" width="400" height="200"></canvas>

<script>
const ctx3 = document.getElementById('languageBarChart').getContext('2d');
const languageBarChart = new Chart(ctx3, {
    type: 'bar',
    data: {
        labels: [
            'English Language', 'Spanish Language', 'Hindi Language',
            'French Language', 'Silent film', 'Italian Language', 
            'Japanese Language', 'German Language', 'Tamil Language', 
            'Malayalam Language', 'Standard Mandarin', 'Telugu language', 
            'Russian Language', 'Cantonese', 'Korean Language', 'Dutch Language',
            'Swedish Language', 'Arabic Language', 'Czech Language', 
            'Standard Cantonese'
        ],
        datasets: [{
            label: 'Number of Movies',
            data: [
                42492, 3778, 3771, 3616, 3255, 2590, 2360, 2358, 1920, 
                1467, 1247, 1210, 1064, 891, 796, 609, 520, 517, 487, 482
            ],
            backgroundColor: 'rgba(153, 102, 255, 0.6)',
            borderColor: 'rgba(153, 102, 255, 1)',
            borderWidth: 1
        }]
    },
    options: {
        responsive: true,
        plugins: {
            legend: {
                display: false,
                position: 'top'
            },
            tooltip: {
                enabled: true
            },
            title: {
                display: true,
                text: 'Top 20 Languages by Number of Movies'
            }
        },
        scales: {
            x: {
                title: {
                    display: true,
                    text: 'Language'
                }
            },
            y: {
                title: {
                    display: true,
                    text: 'Number of Movies'
                },
                beginAtZero: true
            }
        }
    }
});
</script>


English takes a commanding lead in this dataset for two primary reasons. First, American films make up the largest portion of the dataset, which naturally skews the language distribution toward English. 

Second, English is widely recognized as the international language, spoken by large portions of the global population. This global reach amplifies the presence of English-language films, especially in international markets. Similarly, languages like French, Spanish, and Hindi also rank highly due to their widespread use and cultural significance. These languages are not only spoken by millions but also have a strong presence in global media, which boosts their films' potential for success both locally and abroad.

<canvas id="genreBarChart" width="400" height="200"></canvas>

<script>
const ctx4 = document.getElementById('genreBarChart').getContext('2d');
const genreBarChart = new Chart(ctx4, {
    type: 'bar',
    data: {
        labels: [
            'Drama', 'Comedy', 'Romance Film', 'Black-and-white', 'Thriller', 
            'Action', 'Short Film', 'World cinema', 'Crime Fiction', 'Indie', 
            'Documentary', 'Horror', 'Silent film', 'Adventure', 'Family Film', 
            'Action/Adventure', 'Comedy film', 'Musical', 'Animation', 
            'Romantic drama'
        ],
        datasets: [{
            label: 'Number of Movies',
            data: [
                35116, 16715, 10684, 9405, 9227, 9121, 8256, 7397, 7186, 
                7107, 5748, 5592, 5352, 5285, 4971, 4832, 4406, 4401, 3793, 
                3506
            ],
            backgroundColor: 'rgba(255, 159, 64, 0.6)',
            borderColor: 'rgba(255, 159, 64, 1)',
            borderWidth: 1
        }]
    },
    options: {
        responsive: true,
        plugins: {
            legend: {
                display: false,
                position: 'top'
            },
            tooltip: {
                enabled: true
            },
            title: {
                display: true,
                text: 'Top 20 Genres by Number of Movies'
            }
        },
        scales: {
            x: {
                title: {
                    display: true,
                    text: 'Genre'
                }
            },
            y: {
                title: {
                    display: true,
                    text: 'Number of Movies'
                },
                beginAtZero: true
            }
        }
    }
});
</script>


<!-- Table for Country and Genre Data -->
<h2>Top 20 Countries and their Primary Movie Genre</h2>
<table border="1" cellpadding="5" cellspacing="0" style="border-collapse: collapse; border: 1.5px solid gray;">
    <thead>
        <tr>
            <th>Country</th>
            <th>Primary Genre</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>United States of America</td>
            <td>Comedy</td>
        </tr>
        <tr>
            <td>India</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>United Kingdom</td>
            <td>Comedy</td>
        </tr>
        <tr>
            <td>France</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Italy</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Japan</td>
            <td>Drama</td>
        </tr>
        <tr>
            <td>Canada</td>
            <td>Thriller</td>
        </tr>
        <tr>
            <td>Germany</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Argentina</td>
            <td>Drama</td>
        </tr>
        <tr>
            <td>Hong Kong</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Australia</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Spain</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>South Korea</td>
            <td>World cinema</td>
        </tr>
        <tr>
            <td>Mexico</td>
            <td>Comedy film</td>
        </tr>
        <tr>
            <td>Netherlands</td>
            <td>Comedy</td>
        </tr>
        <tr>
            <td>Sweden</td>
            <td>World</td>
        </tr>
    </tbody>
</table>


Drama, leading with the highest count, makes perfect sense. It’s a genre with unmatched versatility, covering a broad spectrum of themes that attract diverse audiences. Whether it’s a small indie gem or a high-budget blockbuster, dramas have a timeless appeal that spans cultures and interests.

Comedy follows closely behind, a genre beloved for its universal charm. Humor knows no boundaries, making comedy films a favorite across cultures and languages. It’s a genre that works just as well in mainstream cinema as it does in independent films, offering something for everyone.

Both World Cinema and Romance films make their mark, underscoring the global hunger for movies that explore diverse cultures and universal emotions like love. These genres never lose their allure, as audiences around the world continue to be captivated by stories that speak to the heart.


Thus, from this, we can see that successful movies typically emerge from countries with well-established film industries, such as the United States, India, and France, where the infrastructure and global reach act as catalysts for success. These films are often in widely spoken languages, particularly English, and belong to universally popular genres like drama, comedy, and romance, which serve as the key ingredients for broad audience appeal.

<br>


<!-- TROPES -->
<h1>TV Tropes 📺</h1>
To maximize your chances of creating a successful movie, it's worth looking at what kind of characters should appear in a movie. But how can one compactly describe a character? There is so many options! Thankfully, the dataset **TV tropes** provides just what we need. 

<h2> What is success?</h2>
Before performing the analysis, we must establish what the ideal success metric is. You might want to aim for a movie which makes a lot of money or a movie which is rated well by the viewers, or perhaps a movie well rated by the critics? Ideally all at the same time, but we need to see if there is an overlap first. Therefore, our chosen success metrics are:

1. Box Office Revenue
2. IMDb Rating 
3. Metacritic Score

<h2> Box Office Revenue Perspective</h2>
Since the TV tropes dataset contains a mapping from each trope cluster to a set of characters, we can look at the Box Office Revenue of the corresponding movie and identify the ranking of the tropes.

The below plot demonstrates the average Box Office Revenue of each trope
<div id="allTropesBoxOfficeRevenuePlot" style="width:100%; max-width:700px; height:500px;"></div>

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
  var rawData = [{'trope': 'gadgeteer_genius', 'success_metric': 647179205.4444444}, {'trope': 'grumpy_old_man', 'success_metric': 539195924.6666666}, {'trope': 'charmer', 'success_metric': 530069451.90909094}, {'trope': 'child_prodigy', 'success_metric': 519998348.0}, {'trope': 'bruiser_with_a_soft_center', 'success_metric': 491920000.0}, {'trope': 'adventurer_archaeologist', 'success_metric': 468307217.4}, {'trope': 'broken_bird', 'success_metric': 419649591.0}, {'trope': 'egomaniac_hunter', 'success_metric': 414209401.12903225}, {'trope': 'doormat', 'success_metric': 373276774.4}, {'trope': 'trickster', 'success_metric': 362858355.6666667}, {'trope': 'pupil_turned_to_evil', 'success_metric': 347447431.0}, {'trope': 'jerk_jock', 'success_metric': 328653453.90625}, {'trope': 'byronic_hero', 'success_metric': 325700482.04761904}, {'trope': 'heartbroken_badass', 'success_metric': 323444962.4285714}, {'trope': 'crazy_survivalist', 'success_metric': 315947885.0}, {'trope': 'loveable_rogue', 'success_metric': 298233643.14285713}, {'trope': 'cultured_badass', 'success_metric': 293522889.3636364}, {'trope': 'master_swordsman', 'success_metric': 291984626.1764706}, {'trope': 'romantic_runnerup', 'success_metric': 282056295.75}, {'trope': 'eccentric_mentor', 'success_metric': 281563895.1666667}, {'trope': 'coward', 'success_metric': 251913361.42857143}, {'trope': 'bully', 'success_metric': 248325981.0}, {'trope': 'arrogant_kungfu_guy', 'success_metric': 239406978.45454547}, {'trope': 'warrior_poet', 'success_metric': 216645852.0}, {'trope': 'consummate_professional', 'success_metric': 209398228.5}, {'trope': 'junkie_prophet', 'success_metric': 203590570.5}, {'trope': 'gentleman_thief', 'success_metric': 202018815.5}, {'trope': 'casanova', 'success_metric': 191592846.625}, {'trope': 'corrupt_corporate_executive', 'success_metric': 180529639.0}, {'trope': 'father_to_his_men', 'success_metric': 179369424.3125}, {'trope': 'chanteuse', 'success_metric': 177277937.1}, {'trope': 'the_chief', 'success_metric': 170395945.0}, {'trope': 'playful_hacker', 'success_metric': 163146946.0}, {'trope': 'fastest_gun_in_the_west', 'success_metric': 160984130.4}, {'trope': 'dean_bitterman', 'success_metric': 157870668.75}, {'trope': 'henpecked_husband', 'success_metric': 151848917.4}, {'trope': 'evil_prince', 'success_metric': 144823493.0}, {'trope': 'hardboiled_detective', 'success_metric': 144395545.0}, {'trope': 'ditz', 'success_metric': 126584388.125}, {'trope': 'psycho_for_hire', 'success_metric': 111343329.875}, {'trope': 'retired_outlaw', 'success_metric': 111058424.83333333}, {'trope': 'officer_and_a_gentleman', 'success_metric': 110681820.5}, {'trope': 'hitman_with_a_heart', 'success_metric': 107333666.8888889}, {'trope': 'stupid_crooks', 'success_metric': 105042870.6}, {'trope': 'storyteller', 'success_metric': 103692910.75}, {'trope': 'morally_bankrupt_banker', 'success_metric': 98430091.5}, {'trope': 'crazy_jealous_guy', 'success_metric': 98010318.86956522}, {'trope': 'absent_minded_professor', 'success_metric': 92579135.8}, {'trope': 'tranquil_fury', 'success_metric': 91995016.36363636}, {'trope': 'dirty_cop', 'success_metric': 90979714.33333333}, {'trope': 'final_girl', 'success_metric': 86648773.70588236}, {'trope': 'drill_sargeant_nasty', 'success_metric': 82004147.6}, {'trope': 'klutz', 'success_metric': 81943981.0}, {'trope': 'young_gun', 'success_metric': 78026562.25}, {'trope': 'loser_protagonist', 'success_metric': 74395358.0}, {'trope': 'the_editor', 'success_metric': 70600000.0}, {'trope': 'slacker', 'success_metric': 65943944.72727273}, {'trope': 'surfer_dude', 'success_metric': 65428949.6}, {'trope': 'bounty_hunter', 'success_metric': 62675618.375}, {'trope': 'valley_girl', 'success_metric': 56005675.28571428}, {'trope': 'ophelia', 'success_metric': 53974270.0}, {'trope': 'stoner', 'success_metric': 51767929.461538464}, {'trope': 'granola_person', 'success_metric': 47637333.25}, {'trope': 'dumb_muscle', 'success_metric': 46205587.71428572}, {'trope': 'dumb_blonde', 'success_metric': 45809676.333333336}, {'trope': 'brainless_beauty', 'success_metric': 42365757.90909091}, {'trope': 'big_man_on_campus', 'success_metric': 39926288.166666664}, {'trope': 'revenge', 'success_metric': 37064736.875}, {'trope': 'prima_donna', 'success_metric': 36563647.125}, {'trope': 'bromantic_foil', 'success_metric': 17392920.5}, {'trope': 'self_made_man', 'success_metric': 10244990.4}, {'trope': 'classy_cat_burglar', 'success_metric': 4500000.0}];

  var top5xData = rawData.slice(0, 5).map(entry => entry.trope);
  var top5yData = rawData.slice(0, 5).map(entry => entry.success_metric);

  var bottom5xData = rawData.slice(-5).map(entry => entry.trope);
  var bottom5yData = rawData.slice(-5).map(entry => entry.success_metric);

  var xData = top5xData.concat(bottom5xData);
  var yData = top5yData.concat(bottom5yData);

  // Create the plot
var data = [
  {
    x: xData,
    y: yData,
    type: 'bar',
    textposition: 'none',  // Remove text on bars
    marker: {
      color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
      line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
    },
    showlegend: false
  },
  // Dummy trace for the top 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(55, 128, 191, 0.7)' },
    name: 'Top 5' // Legend label
  },
  // Dummy trace for the bottom 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(219, 64, 82, 0.7)' },
    name: 'Bottom 5' // Legend label
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
    b: 150, // Increase bottom margin to give more space for x-axis labels
    l: 80,  // Increase left margin to give space for y-axis label
    t: 80,  // Add some space at the top
    r: 50   // Add space on the right for better alignment
  },
  width: 900,  // Set the width of the plot
  height: 500, // Set the height of the plot
  template: 'plotly_white',
  legend: { 
    orientation: 'v', 
    x: 1, 
    y: 1, 
    xanchor: 'right', 
    yanchor: 'top' 
  } 
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
    y: yData.map(entry => (entry).toFixed(2)),
    type: 'bar',
    textposition: 'auto', // Keep the text position on the bars
    marker: {
      color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
      line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
    },
    showlegend: false,
  },
  // Dummy trace for the top 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(55, 128, 191, 0.7)' },
    name: 'Top 5' // Legend label
  },
  // Dummy trace for the bottom 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(219, 64, 82, 0.7)' },
    name: 'Bottom 5' // Legend label
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
    b: 150,  // Increase bottom margin to give more space for x-axis labels
    l: 80,   // Increase left margin for y-axis label space
    t: 80,   // Add space at the top for better title placement
    r: 50    // Add space on the right for alignment
  },
  width: 900,   // Set the width of the plot
  height: 500,  // Set the height of the plot
  template: 'plotly_white',
  legend: { 
    orientation: 'v', 
    x: 1, 
    y: 1, 
    xanchor: 'right', 
    yanchor: 'top' 
  }
};

// Render the plot
Plotly.newPlot('allTropesIMDbRating', data, layout);

</script>


<h2> Metacritic Score Perspective</h2>
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
    y: yData.map(entry => (entry).toFixed(2)),
    type: 'bar',
    textposition: 'auto', // Keep the text position on the bars
    marker: {
      color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 0.7)' : 'rgba(219, 64, 82, 0.7)'),
      line: { color: xData.map((_, i) => i < 5 ? 'rgba(55, 128, 191, 1.0)' : 'rgba(219, 64, 82, 1.0)'), width: 2 }
    },
    showlegend: false
  },
  // Dummy trace for the top 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(55, 128, 191, 0.7)' },
    name: 'Top 5' // Legend label
  },
  // Dummy trace for the bottom 5 legend
  {
    x: [null], // No data points
    y: [null],
    type: 'bar',
    marker: { color: 'rgba(219, 64, 82, 0.7)' },
    name: 'Bottom 5' // Legend label
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
    b: 150,  // Increase bottom margin to give more space for x-axis labels
    l: 80,   // Increase left margin to provide space for y-axis label
    t: 50,   // Add space at the top for title
    r: 50    // Add space on the right
  },
  width: 900,   // Set the width of the plot
  height: 500,  // Set the height of the plot
  template: 'plotly_white',
  legend: { 
    orientation: 'v', 
    x: 1, 
    y: 1, 
    xanchor: 'right', 
    yanchor: 'top' 
  }
};

// Render the plot
Plotly.newPlot('allTropesMetacriticRating', data, layout);

</script>

<h2> Which trope is the best?</h2>
Now that we have visualized which tropes score best in each category, let us propose which trope could be the best predictor for a successful movie! Firstly, we make an observation that there is no overlap between the top 5 tropes ranked by Box Office revenue and the IMDb top 5. Similarly, no overlap between Box Office top 5 and Metacritic top 5. However, the Metacritic and IMDb top 5 share multiple tropes, namely: *Stupid Crooks*, *Doormat*, *Morally bankrupt banker*. Notice that all of these characters show silly or negative traits. It seems there is something people enjoy about these characteristics, perhaps they make us laugh. 

However, the Box Office top 5 include tropes such as: *Gadgeteer Genius* and *Child Prodigy*. These only ranked average in IMDb and Metacritic, but performed exceptionally well in the Box Office. Is it envy which makes us admire these characters, but only silently without ranking them well? Or are they not ranked well simply because it is a cliché? We would need a more thorough analysis to uncover the reasons, but the trend seems to point this way.

Finally, we conclude that the choice of the tropes ultimately comes down to what the movie is trying to optimize for. If it is Box Office revenue - make a movie about a genius! If audience or critics reception is the goal - a silly character which provides a good laugh could be a better choice.


<h2>Remarks about the data used</h2>
To remain objective, it is worth noting that the tropes we had available are not the best representation of the ground truth. They were only selected from around 500 successful movies and the corresponding characters were disproportionally represented by mostly men. The distribution can be clearly visible in the pie chart.

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
  template: 'plotly_white',
  width: 700,  
  height: 500, 
  margin: {
    t: 50,  
    b: 50,  
    l: 50,  
    r: 50   
  }
};

Plotly.newPlot('genderDistributionPie', pieData, pieLayout);

</script>
<br>
<h1> Regression Analysis ✏️ </h1>

<br>

When people evaluate the success of the movies, they are using they personal opinion, which extremely heterogeneous and biased. To solve that issue and identify factors, which are the secret sauce for the movies success, we used **3 success metrics**: 

1. IMDb rating
2. box office (we will call it max revenue)
3. number of oscar wins/nomination

We will do regression analysis on these 3 metrics, because each of them represents a different perspective. IMDb for audience reception, box office represents the profit from the tickets, and oscar wins/nominations representing the professional critics view.

Then we will try identify which covariates are important for movie to be considered good (IMDb rating > 8)


After combining the Oscar dataset with IMDb rating and financial performance of the movie, we perform correlation analysis to select relevant feature and drop over-correlated features from analysis. We encode the categorical feature and standardize the numerical once. 

Below plot demonstrates the correlation matrix between considered features:

<div id="correlation-heatmap"></div>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script>
    var zValues = [[ 1.00000000e+00,  2.09713557e-01,  2.60701722e-01,
         6.77053573e-02,  1.00307697e-01, -2.12153107e-03,
         2.52941159e-03, -2.45106024e-03, -4.67584855e-03,
         1.40228163e-02, -3.69616804e-03, -8.00560318e-03,
         2.65330846e-03,  1.56827720e-02,  1.60456088e-03,
        -4.14500311e-03,  1.93850604e-02, -2.17428655e-01,
        -2.97954426e-01, -1.78698156e-01, -1.26120270e-01,
         1.95350254e-01],
       [ 2.09713557e-01,  1.00000000e+00,  2.63083384e-01,
         5.15743891e-02,  9.81835756e-02, -4.59468725e-02,
         1.00814144e-02,  7.24568910e-03,  8.96531763e-03,
         5.81554989e-02,  1.06563354e-01,  1.01762019e-01,
        -5.03973086e-03, -1.06947975e-02, -7.46283851e-03,
         8.76981488e-02,  8.18665409e-02, -8.63002963e-02,
        -2.88039475e-02,  1.91181750e-01,  2.42480934e-01,
         7.78320448e-02],
       [ 2.60701722e-01,  2.63083384e-01,  1.00000000e+00,
        -2.94368773e-02, -6.96839555e-02,  2.01721656e-02,
        -5.36853361e-03, -1.05758539e-02,  2.29106210e-03,
         1.81643192e-02, -1.07458953e-02, -1.48351755e-02,
        -6.95355729e-03,  2.65640332e-02, -2.31483494e-03,
        -2.47190033e-02, -4.69093655e-02,  1.08027681e-01,
         3.69803817e-02, -9.14682595e-02, -9.92412665e-02,
        -1.22757813e-03],
       [ 6.77053573e-02,  5.15743891e-02, -2.94368773e-02,
         1.00000000e+00,  6.88341739e-01, -1.05298055e-02,
        -6.42695126e-03,  4.85664590e-05, -1.00161991e-02,
         6.89482866e-04,  1.11779404e-02, -1.03626662e-03,
         1.22512853e-02,  2.99177846e-04,  8.63503685e-03,
         2.07973569e-02,  4.80846741e-02, -3.48296621e-02,
        -1.88832450e-02,  7.36599216e-02,  7.27675540e-02,
         6.99999141e-02],
       [ 1.00307697e-01,  9.81835756e-02, -6.96839555e-02,
         6.88341739e-01,  1.00000000e+00, -1.99217580e-02,
        -1.40018171e-02, -6.32408576e-03, -1.50355628e-02,
         4.32567785e-03,  2.23550482e-02,  7.41292129e-03,
         8.97679072e-03,  1.11709140e-02,  1.17271634e-02,
         2.48770779e-02,  8.81641560e-02, -8.31131186e-02,
        -5.66349588e-02,  1.33283608e-01,  1.45239549e-01,
         1.17572800e-01],
       [-2.12153107e-03, -4.59468725e-02,  2.01721656e-02,
        -1.05298055e-02, -1.99217580e-02,  1.00000000e+00,
        -4.81540106e-02, -5.13061670e-02, -5.05352867e-02,
        -5.29969840e-02, -4.68684565e-02, -4.42457237e-02,
        -4.95141731e-02, -5.66771690e-02, -5.58210467e-02,
        -5.07720775e-02, -5.33912569e-02, -7.22962463e-03,
        -1.03103942e-02,  2.30546465e-02,  3.47144713e-02,
         2.20347107e-02],
       [ 2.52941159e-03,  1.00814144e-02, -5.36853361e-03,
        -6.42695126e-03, -1.40018171e-02, -4.81540106e-02,
         1.00000000e+00, -5.05730989e-02, -4.98132331e-02,
        -5.22397574e-02, -4.61987949e-02, -4.36135360e-02,
        -4.88067092e-02, -5.58673595e-02, -5.50234695e-02,
        -5.00466406e-02, -5.26283969e-02, -6.58190223e-03,
        -4.30545791e-04,  1.44668918e-02,  1.37764672e-02,
         2.25357221e-02],
       [-2.45106024e-03,  7.24568910e-03, -1.05758539e-02,
         4.85664590e-05, -6.32408576e-03, -5.13061670e-02,
        -5.05730989e-02,  1.00000000e+00, -5.30740020e-02,
        -5.56593663e-02, -4.92229631e-02, -4.64684734e-02,
        -5.20015912e-02, -5.95244308e-02, -5.86252999e-02,
        -5.33226883e-02, -5.60734461e-02, -7.02609420e-03,
         7.66863529e-03,  2.69518015e-02,  2.59547525e-02,
         2.26406121e-03],
       [-4.67584855e-03,  8.96531763e-03,  2.29106210e-03,
        -1.00161991e-02, -1.50355628e-02, -5.05352867e-02,
        -4.98132331e-02, -5.30740020e-02,  1.00000000e+00,
        -5.48230788e-02, -4.84833832e-02, -4.57702800e-02,
        -5.12202621e-02, -5.86300703e-02, -5.77444490e-02,
        -5.25215096e-02, -5.52309370e-02, -3.18533581e-03,
         2.08271161e-03,  2.48232631e-02,  2.41859177e-02,
         8.88565717e-03],
       [ 1.40228163e-02,  5.81554989e-02,  1.81643192e-02,
         6.89482866e-04,  4.32567785e-03, -5.29969840e-02,
        -5.22397574e-02, -5.56593663e-02, -5.48230788e-02,
         1.00000000e+00, -5.08451272e-02, -4.79998621e-02,
        -5.37153262e-02, -6.14860843e-02, -6.05573222e-02,
        -5.50799607e-02, -5.79213708e-02, -1.65101304e-02,
        -1.00684391e-02,  1.41422787e-02,  1.41381335e-02,
         3.43678482e-02],
       [-3.69616804e-03,  1.06563354e-01, -1.07458953e-02,
         1.11779404e-02,  2.23550482e-02, -4.68684565e-02,
        -4.61987949e-02, -4.92229631e-02, -4.84833832e-02,
        -5.08451272e-02,  1.00000000e+00, -4.24491976e-02,
        -4.75037302e-02, -5.43758842e-02, -5.35545234e-02,
        -4.87105594e-02, -5.12233913e-02, -1.03791691e-02,
        -3.17995887e-03,  3.56556430e-02,  3.60332574e-02,
         8.53924557e-03],
       [-8.00560318e-03,  1.01762019e-01, -1.48351755e-02,
        -1.03626662e-03,  7.41292129e-03, -4.42457237e-02,
        -4.36135360e-02, -4.64684734e-02, -4.57702800e-02,
        -4.79998621e-02, -4.24491976e-02,  1.00000000e+00,
        -4.48454478e-02, -5.13330399e-02, -5.05576420e-02,
        -4.59847436e-02, -4.83569587e-02, -1.65602169e-02,
        -2.08783031e-03,  2.35999249e-02,  1.88158577e-02,
        -2.36282345e-04],
       [ 2.65330846e-03, -5.03973086e-03, -6.95355729e-03,
         1.22512853e-02,  8.97679072e-03, -4.95141731e-02,
        -4.88067092e-02, -5.20015912e-02, -5.12202621e-02,
        -5.37153262e-02, -4.75037302e-02, -4.48454478e-02,
         1.00000000e+00, -5.74453939e-02, -5.65776673e-02,
        -5.14602624e-02, -5.41149432e-02, -2.28943144e-02,
        -1.48029975e-02,  1.68120731e-02,  1.97975233e-02,
         6.38527115e-03],
       [ 1.56827720e-02, -1.06947975e-02,  2.65640332e-02,
         2.99177846e-04,  1.11709140e-02, -5.66771690e-02,
        -5.58673595e-02, -5.95244308e-02, -5.86300703e-02,
        -6.14860843e-02, -5.43758842e-02, -5.13330399e-02,
        -5.74453939e-02,  1.00000000e+00, -6.47625076e-02,
        -5.89047905e-02, -6.19435121e-02, -3.71170022e-02,
        -2.94532814e-02,  2.38057895e-02,  1.69365238e-02,
         5.06539259e-02],
       [ 1.60456088e-03, -7.46283851e-03, -2.31483494e-03,
         8.63503685e-03,  1.17271634e-02, -5.58210467e-02,
        -5.50234695e-02, -5.86252999e-02, -5.77444490e-02,
        -6.05573222e-02, -5.35545234e-02, -5.05576420e-02,
        -5.65776673e-02, -6.47625076e-02,  1.00000000e+00,
        -5.80150194e-02, -6.10078405e-02, -1.14344820e-02,
        -5.53258280e-03,  1.94450385e-02,  1.77412989e-02,
         6.70770773e-03],
       [-4.14500311e-03,  8.76981488e-02, -2.47190033e-02,
         2.07973569e-02,  2.48770779e-02, -5.07720775e-02,
        -5.00466406e-02, -5.33226883e-02, -5.25215096e-02,
        -5.50799607e-02, -4.87105594e-02, -4.59847436e-02,
        -5.14602624e-02, -5.89047905e-02, -5.80150194e-02,
         1.00000000e+00, -5.54897299e-02,  8.21426498e-03,
         1.91743813e-02,  2.82079527e-02,  2.38063964e-02,
        -2.99692627e-03],
       [ 1.93850604e-02,  8.18665409e-02, -4.69093655e-02,
         4.80846741e-02,  8.81641560e-02, -5.33912569e-02,
        -5.26283969e-02, -5.60734461e-02, -5.52309370e-02,
        -5.79213708e-02, -5.12233913e-02, -4.83569587e-02,
        -5.41149432e-02, -6.19435121e-02, -6.10078405e-02,
        -5.54897299e-02,  1.00000000e+00, -2.44249330e-02,
        -7.23248317e-03,  1.91846566e-02,  1.51153098e-02,
         7.46389938e-03],
       [-2.17428655e-01, -8.63002963e-02,  1.08027681e-01,
        -3.48296621e-02, -8.31131186e-02, -7.22962463e-03,
        -6.58190223e-03, -7.02609420e-03, -3.18533581e-03,
        -1.65101304e-02, -1.03791691e-02, -1.65602169e-02,
        -2.28943144e-02, -3.71170022e-02, -1.14344820e-02,
         8.21426498e-03, -2.44249330e-02,  1.00000000e+00,
         7.40420743e-01, -1.33145229e-02, -9.98957361e-03,
        -1.74036577e-01],
       [-2.97954426e-01, -2.88039475e-02,  3.69803817e-02,
        -1.88832450e-02, -5.66349588e-02, -1.03103942e-02,
        -4.30545791e-04,  7.66863529e-03,  2.08271161e-03,
        -1.00684391e-02, -3.17995887e-03, -2.08783031e-03,
        -1.48029975e-02, -2.94532814e-02, -5.53258280e-03,
         1.91743813e-02, -7.23248317e-03,  7.40420743e-01,
         1.00000000e+00,  6.90589645e-02,  5.64972143e-02,
        -1.68215069e-01],
       [-1.78698156e-01,  1.91181750e-01, -9.14682595e-02,
         7.36599216e-02,  1.33283608e-01,  2.30546465e-02,
         1.44668918e-02,  2.69518015e-02,  2.48232631e-02,
         1.41422787e-02,  3.56556430e-02,  2.35999249e-02,
         1.68120731e-02,  2.38057895e-02,  1.94450385e-02,
         2.82079527e-02,  1.91846566e-02, -1.33145229e-02,
         6.90589645e-02,  1.00000000e+00,  6.71288779e-01,
         6.08159225e-02],
       [-1.26120270e-01,  2.42480934e-01, -9.92412665e-02,
         7.27675540e-02,  1.45239549e-01,  3.47144713e-02,
         1.37764672e-02,  2.59547525e-02,  2.41859177e-02,
         1.41381335e-02,  3.60332574e-02,  1.88158577e-02,
         1.97975233e-02,  1.69365238e-02,  1.77412989e-02,
         2.38063964e-02,  1.51153098e-02, -9.98957361e-03,
         5.64972143e-02,  6.71288779e-01,  1.00000000e+00,
         6.46362428e-02],
       [ 1.95350254e-01,  7.78320448e-02, -1.22757813e-03,
         6.99999141e-02,  1.17572800e-01,  2.20347107e-02,
         2.25357221e-02,  2.26406121e-03,  8.88565717e-03,
         3.43678482e-02,  8.53924557e-03, -2.36282345e-04,
         6.38527115e-03,  5.06539259e-02,  6.70770773e-03,
        -2.99692627e-03,  7.46389938e-03, -1.74036577e-01,
        -1.68215069e-01,  6.08159225e-02,  6.46362428e-02,
         1.00000000e+00]];
    var xValues = ['Runtime', 'budget', 'Release_Year', 'total_wins', 'total_nominations',
       'Month_1', 'Month_2', 'Month_3', 'Month_4', 'Month_5', 'Month_6',
       'Month_7', 'Month_8', 'Month_9', 'Month_10', 'Month_11', 'Month_12',
       'production_companies_frequency_encoded', 'director_frequency_encoded',
       'Language_Name_frequency_encoded', 'Country_Name_frequency_encoded',
       'Genre_Name_frequency_encoded'];
    var colorscaleValue = [
        [0, '#3D9970'],
        [1, '#001f3f']
    ];
    var yValues = xValues;
    var data = [{
        x: xValues,
        y: yValues,
        z: zValues,
        type: 'heatmap',
        colorscale: colorscaleValue,
        showscale: false
    }];
    var layout = {
        title: {
            text: 'Annotated Heatmap'
        },
        annotations: [],
        xaxis: {
            ticks: '',
            side: 'top'
        },
        yaxis: {
            ticks: '',
            ticksuffix: ' ',
            width: 1200,
            height: 1200,
            autosize: true
    }
    };
    for ( var i = 0; i < yValues.length; i++ ) {
    for ( var j = 0; j < xValues.length; j++ ) {
        var currentValue = zValues[i][j];
        if (currentValue != 0.0) {
        var textColor = 'white';
        }else{
        var textColor = 'black';
        }
        var result = {
        xref: 'x1',
        yref: 'y1',
        x: xValues[j],
        y: yValues[i],
        text: zValues[i][j].toFixed(2),
        font: {
            family: 'Arial',
            size: 12,
            color: 'rgb(50, 171, 96)'
        },
        showarrow: false,
        font: {
            color: textColor
        }
        };
        layout.annotations.push(result);
    }
    }
    Plotly.newPlot('correlation-heatmap', data, layout);
    // <!-- const corrMatrix = {
    //     z: Zvalues,
    //     x: xValues,
    //     y: xValues,
    //     type: 'heatmap',
    //     colorscale: 'RdBu',
    //     zmin: -1,
    //     zmax: 1,
    //     colorbar: {
    //         title: 'Correlation',
    //         titleside: 'right'
    //     }
    // };
    // const layout = {
    //     title: 'Correlation Matrix for Movies Encoded',
    //     width: 1000,
    //     height: 1000,
    //     xaxis: {
    //         title: 'Features',
    //         tickangle: 45
    //     },
    //     yaxis: {
    //         title: 'Features'
    //     }
    // };
    // Plotly.newPlot('correlation-heatmap', [corrMatrix], layout); -->
</script>


Our analysis reveals minimal correlation between most variables, with three notable clusters:

- Director, Writers, Producers, and Production Companies: These columns show a high correlation after frequency encoding. This is likely because renowned directors often collaborate with well-known writers, producers, and production companies.

- Language and Country: A strong correlation exists between these variables, as countries typically use their predominant language in film production.

- Budget: Demonstrates interesting around 20% correlation with Runtime, Release Year, and around 10% with total nominations and wins (might indicate that money investment might payoff in more nominations)

- To streamline our analysis, we included only a subset of these variables. The final selection was determined through experimentation to optimize relevance and impact.

<h2> Linear Regression </h2>

As mentioned above, with start with regression analysis and at first run it using box office revenue of the indicator of great movie. After running the linear regression we obtain the following table:

<h3> Box Office Revenue Regression </h3>

<table>
    <tr>
        <th>Variable</th>
        <th>Coefficient</th>
        <th>Standard Error</th>
        <th>t-value</th>
        <th>P-value</th>
        <th>95% Confidence Interval (Lower)</th>
        <th>95% Confidence Interval (Upper)</th>
    </tr>
    <tr>
        <td>Intercept</td>
        <td>-1.0891</td>
        <td>1.092</td>
        <td>-0.997</td>
        <td>0.319</td>
        <td>-3.230</td>
        <td>1.052</td>
    </tr>
    <tr>
        <td>Runtime</td>
        <td>0.0046</td>
        <td>0.015</td>
        <td>0.313</td>
        <td>0.754</td>
        <td>-0.024</td>
        <td>0.033</td>
    </tr>
    <tr>
        <td>budget</td>
        <td>0.6317</td>
        <td>0.010</td>
        <td>62.349</td>
        <td>0.000</td>
        <td>0.612</td>
        <td>0.652</td>
    </tr>
    <tr>
        <td>Release_Year</td>
        <td>0.0005</td>
        <td>0.001</td>
        <td>0.940</td>
        <td>0.347</td>
        <td>-0.001</td>
        <td>0.002</td>
    </tr>
    <tr>
        <td>total_wins</td>
        <td>0.1419</td>
        <td>0.017</td>
        <td>8.176</td>
        <td>0.000</td>
        <td>0.108</td>
        <td>0.176</td>
    </tr>
    <tr>
        <td>total_nominations</td>
        <td>0.0492</td>
        <td>0.007</td>
        <td>7.224</td>
        <td>0.000</td>
        <td>0.036</td>
        <td>0.063</td>
    </tr>
    <tr>
        <td>Month_1</td>
        <td>0.0155</td>
        <td>0.042</td>
        <td>0.365</td>
        <td>0.715</td>
        <td>-0.067</td>
        <td>0.098</td>
    </tr>
    <tr>
        <td>Month_2</td>
        <td>-0.0207</td>
        <td>0.042</td>
        <td>-0.488</td>
        <td>0.626</td>
        <td>-0.104</td>
        <td>0.062</td>
    </tr>
    <tr>
        <td>Month_3</td>
        <td>0.0157</td>
        <td>0.042</td>
        <td>0.378</td>
        <td>0.705</td>
        <td>-0.066</td>
        <td>0.097</td>
    </tr>
    <tr>
        <td>Month_4</td>
        <td>0.0109</td>
        <td>0.041</td>
        <td>0.262</td>
        <td>0.793</td>
        <td>-0.070</td>
        <td>0.092</td>
    </tr>
    <tr>
        <td>Month_5</td>
        <td>0.2161</td>
        <td>0.039</td>
        <td>5.474</td>
        <td>0.000</td>
        <td>0.139</td>
        <td>0.293</td>
    </tr>
    <tr>
        <td>Month_6</td>
        <td>0.2140</td>
        <td>0.041</td>
        <td>5.176</td>
        <td>0.000</td>
        <td>0.133</td>
        <td>0.295</td>
    </tr>
    <tr>
        <td>Month_7</td>
        <td>0.1599</td>
        <td>0.043</td>
        <td>3.754</td>
        <td>0.000</td>
        <td>0.076</td>
        <td>0.243</td>
    </tr>
    <tr>
        <td>Month_8</td>
        <td>-0.0227</td>
        <td>0.039</td>
        <td>-0.577</td>
        <td>0.564</td>
        <td>-0.100</td>
        <td>0.054</td>
    </tr>
    <tr>
        <td>Month_9</td>
        <td>-0.0651</td>
        <td>0.037</td>
        <td>-1.753</td>
        <td>0.080</td>
        <td>-0.138</td>
        <td>0.008</td>
    </tr>
    <tr>
        <td>Month_10</td>
        <td>-0.0331</td>
        <td>0.040</td>
        <td>-0.837</td>
        <td>0.403</td>
        <td>-0.111</td>
        <td>0.044</td>
    </tr>
    <tr>
        <td>Month_11</td>
        <td>0.0767</td>
        <td>0.044</td>
        <td>1.758</td>
        <td>0.079</td>
        <td>-0.009</td>
        <td>0.162</td>
    </tr>
    <tr>
        <td>Month_12</td>
        <td>0.0408</td>
        <td>0.038</td>
        <td>1.062</td>
        <td>0.288</td>
        <td>-0.034</td>
        <td>0.116</td>
    </tr>
    <tr>
        <td>production_companies_frequency_encoded</td>
        <td>0.0067</td>
        <td>0.033</td>
        <td>0.203</td>
        <td>0.839</td>
        <td>-0.058</td>
        <td>0.071</td>
    </tr>
    <tr>
        <td>director_frequency_encoded</td>
        <td>-0.0055</td>
        <td>0.295</td>
        <td>-0.019</td>
        <td>0.985</td>
        <td>-0.583</td>
        <td>0.572</td>
    </tr>
    <tr>
        <td>Language_Name_frequency_encoded</td>
        <td>-0.0034</td>
        <td>0.017</td>
        <td>-0.201</td>
        <td>0.841</td>
        <td>-0.037</td>
        <td>0.030</td>
    </tr>
    <tr>
        <td>Country_Name_frequency_encoded</td>
        <td>-0.0169</td>
        <td>0.014</td>
        <td>-1.174</td>
        <td>0.240</td>
        <td>-0.045</td>
        <td>0.011</td>
    </tr>
    <tr>
        <td>Genre_Name_frequency_encoded</td>
        <td>-0.0342</td>
        <td>0.009</td>
        <td>-3.705</td>
        <td>0.000</td>
        <td>-0.052</td>
        <td>-0.016</td>
    </tr>
</table>



We have approximately 7'000 observations in this dataset, which makes the results less robust. However, the analysis shows that movies released in months 5, 6, and 7 have the highest positive additive impact on box office revenue. Similarly, months 11 and 12 also show a significant increase, aligning with the known summer and winter box office booms. November also have low p-values, indicating statistical significance.

- In contrast, months 8 and 9 have high p-values, making it difficult to draw meaningful conclusions about their impact on box office performance.

- The variables director_frequency_encoded and country_name_frequency_encoded are statistically insignificant and have neglectably small values.

- Budget also demonstrates a positive additive effect with a low p-value, suggesting that higher money invested in movie production, better it may perform at the box office.

- On top of it, we see that total wins and total nominations for Oscar ceremony have high positive and statically significant impact, suggesting that "premium cinematography" can become a box office banger 


<h3> IMDb Rating Regression </h3>


<table>
  <tr>
    <th>Variable</th>
    <th>Coefficient</th>
    <th>Standard Error</th>
    <th>t-value</th>
    <th>P-value</th>
    <th>95% Confidence Interval (Lower)</th>
    <th>95% Confidence Interval (Upper)</th>
  </tr>
  <tr>
    <td>Intercept</td>
    <td>19.9772</td>
    <td>1.024</td>
    <td>19.508</td>
    <td>0.000</td>
    <td>17.970</td>
    <td>21.985</td>
  </tr>
  <tr>
    <td>Runtime</td>
    <td>0.2628</td>
    <td>0.014</td>
    <td>18.880</td>
    <td>0.000</td>
    <td>0.236</td>
    <td>0.290</td>
  </tr>
  <tr>
    <td>budget</td>
    <td>0.0462</td>
    <td>0.010</td>
    <td>4.508</td>
    <td>0.000</td>
    <td>0.026</td>
    <td>0.066</td>
  </tr>
  <tr>
    <td>Release_Year</td>
    <td>-0.0101</td>
    <td>0.001</td>
    <td>-19.649</td>
    <td>0.000</td>
    <td>-0.011</td>
    <td>-0.009</td>
  </tr>
  <tr>
    <td>total_wins</td>
    <td>-0.0891</td>
    <td>0.020</td>
    <td>-4.407</td>
    <td>0.000</td>
    <td>-0.129</td>
    <td>-0.049</td>
  </tr>
  <tr>
    <td>total_nominations</td>
    <td>0.1442</td>
    <td>0.008</td>
    <td>18.616</td>
    <td>0.000</td>
    <td>0.129</td>
    <td>0.159</td>
  </tr>
  <tr>
    <td>Month_1</td>
    <td>0.3030</td>
    <td>0.041</td>
    <td>7.329</td>
    <td>0.000</td>
    <td>0.222</td>
    <td>0.384</td>
  </tr>
  <tr>
    <td>Month_2</td>
    <td>0.1004</td>
    <td>0.043</td>
    <td>2.357</td>
    <td>0.018</td>
    <td>0.017</td>
    <td>0.184</td>
  </tr>
  <tr>
    <td>Month_3</td>
    <td>0.1692</td>
    <td>0.040</td>
    <td>4.182</td>
    <td>0.000</td>
    <td>0.090</td>
    <td>0.248</td>
  </tr>
  <tr>
    <td>Month_4</td>
    <td>0.2096</td>
    <td>0.041</td>
    <td>5.140</td>
    <td>0.000</td>
    <td>0.130</td>
    <td>0.290</td>
  </tr>
  <tr>
    <td>Month_5</td>
    <td>0.2532</td>
    <td>0.039</td>
    <td>6.462</td>
    <td>0.000</td>
    <td>0.176</td>
    <td>0.330</td>
  </tr>
  <tr>
    <td>Month_6</td>
    <td>0.1546</td>
    <td>0.042</td>
    <td>3.707</td>
    <td>0.000</td>
    <td>0.073</td>
    <td>0.236</td>
  </tr>
  <tr>
    <td>Month_7</td>
    <td>0.1341</td>
    <td>0.043</td>
    <td>3.090</td>
    <td>0.002</td>
    <td>0.049</td>
    <td>0.219</td>
  </tr>
  <tr>
    <td>Month_8</td>
    <td>0.1078</td>
    <td>0.039</td>
    <td>2.742</td>
    <td>0.006</td>
    <td>0.031</td>
    <td>0.185</td>
  </tr>
  <tr>
    <td>Month_9</td>
    <td>0.3345</td>
    <td>0.036</td>
    <td>9.246</td>
    <td>0.000</td>
    <td>0.264</td>
    <td>0.405</td>
  </tr>
  <tr>
    <td>Month_10</td>
    <td>0.1658</td>
    <td>0.038</td>
    <td>4.329</td>
    <td>0.000</td>
    <td>0.091</td>
    <td>0.241</td>
  </tr>
  <tr>
    <td>Month_11</td>
    <td>0.1333</td>
    <td>0.044</td>
    <td>3.041</td>
    <td>0.002</td>
    <td>0.047</td>
    <td>0.219</td>
  </tr>
  <tr>
    <td>Month_12</td>
    <td>0.1750</td>
    <td>0.039</td>
    <td>4.529</td>
    <td>0.000</td>
    <td>0.099</td>
    <td>0.251</td>
  </tr>
  <tr>
    <td>production_companies_frequency_encoded</td>
    <td>-0.0530</td>
    <td>0.021</td>
    <td>-2.557</td>
    <td>0.011</td>
    <td>-0.094</td>
    <td>-0.012</td>
  </tr>
  <tr>
    <td>director_frequency_encoded</td>
    <td>0.5017</td>
    <td>0.077</td>
    <td>6.501</td>
    <td>0.000</td>
    <td>0.350</td>
    <td>0.653</td>
  </tr>
  <tr>
    <td>Language_Name_frequency_encoded</td>
    <td>-0.0193</td>
    <td>0.015</td>
    <td>-1.275</td>
    <td>0.202</td>
    <td>-0.049</td>
    <td>0.010</td>
  </tr>
  <tr>
    <td>Country_Name_frequency_encoded</td>
    <td>-0.1588</td>
    <td>0.013</td>
    <td>-12.101</td>
    <td>0.000</td>
    <td>-0.184</td>
    <td>-0.133</td>
  </tr>
  <tr>
    <td>Genre_Name_frequency_encoded</td>
    <td>0.2114</td>
    <td>0.009</td>
    <td>23.027</td>
    <td>0.000</td>
    <td>0.193</td>
    <td>0.229</td>
  </tr>
</table>

When running almost the same regression, but using the different target, we can see that the results indicate that runtime has a consistently positive additive effect on movie ratings, aligning with previous findings.

- Considering different months as the covariates, the analysis reveals that September, December, May, and January have the highest coefficients with low p-values, suggesting these months are strong predictors of higher movie ratings. Interestingly, the summer-boom effect observed in box office revenue is absent here. This could imply that while summer movies tend to perform well financially, movies released in colder months may resonate better with audiences or critics, possibly due to their depth or complexity.

-  What is more, all Month coefficients have a positive values, that can be explained by the fact, that for "newer" movies (realized in 21st century), we typically have higher rating (simply because more people using the platform checked it) and higher data quality comparing to "old" movies. So better data quality, i.e. knowing of the month realized, typically makes the movies newer and therefore suggests higher ratings.

- Speaking about frequency decoded features, frequent directors and genres emerge as positive predictors of higher movie ratings, indicating that experience and popular genres contribute to a movie's success.

- When we consider IMDb rating as a target, we observe, that total_nominations for Oscar still has a positive and statically significant impact, while total_wins suddenly appear negative. It can be explained by some controversial movies being granted a win in Oscars committee, but not highly apprised by the audience.


<h3> Oscar Nominations / Wins Regression </h3>

We perform two regressions using the same features, but different targets: number of total nominations in Oscar committee and number of total wins awarded by Oscar for a given movie. **Coefficients should be the same right? Right? Turns, there is a major difference between this two models.**

Let's compare only statistically significant coefficient between two regression. (`NA` will indicate that coefficient wasn't statistically significant in corresponding regression)


<table>
    <thead>
        <tr>
            <th>Variable</th>
            <th>Coefficient wins</th>
            <th>Coefficient nominees</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Runtime</td>
            <td>0.255</td>
            <td>0.866</td>
        </tr>
        <tr>
            <td>Budget</td>
            <td>NA</td>
            <td>0.103</td>
        </tr>
        <tr>
            <td>Release Year</td>
            <td>-0.127</td>
            <td>-0.537</td>
        </tr>
        <tr>
            <td>Month_6</td>
            <td>NA</td>
            <td>0.218</td>
        </tr>
        <tr>
            <td>Month_8</td>
            <td>0.087</td>
            <td>NA</td>
        </tr>
        <tr>
            <td>Month_9</td>
            <td>NA</td>
            <td>0.325</td>
        </tr>
        <tr>
            <td>Month_10</td>
            <td>NA</td>
            <td>0.290</td>
        </tr>
        <tr>
            <td>Month_11</td>
            <td>0.157</td>
            <td>0.455</td>
        </tr>
        <tr>
            <td>Month_12</td>
            <td>0.138</td>
            <td>0.896</td>
        </tr>
        <tr> 
            <td>Language_Name_frequency_encoded</td>
            <td>0.078</td>
            <td>0.229</td>
        </tr>
        <tr> 
            <td>Country_Name_frequency_encoded</td>
            <td>NA</td>
            <td>0.081</td>
        </tr>
        <tr>
            <td>Genre_Name_frequency_encoded</td>
            <td>0.034</td>
            <td>0.161</td>
        </tr>
    </tbody>
</table>


1. Turns out coefficients are very different, in the nominees regression, budget becomes statistically significant and having a positive impact on the target. That can be explained that significant money investment can increase hypo around your movie to such magnitude, that it will bring couple of nominations in some, might not really important, categories. Though with actual win - budget can hardly help you here

2. We once again encounter the phenomenon of "summer film", which works rather well for nominees regression and has a positive impact, while for wining an oscar it is not significant statistically and films with a deep sense winning an oscar are getting released in November, December or (surprisingly) August. On top of it, the release month impact is much higher for number of total nominations as a target comparing with oscar winning target.


<h2> Logistic Regression </h2>

<h3> Find a "good" movies relevant features </h3>
We understood, that oscar winning prediction or nomination is rather complicated problem, which requires much more data involved. Maybe we can at least define, will the film be considered good or not? Let's define the good film as one, laying in top 90% quantile of the IMDb distribution, i.e. having rating > 8. 

Now, on top of our typical feature set, we will use total Oscar nominations/wins as additional features in the analysis (Is winning an oscar a guarantee to be perceived as a good film in the eyes of the audience).

Let's explore selected results only to save some space purely (only coefficient on statistically significant covariates at 5% level)


<table>
    <thead>
        <tr>
            <th>Variable</th>
            <th>Coefficient</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Runtime</td>
            <td>0.574</td>
        </tr>
        <tr>
            <td>Month_5</td>
            <td>0.743</td>
        </tr>
        <tr>
            <td>Month_6</td>
            <td>0.702</td>
        </tr>
        <tr>
            <td>Month_7</td>
            <td>0.717</td>
        </tr>
        <tr>
            <td>Production companies</td>
            <td>0.293</td>
        </tr>
        <tr>
            <td>Director encoded</td>
            <td>1.307</td>
        </tr>
        <tr>
            <td>Language Name encoded</td>
            <td>-0.255</td>
        </tr>
        <tr>
            <td>Country Name encoded</td>
            <td>-0.301</td>
        </tr>
        <tr>
            <td>Total Nominations</td>
            <td>0.300</td>
        </tr>
    </tbody>
</table>



When analyzing for probability of movie being good, it shows very interesting observations:

- First of all, budget variable becomes statistically insignificant, meaning that regular viewers overall sympathy can be hardly impacted by the money invested

- Total Oscar nominations, have high positive incremental impact on the movie being good, but here very likely it can be caused by reverse causality issue (what was first good movie review from the audience of oscar nominations?)

- Total Oscar wins, however, is not statistically significant and have a negative coefficient of -0.0054, demonstrating issues, which we encountered above - some controversial movies being granted a win in Oscars committee at the same time being not highly apprised by the audience.

That add very interesting ingredient to our formula of successful movie - **it should have a lot of oscar nominations, but might not a single to be lowed by the audience**.


<h3> Section Conclusion </h3>
Common Results:
-   We can conclude that runtime seems to have a positive effect on both *IMDb Rating* and *Box Office Revenue* and *Oscar Nominations*. Similarly, director frequency (the number of times a director appears in movies) is a strong predictor of success for both metrics. Budget is not always, but typically a good incremental proxy of movie success.


Metric-dependent results:
-   For Box Office Revenue, the release month plays a significant role, with movies released in May, June, and July showing the highest positive impact. Budget and total wins/nominations also positively influence revenue.
-   For IMDb Ratings, runtime, total nominations, and specific months (January, May, September, and December) are strong predictors. Interestingly, total wins have a negative impact, possibly due to controversial Oscar decisions.
-   For Oscar Nominations/Wins, runtime and release month are crucial. Budget is significant for nominations but not for wins, indicating that while money can generate buzz, it doesn't guarantee an Oscar win.
-   For predicting "good" movies (IMDb rating > 8), runtime, specific months (May, June, July), and total nominations are significant. Budget is not a predictor, and total wins are not statistically significant, highlighting the complexity of audience perception.
-   When it comes to other covariates, they vary depending on the success metric. Country frequency positively influences Box Office Revenue—likely due to the dominance of U.S. films, which often gross high revenues. Genre frequency is a strong predictor of higher IMDb Ratings, potentially because popular genres tend to resonate better with audiences and critics. To have high IMBd rating it's quite indicator to be nominated by the Oscar committee.
-   Additionally, we identified specific months where movies tend to perform better, though these vary depending on the success metric.

Limitations:
-   Most models have relatively low R-squared values, indicating room for improvement. This can be addressed by adding more data to the analysis and some feature engendering.  


<h1>Data Story Conclusion 🎥 🧪🔬💊 </h1>


In our scientific investigation of movie success, we analyzed multiple variables to understand their impact on a film's performance, treating each factor like a distinct element in an experiment.

- Actor Success & Oscar Score: Our experiment revealed a slight positive correlation between the Oscar Score and movie ratings. This suggests that higher recognition at the Oscars may contribute to a movie's success, though this factor may interact with others, similar to how variables in a lab experiment influence each other.

- Budget: The effect of budget on success was less pronounced than anticipated. Much like a chemical reaction, the right conditions (e.g., timing, marketing) seemed to matter more than just the raw material (budget) itself.

- Timing of Release: We observed a “summer phenomenon,” where films released in the summer months had significantly better performance. This could be likened to a favorable environmental condition in a lab experiment, where the timing and setting directly impact the results.


- Production Country: The data showed a clear dominance of American films, reflecting Hollywood’s global influence. This "bias" in our sample could be compared to a skewed control group in a lab experiment, highlighting the importance of considering regional factors in our analysis.


- Tropes: Certain tropes, such as "stupid crooks" and "morally bankrupt bankers," acted like successful catalysts in both critical and commercial reactions. Additionally, the data suggests that male-dominated narratives are still prevalent, akin to an unbalanced sample in a scientific study.

- Regression Analysis: 

<p style="font-weight: 700"> Therefore, the chemical concoction for a successful film would include a release date in the summer, using popular tropes like stupid crooks or morally bankrupt bankers, to optimize both box office earnings and audience reception. Additionally, ensuring a longer runtime and working with a high-frequency director could further enhance the film's chances of success across critical and commercial metrics</p>


*With empirical precision, from the Blue Sweater Research Team. Wishing you all a scientifically delightful Christmas, New Year, and joyful holidays in general!* 🎁🎄🎬❄️





