---
layout: full
---

Welcome to the BlueSweater Lab, where we treat movies as complex chemical reactions—combining various elements that can either explode into blockbuster success or fizzle into obscurity! From budgets and genres to audience ratings and box office numbers, every factor plays a pivotal role in a movie’s success formula.

<p style="font-weight: 700"> Our goal: to identify the key ingredients that contribute to a movie’s success and uncover what truly makes a film resonate with audiences.</p>

In this exploration, we'll examine how different elements—such as ratings, star power, Oscar wins, genre, and production country—interact and influence a movie’s journey to the top. But first, meet the team :)

## Meet Our BlueSweater Lab Members 

<div style="display: flex; gap: 15px; justify-content: center; flex-wrap: wrap;">
    <div style="text-align: center; min-width: 160px; flex: 1 1 160px; margin-bottom: 15px;">
        <img src="scientists_pics/ivan.png" alt="Ivan" style="width: 160px; height: auto; max-width: 100%;"/>
        <p style="margin-bottom: 5px;">Ivan</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in MTE</p>
    </div>
    <div style="text-align: center; min-width: 160px; flex: 1 1 160px; margin-bottom: 15px;">
        <img src="scientists_pics/david.jpg" alt="David" style="width: 160px; height: auto; max-width: 100%;"/>
        <p style="margin-bottom: 5px;">David</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in CS</p>
    </div>
    <div style="text-align: center; min-width: 160px; flex: 1 1 160px; margin-bottom: 15px;">
        <img src="scientists_pics/adam.jpg" alt="Adam" style="width: 160px; height: auto; max-width: 100%;"/>
        <p style="margin-bottom: 5px;">Adam</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in CS</p>
    </div>
    <div style="text-align: center; min-width: 160px; flex: 1 1 160px; margin-bottom: 15px;">
        <img src="scientists_pics/ali.png" alt="Ali" style="width: 160px; height: auto; max-width: 100%;"/>
        <p style="margin-bottom: 5px;">Ali</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in EEE</p>
    </div>
    <div style="text-align: center; min-width: 160px; flex: 1 1 160px; margin-bottom: 15px;">
        <img src="scientists_pics/dana.png" alt="Dana" style="width: 160px; height: auto; max-width: 100%;"/>
        <p style="margin-bottom: 5px;">Dana</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in NX</p>
    </div>
</div>
<!-- 
<style>
  div img:hover {
    transform: scale(1.1) rotate(5deg); /* Scale up and rotate slightly */
    box-shadow: 4px 4px 12px rgba(0, 0, 0, 0.2); /* Increase shadow size */
    opacity: 0.9; /* Slight opacity change */
  }
</style> -->



<br>

## Let’s get started by deriving our ingredients...

The primary dataset we'll use for this analysis is the **CMU Movie Dataset**, which contains key information like:

- Movie ID (Wikipedia, Freebase)
- Name
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

## Addressing key issues with our dataset

### 1) Duplicates
There are 6,377 duplicates in the dataset. Initially, we considered dropping movies with the same title, but this approach could remove important variations—such as different release years or production countries. 

For example, "100 Days" appears twice: once in 1991 and again in 2001. Therefore, we'll retain duplicates to preserve the data's richness.

### 2) Nulls

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
    marker: { color: 'purple' },
    hovertemplate: '%{y}%<extra></extra>'  // Hover display only
  }];
  
  var layout = {
    title: 'Percentage of Null Values in Movie Dataset Columns',
    xaxis: {
      title: 'Columns',
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

Our dataset contains a significant number of missing values, especially in the Box Office Revenue column. This poses challenges for standard data-cleaning methods, such as removing rows with null values or imputing them with the mean, as these approaches could result in considerable data loss.

That said, it's important to recognize that Box Office Revenue represents only one component of the total revenue, and the Revenue column has far fewer missing values. This allows us to analyze the relationship between box office revenue and total revenue to extract meaningful insights.

For other columns with missing data—such as Producers, Production Company, Writers, and Director—we've decided not to prioritize them in our analysis. Additional details on this decision can be found in the analysis proposal section.

To deal with the rest of the nulls, we employed two strategies:

- **Helper Datasets (TMDB, IMDB, Metacritic)**: These datasets help fill in missing data points with simple data frame merges. 
- **OMDB API**: In a bid to tackle the remaining nulls, we purchased an API key to pull additional information and fill gaps, particularly for Box Office Revenue, Runtime, and IMDb Vote Averages. 



| Column             | Before OMDB API (Nulls) | After OMDB API (NUlls) |
|--------------------|-------------------------|------------------------|
| Box Office Revenue | 73,452                  | 68,270                 |
| Runtime            | 20,498                  | 11,642                 |
| IMDB Votes         | 22,452                  | 21,044                 |

The OMDB API successfully replaced hundreds of null values, improving the dataset compared to its previous state.

##  Derived chemical movie ingredients

From the above dataset, its helpers and api, we have derived the following chemical movie ingredietns that we will be exploring in our project:

### Section 1: Global Influence in Cinema  
The global cinema landscape reflects the dominance of certain countries and languages in shaping the film industry. To be more specific, we plan to analyze our data to identify the top countries and languages in the film industry. Additionally, we examined how these factors relate to critical metrics such as budget and revenue, offering insights into their impact on global cinematic success. This approach provides a straightforward yet valuable perspective on the key players and their contributions to the entertainment ecosystem.

### Section 2: Blueprint of Success in Acting 
In the movie lab, an actor's journey to stardom is a critical ingredient in our cinematic formula. Actors reach their breakthrough moments at different stages—at what age do they typically "ignite," and is there a critical mass of roles that triggers success, or can one iconic role act as a catalyst for fame? Does this trajectory vary between men and women? To analyze this, we also use our *Oscar Score*, a weighted metric reflecting actors' achievements through Oscar wins and nominations, to uncover trends in performance, box office revenue, audience ratings, and age, shedding light on the paths actors take to rise to prominence.

 
### Section 3: Tropes and their influence on Movie Success
Character tropes are central to storytelling, but which ones consistently lead to success on the big screen? In this section, we will explore how different character archetypes and gender pairings impact a film’s performance, from box office revenue to critics’ and audience ratings. By analyzing patterns in successful movies, we aim to uncover which tropes resonate most with viewers and whether certain dynamics—such as the pairing of leads by gender or genre-specific archetypes—play a significant role. This study seeks to provide insights into the storytelling elements that drive both commercial and critical acclaim, offering a deeper understanding of how character dynamics shape the overall success of a movie. 


### Section 4: Budget and Success Correlation 
Does a high budget truly guarantee commercial success, or are other factors more influential in determining a movie’s performance? In this section, we will examine the relationship between budget size and success by analyzing box office-to-budget ratios and comparing the relative "custom rating" performance of the highest-budget films each year. Beyond sheer financial investment, we’ll investigate the role of high-profile collaborations, such as A-list actors and directors, in shaping outcomes. This analysis aims to uncover whether bigger budgets consistently lead to better results or if success depends on a more nuanced combination of resources, talent, and strategic execution.


### Section 5: Timing and Audience Reception
Timing can make or break a movie’s success, but when is the optimal moment to release a film? This section explores the influence of seasonal and holiday periods on audience reception and box office revenue. By segmenting the year into distinct timeframes, we will analyze how factors such as genre, sequel status, and prevailing cultural moods align with release strategies. This investigation aims to identify patterns in audience behavior and uncover strategies that filmmakers and studios can use to maximize both reception and financial performance. Understanding the timing sweet spot could provide valuable insights into the art and science of film release planning.



<!-- OSCARS -->
# Oscars

When people think about a movie's success, the first thing that often comes to mind is the actors. A well-known actor can elevate a movie, making it more popular, drawing larger audiences, and ultimately contributing to its commercial success. Conversely, an unknown actor may not have the same impact. In this section, we aim to explore whether there is a relationship between an actor's success and the average movie ratings they appear in.

## What does it mean for an actor to be successful?
Before diving into the analysis, we must first define what constitutes a "successful actor." For many in the industry, winning an Oscar is the pinnacle of achievement. While an actor's success can be measured in many ways, for the purposes of this milestone, we will define a successful actor as one who has won the highest number of Oscars.

However, Best Actor and Best Actress aren’t the only possible Oscar wins; actors can also receive recognition in Supporting Actor and Supporting Actress categories. Additionally, Oscar nominations play a significant role, as even a nomination can enhance an actor’s reputation and attract audiences.

However, lead roles generally have a larger impact on a movie’s success, as these actors often drive the story and have more screen time.  For this reason, we are going to build an Oscar score that applies weights to different award categories, giving greater significance to lead roles and including nominations to reflect their influence. We have decided to apply the following weights:

## Oscar Score Formula

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


## IMDb Score vs Oscar Score

This scatter plot shows the relationship between IMDb scores and total Oscar scores of various movies.

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
                text: 'Scatter Plot: IMDb Score vs Oscar Score'
            },
            legend: {
                display: false // Hide the legend
            }
        },
        scales: {
            x: {
                title: { display: true, text: 'IMDb Score' },  // Change title to IMDb Score
                beginAtZero: false
            },
            y: {
                title: { display: true, text: 'Oscar Score' },  // Change title to Oscar Score
                beginAtZero: false,
                suggestedMin: 30, 
                suggestedMax: 100 
            }
        }
    }
});


</script>



The plot of IMDb scores versus Oscar scores shows a slight correlation between the two variables, but the relationship is not strong as we can see by the regression line. While some actors with higher Oscar scores also tend to have higher IMDb scores, this pattern is not consistent across all data points. One reason for this is that the Oscars and IMDb ratings measure different aspects of an actor's career. 

Oscar scores are based on professional recognition for outstanding performances, typically within the context of prestigious award ceremonies, whereas IMDb scores reflect the general public's opinion of movies and performances. These two metrics are influenced by different factors—Oscar wins or nominations are often determined by industry professionals, while IMDb ratings are driven by broader audience sentiments. 

As a result, while some actors may appear in both highly rated Oscar-winning films and movies with high IMDb scores, the correlation between the two is weak. This is because each score is influenced by different factors and biases—Oscars are based on the opinions of industry professionals, while IMDb ratings reflect general public sentiment.

It is thus  useful to conduct further experiments and explore additional factors that could impact a movie's success. We should also consider incorporating other sources of data, such as Metacritic, which includes professional critics' ratings, and Rotten Tomatoes, to provide a more comprehensive view of a movie's reception across different audiences.

## Now try searching for your favorite actor!
Type in the name of an actor in the search bar below to see their IMDb score, Oscar score, and number of movies they have appeared in.

{% raw %}
<h2>Actor Search</h2>
<div id="search-container">
    <input type="text" id="actor-search" placeholder="Enter an actor's name..." style="width: 300px; padding: 10px;">
</div>
<div id="result" style="margin-top: 20px;"></div>

<script>
const actors = [{"actor_name":"Meryl Streep","total_oscar_score":46,"imdb_score":67.375,"num_movies":8},{"actor_name":"Katharine Hepburn","total_oscar_score":36,"imdb_score":76.0,"num_movies":1},{"actor_name":"Bette Davis","total_oscar_score":28,"imdb_score":80.5,"num_movies":2},{"actor_name":"Jack Nicholson","total_oscar_score":28,"imdb_score":73.3,"num_movies":10},{"actor_name":"Spencer Tracy","total_oscar_score":24,"imdb_score":75.0,"num_movies":2},{"actor_name":"Denzel Washington","total_oscar_score":21,"imdb_score":65.8387096774,"num_movies":31},{"actor_name":"Marlon Brando","total_oscar_score":21,"imdb_score":78.2,"num_movies":5},{"actor_name":"Jack Lemmon","total_oscar_score":20,"imdb_score":69.0,"num_movies":2},{"actor_name":"Paul Newman","total_oscar_score":20,"imdb_score":74.8,"num_movies":5},{"actor_name":"Laurence Olivier","total_oscar_score":20,"imdb_score":79.0,"num_movies":1},{"actor_name":"Dustin Hoffman","total_oscar_score":20,"imdb_score":70.4,"num_movies":10},{"actor_name":"Jane Fonda","total_oscar_score":19,"imdb_score":60.0,"num_movies":1},{"actor_name":"Robert De Niro","total_oscar_score":18,"imdb_score":69.3666666667,"num_movies":30},{"actor_name":"Cate Blanchett","total_oscar_score":18,"imdb_score":67.6666666667,"num_movies":6},{"actor_name":"Frances McDormand","total_oscar_score":18,"imdb_score":75.0,"num_movies":4},{"actor_name":"Al Pacino","total_oscar_score":17,"imdb_score":73.0,"num_movies":13},{"actor_name":"Tom Hanks","total_oscar_score":17,"imdb_score":71.2307692308,"num_movies":39},{"actor_name":"Anthony Hopkins","total_oscar_score":16,"imdb_score":67.75,"num_movies":12},{"actor_name":"Daniel Day-Lewis","total_oscar_score":16,"imdb_score":73.6666666667,"num_movies":6},{"actor_name":"Peter O'Toole","total_oscar_score":16,"imdb_score":80.0,"num_movies":1},{"actor_name":"Gary Cooper","total_oscar_score":16,"imdb_score":77.0,"num_movies":1},{"actor_name":"Sean Penn","total_oscar_score":16,"imdb_score":70.5555555556,"num_movies":9},{"actor_name":"Elizabeth Taylor","total_oscar_score":16,"imdb_score":74.6666666667,"num_movies":3},{"actor_name":"Paul Muni","total_oscar_score":15,"imdb_score":75.0,"num_movies":1},{"actor_name":"Sissy Spacek","total_oscar_score":15,"imdb_score":73.0,"num_movies":2},{"actor_name":"Judi Dench","total_oscar_score":15,"imdb_score":70.5,"num_movies":2},{"actor_name":"Ellen Burstyn","total_oscar_score":14,"imdb_score":78.5,"num_movies":2},{"actor_name":"Jodie Foster","total_oscar_score":14,"imdb_score":69.3333333333,"num_movies":6},{"actor_name":"Leonardo DiCaprio","total_oscar_score":14,"imdb_score":70.1428571429,"num_movies":21},{"actor_name":"Michael Caine","total_oscar_score":14,"imdb_score":64.375,"num_movies":8},{"actor_name":"Kate Winslet","total_oscar_score":14,"imdb_score":70.625,"num_movies":8},{"actor_name":"Richard Burton","total_oscar_score":13,"imdb_score":63.3333333333,"num_movies":3},{"actor_name":"Robert Duvall","total_oscar_score":13,"imdb_score":63.5,"num_movies":2},{"actor_name":"Anne Bancroft","total_oscar_score":13,"imdb_score":77.0,"num_movies":1},{"actor_name":"Audrey Hepburn","total_oscar_score":13,"imdb_score":75.1666666667,"num_movies":6},{"actor_name":"Gregory Peck","total_oscar_score":13,"imdb_score":73.5,"num_movies":8},{"actor_name":"Jeff Bridges","total_oscar_score":13,"imdb_score":66.2352941176,"num_movies":17},{"actor_name":"Susan Sarandon","total_oscar_score":13,"imdb_score":70.6666666667,"num_movies":3},{"actor_name":"Shirley MacLaine","total_oscar_score":13,"imdb_score":70.5,"num_movies":2},{"actor_name":"James Stewart","total_oscar_score":13,"imdb_score":79.4,"num_movies":5},{"actor_name":"Ren\u00e9e Zellweger","total_oscar_score":12,"imdb_score":62.3333333333,"num_movies":6},{"actor_name":"Emma Stone","total_oscar_score":12,"imdb_score":69.0,"num_movies":5},{"actor_name":"Glenn Close","total_oscar_score":12,"imdb_score":65.2,"num_movies":5},{"actor_name":"Gene Hackman","total_oscar_score":12,"imdb_score":71.5714285714,"num_movies":7},{"actor_name":"Nicole Kidman","total_oscar_score":12,"imdb_score":65.125,"num_movies":16},{"actor_name":"Burt Lancaster","total_oscar_score":11,"imdb_score":67.75,"num_movies":4},{"actor_name":"Diane Keaton","total_oscar_score":11,"imdb_score":59.6,"num_movies":5},{"actor_name":"Sally Field","total_oscar_score":11,"imdb_score":66.6666666667,"num_movies":3},{"actor_name":"Julianne Moore","total_oscar_score":11,"imdb_score":63.0,"num_movies":4},{"actor_name":"Jennifer Lawrence","total_oscar_score":10,"imdb_score":61.8181818182,"num_movies":11},{"actor_name":"William Hurt","total_oscar_score":10,"imdb_score":69.0,"num_movies":4},{"actor_name":"Jon Voight","total_oscar_score":10,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"Hilary Swank","total_oscar_score":10,"imdb_score":67.875,"num_movies":8},{"actor_name":"Anthony Quinn","total_oscar_score":10,"imdb_score":72.5,"num_movies":2},{"actor_name":"Emma Thompson","total_oscar_score":10,"imdb_score":69.6666666667,"num_movies":6},{"actor_name":"Morgan Freeman","total_oscar_score":10,"imdb_score":61.9,"num_movies":10},{"actor_name":"Joaquin Phoenix","total_oscar_score":10,"imdb_score":65.2727272727,"num_movies":11},{"actor_name":"Vivien Leigh","total_oscar_score":10,"imdb_score":78.5,"num_movies":2},{"actor_name":"Clark Gable","total_oscar_score":9,"imdb_score":79.0,"num_movies":1},{"actor_name":"Charles Laughton","total_oscar_score":9,"imdb_score":76.3333333333,"num_movies":3},{"actor_name":"Charlize Theron","total_oscar_score":9,"imdb_score":65.4444444444,"num_movies":9},{"actor_name":"Javier Bardem","total_oscar_score":9,"imdb_score":64.0,"num_movies":3},{"actor_name":"Ben Kingsley","total_oscar_score":9,"imdb_score":67.375,"num_movies":8},{"actor_name":"Bing Crosby","total_oscar_score":9,"imdb_score":65.0,"num_movies":1},{"actor_name":"Bradley Cooper","total_oscar_score":9,"imdb_score":67.5454545455,"num_movies":11},{"actor_name":"Annette Bening","total_oscar_score":9,"imdb_score":73.0,"num_movies":1},{"actor_name":"William Holden","total_oscar_score":9,"imdb_score":75.75,"num_movies":4},{"actor_name":"Will Smith","total_oscar_score":9,"imdb_score":68.6538461538,"num_movies":26},{"actor_name":"Albert Finney","total_oscar_score":9,"imdb_score":72.0,"num_movies":1},{"actor_name":"Gary Oldman","total_oscar_score":9,"imdb_score":65.7142857143,"num_movies":7},{"actor_name":"Geoffrey Rush","total_oscar_score":9,"imdb_score":67.3333333333,"num_movies":3},{"actor_name":"Julie Andrews","total_oscar_score":9,"imdb_score":76.5,"num_movies":2},{"actor_name":"George C. Scott","total_oscar_score":9,"imdb_score":68.5,"num_movies":4},{"actor_name":"George Clooney","total_oscar_score":9,"imdb_score":64.380952381,"num_movies":21},{"actor_name":"Russell Crowe","total_oscar_score":9,"imdb_score":69.875,"num_movies":16},{"actor_name":"Robin Williams","total_oscar_score":9,"imdb_score":68.4666666667,"num_movies":15},{"actor_name":"Julia Roberts","total_oscar_score":9,"imdb_score":66.1538461538,"num_movies":13},{"actor_name":"Helen Mirren","total_oscar_score":9,"imdb_score":71.0,"num_movies":3},{"actor_name":"Holly Hunter","total_oscar_score":9,"imdb_score":74.0,"num_movies":1},{"actor_name":"Humphrey Bogart","total_oscar_score":9,"imdb_score":78.2,"num_movies":5},{"actor_name":"Charles Boyer","total_oscar_score":8,"imdb_score":75.0,"num_movies":1},{"actor_name":"Kathy Bates","total_oscar_score":8,"imdb_score":75.0,"num_movies":2},{"actor_name":"Jessica Chastain","total_oscar_score":8,"imdb_score":67.75,"num_movies":8},{"actor_name":"Pen\u00e9lope Cruz","total_oscar_score":8,"imdb_score":68.0,"num_movies":4},{"actor_name":"Alan Arkin","total_oscar_score":8,"imdb_score":72.5,"num_movies":2},{"actor_name":"Warren Beatty","total_oscar_score":8,"imdb_score":67.0,"num_movies":3},{"actor_name":"Viola Davis","total_oscar_score":8,"imdb_score":71.5,"num_movies":2},{"actor_name":"Rod Steiger","total_oscar_score":8,"imdb_score":77.0,"num_movies":1},{"actor_name":"Helen Hayes","total_oscar_score":8,"imdb_score":61.0,"num_movies":1},{"actor_name":"Kevin Spacey","total_oscar_score":8,"imdb_score":70.4,"num_movies":5},{"actor_name":"Philip Seymour Hoffman","total_oscar_score":8,"imdb_score":68.0,"num_movies":2},{"actor_name":"Olivia Colman","total_oscar_score":8,"imdb_score":69.3333333333,"num_movies":3},{"actor_name":"Natalie Portman","total_oscar_score":8,"imdb_score":67.75,"num_movies":8},{"actor_name":"Michelle Williams","total_oscar_score":8,"imdb_score":69.8,"num_movies":5},{"actor_name":"Christian Bale","total_oscar_score":8,"imdb_score":70.2727272727,"num_movies":22},{"actor_name":"Brad Pitt","total_oscar_score":8,"imdb_score":70.4782608696,"num_movies":23},{"actor_name":"Colin Firth","total_oscar_score":7,"imdb_score":65.7142857143,"num_movies":7},{"actor_name":"Henry Fonda","total_oscar_score":7,"imdb_score":78.0,"num_movies":2},{"actor_name":"John Wayne","total_oscar_score":7,"imdb_score":72.5833333333,"num_movies":12},{"actor_name":"Eddie Redmayne","total_oscar_score":7,"imdb_score":71.5,"num_movies":8},{"actor_name":"Sidney Poitier","total_oscar_score":7,"imdb_score":77.0,"num_movies":1},{"actor_name":"Tommy Lee Jones","total_oscar_score":7,"imdb_score":63.875,"num_movies":8},{"actor_name":"Sandra Bullock","total_oscar_score":7,"imdb_score":65.9130434783,"num_movies":23},{"actor_name":"Saoirse Ronan","total_oscar_score":7,"imdb_score":70.3333333333,"num_movies":9},{"actor_name":"Barbra Streisand","total_oscar_score":7,"imdb_score":65.6666666667,"num_movies":3},{"actor_name":"Sophia Loren","total_oscar_score":7,"imdb_score":65.0,"num_movies":1},{"actor_name":"Marion Cotillard","total_oscar_score":7,"imdb_score":66.0,"num_movies":1},{"actor_name":"Richard Dreyfuss","total_oscar_score":7,"imdb_score":72.0,"num_movies":2},{"actor_name":"Rex Harrison","total_oscar_score":7,"imdb_score":62.0,"num_movies":1},{"actor_name":"Amy Adams","total_oscar_score":7,"imdb_score":69.7142857143,"num_movies":7},{"actor_name":"Reese Witherspoon","total_oscar_score":7,"imdb_score":62.7,"num_movies":10},{"actor_name":"Liza Minnelli","total_oscar_score":7,"imdb_score":74.0,"num_movies":1},{"actor_name":"Walter Matthau","total_oscar_score":7,"imdb_score":58.0,"num_movies":1},{"actor_name":"Peter Ustinov","total_oscar_score":7,"imdb_score":66.0,"num_movies":2},{"actor_name":"Nicolas Cage","total_oscar_score":7,"imdb_score":59.3442622951,"num_movies":61},{"actor_name":"Peter Finch","total_oscar_score":7,"imdb_score":79.0,"num_movies":1},{"actor_name":"Kirk Douglas","total_oscar_score":6,"imdb_score":73.4,"num_movies":5},{"actor_name":"Helen Hunt","total_oscar_score":6,"imdb_score":67.5,"num_movies":2},{"actor_name":"Robert Downey Jr.","total_oscar_score":6,"imdb_score":72.8823529412,"num_movies":17},{"actor_name":"Casey Affleck","total_oscar_score":6,"imdb_score":69.75,"num_movies":4},{"actor_name":"Anjelica Huston","total_oscar_score":6,"imdb_score":70.0,"num_movies":1},{"actor_name":"Viggo Mortensen","total_oscar_score":6,"imdb_score":73.5,"num_movies":8},{"actor_name":"Cher","total_oscar_score":6,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Christoph Waltz","total_oscar_score":6,"imdb_score":57.0,"num_movies":1},{"actor_name":"Laura Dern","total_oscar_score":6,"imdb_score":71.0,"num_movies":1},{"actor_name":"Carey Mulligan","total_oscar_score":6,"imdb_score":71.3333333333,"num_movies":6},{"actor_name":"Marcello Mastroianni","total_oscar_score":6,"imdb_score":82.0,"num_movies":1},{"actor_name":"Johnny Depp","total_oscar_score":6,"imdb_score":67.8620689655,"num_movies":29},{"actor_name":"Jamie Foxx","total_oscar_score":6,"imdb_score":69.0,"num_movies":11},{"actor_name":"Mahershala Ali","total_oscar_score":6,"imdb_score":73.0,"num_movies":1},{"actor_name":"Mickey Rooney","total_oscar_score":6,"imdb_score":63.0,"num_movies":2},{"actor_name":"Michelle Yeoh","total_oscar_score":5,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Octavia Spencer","total_oscar_score":5,"imdb_score":58.0,"num_movies":1},{"actor_name":"Geena Davis","total_oscar_score":5,"imdb_score":58.0,"num_movies":1},{"actor_name":"Nick Nolte","total_oscar_score":5,"imdb_score":63.0,"num_movies":4},{"actor_name":"Tom Cruise","total_oscar_score":5,"imdb_score":65.6,"num_movies":35},{"actor_name":"Ed Harris","total_oscar_score":5,"imdb_score":64.5,"num_movies":4},{"actor_name":"Louise Fletcher","total_oscar_score":5,"imdb_score":62.0,"num_movies":1},{"actor_name":"Frank Sinatra","total_oscar_score":5,"imdb_score":64.0,"num_movies":1},{"actor_name":"Forest Whitaker","total_oscar_score":5,"imdb_score":71.3333333333,"num_movies":3},{"actor_name":"F. Murray Abraham","total_oscar_score":5,"imdb_score":80.0,"num_movies":1},{"actor_name":"Lee Marvin","total_oscar_score":5,"imdb_score":76.0,"num_movies":1},{"actor_name":"Joe Pesci","total_oscar_score":5,"imdb_score":75.0,"num_movies":1},{"actor_name":"Juliette Binoche","total_oscar_score":5,"imdb_score":65.75,"num_movies":4},{"actor_name":"Whoopi Goldberg","total_oscar_score":5,"imdb_score":64.6666666667,"num_movies":3},{"actor_name":"Willem Dafoe","total_oscar_score":5,"imdb_score":70.5714285714,"num_movies":7},{"actor_name":"Laura Linney","total_oscar_score":5,"imdb_score":60.0,"num_movies":2},{"actor_name":"Jack Palance","total_oscar_score":5,"imdb_score":66.0,"num_movies":1},{"actor_name":"Rami Malek","total_oscar_score":5,"imdb_score":80.0,"num_movies":1},{"actor_name":"Marisa Tomei","total_oscar_score":5,"imdb_score":60.0,"num_movies":1},{"actor_name":"Jean Dujardin","total_oscar_score":5,"imdb_score":72.75,"num_movies":4},{"actor_name":"Goldie Hawn","total_oscar_score":5,"imdb_score":67.6666666667,"num_movies":3},{"actor_name":"Michelle Pfeiffer","total_oscar_score":5,"imdb_score":63.3333333333,"num_movies":6},{"actor_name":"Michael Douglas","total_oscar_score":5,"imdb_score":66.4666666667,"num_movies":15},{"actor_name":"Heath Ledger","total_oscar_score":5,"imdb_score":67.25,"num_movies":4},{"actor_name":"Halle Berry","total_oscar_score":5,"imdb_score":61.1428571429,"num_movies":7},{"actor_name":"Gwyneth Paltrow","total_oscar_score":5,"imdb_score":62.0,"num_movies":1},{"actor_name":"Roberto Benigni","total_oscar_score":5,"imdb_score":64.75,"num_movies":4},{"actor_name":"Ryan Gosling","total_oscar_score":5,"imdb_score":71.9,"num_movies":10},{"actor_name":"Jeremy Irons","total_oscar_score":5,"imdb_score":70.0,"num_movies":4},{"actor_name":"Matthew McConaughey","total_oscar_score":5,"imdb_score":66.2380952381,"num_movies":21},{"actor_name":"Natalie Wood","total_oscar_score":5,"imdb_score":73.0,"num_movies":2},{"actor_name":"Matt Damon","total_oscar_score":5,"imdb_score":68.3181818182,"num_movies":22},{"actor_name":"Ray Milland","total_oscar_score":5,"imdb_score":78.5,"num_movies":2},{"actor_name":"Martin Landau","total_oscar_score":5,"imdb_score":53.0,"num_movies":1},{"actor_name":"Sigourney Weaver","total_oscar_score":5,"imdb_score":64.5714285714,"num_movies":7},{"actor_name":"Yul Brynner","total_oscar_score":5,"imdb_score":67.25,"num_movies":4},{"actor_name":"Anne Hathaway","total_oscar_score":5,"imdb_score":63.5714285714,"num_movies":14},{"actor_name":"Brendan Fraser","total_oscar_score":5,"imdb_score":61.1875,"num_movies":16},{"actor_name":"Cillian Murphy","total_oscar_score":5,"imdb_score":54.25,"num_movies":4},{"actor_name":"Christopher Plummer","total_oscar_score":5,"imdb_score":70.5,"num_movies":2},{"actor_name":"Adrien Brody","total_oscar_score":5,"imdb_score":68.625,"num_movies":8},{"actor_name":"Charlton Heston","total_oscar_score":5,"imdb_score":71.4285714286,"num_movies":14},{"actor_name":"Brie Larson","total_oscar_score":5,"imdb_score":71.4,"num_movies":5},{"actor_name":"Daniel Kaluuya","total_oscar_score":5,"imdb_score":72.5,"num_movies":2},{"actor_name":"David Niven","total_oscar_score":5,"imdb_score":56.8333333333,"num_movies":6},{"actor_name":"Angelina Jolie","total_oscar_score":5,"imdb_score":66.3,"num_movies":10},{"actor_name":"Benedict Cumberbatch","total_oscar_score":4,"imdb_score":72.875,"num_movies":8},{"actor_name":"James Mason","total_oscar_score":4,"imdb_score":74.0,"num_movies":2},{"actor_name":"Clint Eastwood","total_oscar_score":4,"imdb_score":70.1666666667,"num_movies":30},{"actor_name":"Bette Midler","total_oscar_score":4,"imdb_score":72.5,"num_movies":2},{"actor_name":"Walter Pidgeon","total_oscar_score":4,"imdb_score":73.0,"num_movies":1},{"actor_name":"Naomi Watts","total_oscar_score":4,"imdb_score":64.7647058824,"num_movies":17},{"actor_name":"Sam Rockwell","total_oscar_score":4,"imdb_score":62.8333333333,"num_movies":6},{"actor_name":"Clifton Webb","total_oscar_score":4,"imdb_score":66.0,"num_movies":1},{"actor_name":"Peter Sellers","total_oscar_score":4,"imdb_score":72.5,"num_movies":4},{"actor_name":"Jill Clayburgh","total_oscar_score":4,"imdb_score":65.0,"num_movies":1},{"actor_name":"Mark Ruffalo","total_oscar_score":4,"imdb_score":75.3333333333,"num_movies":3},{"actor_name":"Isabelle Adjani","total_oscar_score":4,"imdb_score":72.3333333333,"num_movies":3},{"actor_name":"Richard Harris","total_oscar_score":4,"imdb_score":54.5,"num_movies":4},{"actor_name":"Andrew Garfield","total_oscar_score":4,"imdb_score":70.5,"num_movies":8},{"actor_name":"Cary Grant","total_oscar_score":4,"imdb_score":76.2,"num_movies":5},{"actor_name":"Rachel Weisz","total_oscar_score":4,"imdb_score":65.75,"num_movies":4},{"actor_name":"Christopher Walken","total_oscar_score":4,"imdb_score":64.8,"num_movies":5},{"actor_name":"Emily Watson","total_oscar_score":4,"imdb_score":66.3333333333,"num_movies":3},{"actor_name":"Woody Harrelson","total_oscar_score":4,"imdb_score":70.0,"num_movies":7},{"actor_name":"Edward Norton","total_oscar_score":4,"imdb_score":72.375,"num_movies":8},{"actor_name":"John Travolta","total_oscar_score":4,"imdb_score":62.75,"num_movies":20},{"actor_name":"James Dean","total_oscar_score":4,"imdb_score":76.0,"num_movies":1},{"actor_name":"J.K. Simmons","total_oscar_score":4,"imdb_score":57.0,"num_movies":2},{"actor_name":"Burl Ives","total_oscar_score":3,"imdb_score":74.0,"num_movies":1},{"actor_name":"Jamie Lee Curtis","total_oscar_score":3,"imdb_score":62.3,"num_movies":10},{"actor_name":"James Woods","total_oscar_score":3,"imdb_score":68.0,"num_movies":2},{"actor_name":"James Whitmore","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Jared Leto","total_oscar_score":3,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Rooney Mara","total_oscar_score":3,"imdb_score":70.25,"num_movies":4},{"actor_name":"Margot Robbie","total_oscar_score":3,"imdb_score":55.2,"num_movies":5},{"actor_name":"Tilda Swinton","total_oscar_score":3,"imdb_score":73.3333333333,"num_movies":3},{"actor_name":"Tim Robbins","total_oscar_score":3,"imdb_score":77.3333333333,"num_movies":3},{"actor_name":"Bruce Dern","total_oscar_score":3,"imdb_score":63.5,"num_movies":2},{"actor_name":"Cloris Leachman","total_oscar_score":3,"imdb_score":53.0,"num_movies":1},{"actor_name":"Roy Scheider","total_oscar_score":3,"imdb_score":66.0,"num_movies":3},{"actor_name":"Jude Law","total_oscar_score":3,"imdb_score":67.0,"num_movies":3},{"actor_name":"Scarlett Johansson","total_oscar_score":3,"imdb_score":64.375,"num_movies":8},{"actor_name":"Sean Connery","total_oscar_score":3,"imdb_score":66.0,"num_movies":15},{"actor_name":"Judy Garland","total_oscar_score":3,"imdb_score":76.0,"num_movies":1},{"actor_name":"Catherine Zeta-Jones","total_oscar_score":3,"imdb_score":64.0,"num_movies":3},{"actor_name":"Lupita Nyong'o","total_oscar_score":3,"imdb_score":70.0,"num_movies":1},{"actor_name":"Kim Basinger","total_oscar_score":3,"imdb_score":56.0,"num_movies":1},{"actor_name":"Kevin Kline","total_oscar_score":3,"imdb_score":69.0,"num_movies":3},{"actor_name":"Angela Bassett","total_oscar_score":3,"imdb_score":50.0,"num_movies":1},{"actor_name":"Angela Lansbury","total_oscar_score":3,"imdb_score":70.0,"num_movies":1},{"actor_name":"Kenneth Branagh","total_oscar_score":3,"imdb_score":68.0,"num_movies":3},{"actor_name":"Keira Knightley","total_oscar_score":3,"imdb_score":67.8181818182,"num_movies":11},{"actor_name":"Adam Driver","total_oscar_score":3,"imdb_score":67.6666666667,"num_movies":6},{"actor_name":"John Mills","total_oscar_score":3,"imdb_score":72.0,"num_movies":1},{"actor_name":"John Hurt","total_oscar_score":3,"imdb_score":66.0,"num_movies":2},{"actor_name":"Jennifer Connelly","total_oscar_score":3,"imdb_score":60.0,"num_movies":4},{"actor_name":"Jennifer Hudson","total_oscar_score":3,"imdb_score":68.0,"num_movies":1},{"actor_name":"Anna Paquin","total_oscar_score":3,"imdb_score":73.0,"num_movies":1},{"actor_name":"Sylvester Stallone","total_oscar_score":3,"imdb_score":62.1081081081,"num_movies":37},{"actor_name":"Jeremy Renner","total_oscar_score":3,"imdb_score":64.6666666667,"num_movies":6},{"actor_name":"Sally Hawkins","total_oscar_score":3,"imdb_score":70.0,"num_movies":3},{"actor_name":"Mark Rylance","total_oscar_score":3,"imdb_score":67.3333333333,"num_movies":3},{"actor_name":"Alicia Vikander","total_oscar_score":3,"imdb_score":64.6666666667,"num_movies":3},{"actor_name":"Richard Jenkins","total_oscar_score":3,"imdb_score":55.0,"num_movies":1},{"actor_name":"Billy Bob Thornton","total_oscar_score":3,"imdb_score":63.75,"num_movies":4},{"actor_name":"Michael Fassbender","total_oscar_score":3,"imdb_score":61.6666666667,"num_movies":6},{"actor_name":"Patricia Arquette","total_oscar_score":3,"imdb_score":63.0,"num_movies":1},{"actor_name":"Melissa McCarthy","total_oscar_score":3,"imdb_score":61.0,"num_movies":8},{"actor_name":"Ralph Fiennes","total_oscar_score":3,"imdb_score":63.4444444444,"num_movies":9},{"actor_name":"Mo'Nique","total_oscar_score":3,"imdb_score":57.0,"num_movies":2},{"actor_name":"Don Ameche","total_oscar_score":3,"imdb_score":66.0,"num_movies":1},{"actor_name":"George Kennedy","total_oscar_score":3,"imdb_score":58.5,"num_movies":2},{"actor_name":"Miranda Richardson","total_oscar_score":3,"imdb_score":59.0,"num_movies":1},{"actor_name":"Mira Sorvino","total_oscar_score":3,"imdb_score":68.5,"num_movies":2},{"actor_name":"Paul Giamatti","total_oscar_score":3,"imdb_score":71.0,"num_movies":1},{"actor_name":"Martin Balsam","total_oscar_score":3,"imdb_score":85.0,"num_movies":1},{"actor_name":"Allison Janney","total_oscar_score":3,"imdb_score":67.0,"num_movies":1},{"actor_name":"Max von Sydow","total_oscar_score":3,"imdb_score":69.6666666667,"num_movies":3},{"actor_name":"Mary Steenburgen","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Tom Wilkinson","total_oscar_score":3,"imdb_score":69.0,"num_movies":1},{"actor_name":"Winona Ryder","total_oscar_score":3,"imdb_score":70.4,"num_movies":5},{"actor_name":"Ian McKellen","total_oscar_score":3,"imdb_score":71.75,"num_movies":4},{"actor_name":"Helena Bonham Carter","total_oscar_score":3,"imdb_score":72.0,"num_movies":1},{"actor_name":"Laurence Fishburne","total_oscar_score":2,"imdb_score":66.4,"num_movies":5},{"actor_name":"Sandra H\u00fcller","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Austin Butler","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Keisha Castle-Hughes","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Paul Mescal","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Ralph Richardson","total_oscar_score":2,"imdb_score":64.0,"num_movies":1},{"actor_name":"Carroll Baker","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Orson Welles","total_oscar_score":2,"imdb_score":80.0,"num_movies":1},{"actor_name":"Quvenzhan\u00e9 Wallis","total_oscar_score":2,"imdb_score":62.0,"num_movies":1},{"actor_name":"Kristen Stewart","total_oscar_score":2,"imdb_score":63.5714285714,"num_movies":14},{"actor_name":"Kristin Scott Thomas","total_oscar_score":2,"imdb_score":73.0,"num_movies":1},{"actor_name":"Lady Gaga","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Kevin Costner","total_oscar_score":2,"imdb_score":69.1875,"num_movies":16},{"actor_name":"Rosamund Pike","total_oscar_score":2,"imdb_score":64.0,"num_movies":3},{"actor_name":"Bob Hoskins","total_oscar_score":2,"imdb_score":62.3333333333,"num_movies":3},{"actor_name":"Riz Ahmed","total_oscar_score":2,"imdb_score":70.5,"num_movies":2},{"actor_name":"Anthony Franciosa","total_oscar_score":2,"imdb_score":68.0,"num_movies":1},{"actor_name":"Bryan Cranston","total_oscar_score":2,"imdb_score":73.3333333333,"num_movies":6},{"actor_name":"Melina Mercouri","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Melanie Griffith","total_oscar_score":2,"imdb_score":65.0,"num_movies":2},{"actor_name":"Margaret Sullavan","total_oscar_score":2,"imdb_score":83.0,"num_movies":1},{"actor_name":"Robert Redford","total_oscar_score":2,"imdb_score":67.4,"num_movies":10},{"actor_name":"Sharon Stone","total_oscar_score":2,"imdb_score":56.6666666667,"num_movies":3},{"actor_name":"Michael Keaton","total_oscar_score":2,"imdb_score":67.0,"num_movies":6},{"actor_name":"Antonio Banderas","total_oscar_score":2,"imdb_score":66.1666666667,"num_movies":18},{"actor_name":"Sam Waterston","total_oscar_score":2,"imdb_score":75.0,"num_movies":1},{"actor_name":"Salma Hayek","total_oscar_score":2,"imdb_score":64.3333333333,"num_movies":3},{"actor_name":"Liam Neeson","total_oscar_score":2,"imdb_score":65.1153846154,"num_movies":26},{"actor_name":"Michael Shannon","total_oscar_score":2,"imdb_score":63.5,"num_movies":2},{"actor_name":"Ryan O'Neal","total_oscar_score":2,"imdb_score":80.0,"num_movies":1},{"actor_name":"Mickey Rourke","total_oscar_score":2,"imdb_score":65.8333333333,"num_movies":6},{"actor_name":"Bill Murray","total_oscar_score":2,"imdb_score":68.4615384615,"num_movies":13},{"actor_name":"Bill Nighy","total_oscar_score":2,"imdb_score":66.5,"num_movies":4},{"actor_name":"Felicity Jones","total_oscar_score":2,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"James Franco","total_oscar_score":2,"imdb_score":60.7,"num_movies":10},{"actor_name":"James Garner","total_oscar_score":2,"imdb_score":70.0,"num_movies":1},{"actor_name":"Topol","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Terrence Howard","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Diane Lane","total_oscar_score":2,"imdb_score":66.8,"num_movies":5},{"actor_name":"Tony Curtis","total_oscar_score":2,"imdb_score":74.5,"num_movies":2},{"actor_name":"Don Cheadle","total_oscar_score":2,"imdb_score":70.5,"num_movies":2},{"actor_name":"Ali MacGraw","total_oscar_score":2,"imdb_score":68.0,"num_movies":1},{"actor_name":"Jeffrey Wright","total_oscar_score":2,"imdb_score":54.0,"num_movies":1},{"actor_name":"Harrison Ford","total_oscar_score":2,"imdb_score":63.5263157895,"num_movies":19},{"actor_name":"Jesse Eisenberg","total_oscar_score":2,"imdb_score":65.75,"num_movies":8},{"actor_name":"Hugh Jackman","total_oscar_score":2,"imdb_score":69.8235294118,"num_movies":17},{"actor_name":"Cynthia Erivo","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Catalina Sandino Moreno","total_oscar_score":2,"imdb_score":72.0,"num_movies":1},{"actor_name":"Gene Kelly","total_oscar_score":2,"imdb_score":82.0,"num_movies":1},{"actor_name":"Imelda Staunton","total_oscar_score":2,"imdb_score":73.0,"num_movies":1},{"actor_name":"Gabourey Sidibe","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Isabelle Huppert","total_oscar_score":2,"imdb_score":59.6666666667,"num_movies":3},{"actor_name":"Colin Farrell","total_oscar_score":2,"imdb_score":65.1111111111,"num_movies":18},{"actor_name":"Timoth\u00e9e Chalamet","total_oscar_score":2,"imdb_score":73.75,"num_movies":4},{"actor_name":"Gary Busey","total_oscar_score":2,"imdb_score":66.0,"num_movies":1},{"actor_name":"Tom Hulce","total_oscar_score":2,"imdb_score":62.5,"num_movies":2},{"actor_name":"Demi\u00e1n Bichir","total_oscar_score":2,"imdb_score":68.5,"num_movies":2},{"actor_name":"Ethan Hawke","total_oscar_score":2,"imdb_score":66.5625,"num_movies":16},{"actor_name":"Dudley Moore","total_oscar_score":2,"imdb_score":58.75,"num_movies":4},{"actor_name":"Elisabeth Shue","total_oscar_score":2,"imdb_score":61.5,"num_movies":2},{"actor_name":"Jonah Hill","total_oscar_score":2,"imdb_score":67.1666666667,"num_movies":6},{"actor_name":"Jonathan Pryce","total_oscar_score":2,"imdb_score":76.0,"num_movies":2},{"actor_name":"Peter Fonda","total_oscar_score":2,"imdb_score":71.0,"num_movies":1},{"actor_name":"Chadwick Boseman","total_oscar_score":2,"imdb_score":71.5,"num_movies":4},{"actor_name":"Celia Johnson","total_oscar_score":2,"imdb_score":78.0,"num_movies":1},{"actor_name":"Andrea Riseborough","total_oscar_score":2,"imdb_score":61.25,"num_movies":4},{"actor_name":"Catherine Keener","total_oscar_score":2,"imdb_score":74.0,"num_movies":1},{"actor_name":"Edward James Olmos","total_oscar_score":2,"imdb_score":75.0,"num_movies":2},{"actor_name":"Yalitza Aparicio","total_oscar_score":2,"imdb_score":77.0,"num_movies":1},{"actor_name":"Catherine Deneuve","total_oscar_score":2,"imdb_score":72.4,"num_movies":5},{"actor_name":"Eddie Albert","total_oscar_score":2,"imdb_score":76.0,"num_movies":1},{"actor_name":"Kathleen Turner","total_oscar_score":2,"imdb_score":58.0,"num_movies":3},{"actor_name":"John Lithgow","total_oscar_score":2,"imdb_score":61.0,"num_movies":1},{"actor_name":"Woody Allen","total_oscar_score":2,"imdb_score":66.1666666667,"num_movies":6},{"actor_name":"Chiwetel Ejiofor","total_oscar_score":2,"imdb_score":70.0,"num_movies":4},{"actor_name":"Ana de Armas","total_oscar_score":2,"imdb_score":31.0,"num_movies":2},{"actor_name":"Vanessa Kirby","total_oscar_score":2,"imdb_score":63.0,"num_movies":2},{"actor_name":"Steven Yeun","total_oscar_score":2,"imdb_score":68.5,"num_movies":2},{"actor_name":"Steve McQueen","total_oscar_score":2,"imdb_score":73.3333333333,"num_movies":6},{"actor_name":"Charlotte Rampling","total_oscar_score":2,"imdb_score":65.0,"num_movies":1},{"actor_name":"Steve Carell","total_oscar_score":2,"imdb_score":64.6666666667,"num_movies":12},{"actor_name":"Stephen Rea","total_oscar_score":2,"imdb_score":69.0,"num_movies":1},{"actor_name":"Virginia Madsen","total_oscar_score":1,"imdb_score":64.0,"num_movies":2},{"actor_name":"Richard Widmark","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"William H. Macy","total_oscar_score":1,"imdb_score":58.0,"num_movies":1},{"actor_name":"Queen Latifah","total_oscar_score":1,"imdb_score":63.6,"num_movies":5},{"actor_name":"Rachel McAdams","total_oscar_score":1,"imdb_score":69.5,"num_movies":4},{"actor_name":"Adriana Barraza","total_oscar_score":1,"imdb_score":49.0,"num_movies":1},{"actor_name":"Albert Brooks","total_oscar_score":1,"imdb_score":74.0,"num_movies":2},{"actor_name":"Alec Baldwin","total_oscar_score":1,"imdb_score":63.8333333333,"num_movies":6},{"actor_name":"Amy Ryan","total_oscar_score":1,"imdb_score":61.0,"num_movies":1},{"actor_name":"Anna Kendrick","total_oscar_score":1,"imdb_score":66.6363636364,"num_movies":11},{"actor_name":"Sacha Baron Cohen","total_oscar_score":1,"imdb_score":61.6666666667,"num_movies":6},{"actor_name":"Taraji P. Henson","total_oscar_score":1,"imdb_score":56.5,"num_movies":6},{"actor_name":"Vera Farmiga","total_oscar_score":1,"imdb_score":67.0,"num_movies":2},{"actor_name":"Stanley Tucci","total_oscar_score":1,"imdb_score":60.0,"num_movies":1},{"actor_name":"Samuel L. Jackson","total_oscar_score":1,"imdb_score":63.5,"num_movies":18},{"actor_name":"Sam Shepard","total_oscar_score":1,"imdb_score":74.0,"num_movies":1},{"actor_name":"Terence Stamp","total_oscar_score":1,"imdb_score":68.0,"num_movies":1},{"actor_name":"Tom Hardy","total_oscar_score":1,"imdb_score":67.3636363636,"num_movies":11},{"actor_name":"Toni Collette","total_oscar_score":1,"imdb_score":54.4,"num_movies":5},{"actor_name":"Robert Mitchum","total_oscar_score":1,"imdb_score":76.0,"num_movies":1},{"actor_name":"Uma Thurman","total_oscar_score":1,"imdb_score":64.3333333333,"num_movies":6},{"actor_name":"Anthony Perkins","total_oscar_score":1,"imdb_score":70.0,"num_movies":2},{"actor_name":"River Phoenix","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Amanda Seyfried","total_oscar_score":1,"imdb_score":65.3,"num_movies":10},{"actor_name":"Thomas Haden Church","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Tim Roth","total_oscar_score":1,"imdb_score":70.5,"num_movies":2},{"actor_name":"Tom Berenger","total_oscar_score":1,"imdb_score":60.125,"num_movies":8},{"actor_name":"Peter Firth","total_oscar_score":1,"imdb_score":61.0,"num_movies":1},{"actor_name":"Jennifer Tilly","total_oscar_score":1,"imdb_score":66.0,"num_movies":2},{"actor_name":"Chris Sarandon","total_oscar_score":1,"imdb_score":74.0,"num_movies":2},{"actor_name":"Jennifer Jason Leigh","total_oscar_score":1,"imdb_score":60.5,"num_movies":2},{"actor_name":"James Cromwell","total_oscar_score":1,"imdb_score":59.0,"num_movies":1},{"actor_name":"Clive Owen","total_oscar_score":1,"imdb_score":63.0,"num_movies":8},{"actor_name":"James Caan","total_oscar_score":1,"imdb_score":78.0,"num_movies":1},{"actor_name":"Jake Gyllenhaal","total_oscar_score":1,"imdb_score":65.5882352941,"num_movies":17},{"actor_name":"Jackie Earle Haley","total_oscar_score":1,"imdb_score":55.0,"num_movies":2},{"actor_name":"Jesse Plemons","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Kate Hudson","total_oscar_score":1,"imdb_score":65.0,"num_movies":3},{"actor_name":"Josh Brolin","total_oscar_score":1,"imdb_score":64.0,"num_movies":6},{"actor_name":"Jessie Buckley","total_oscar_score":1,"imdb_score":63.0,"num_movies":1},{"actor_name":"John C. Reilly","total_oscar_score":1,"imdb_score":69.75,"num_movies":4},{"actor_name":"Chazz Palminteri","total_oscar_score":1,"imdb_score":57.0,"num_movies":1},{"actor_name":"Gene Wilder","total_oscar_score":1,"imdb_score":70.75,"num_movies":4},{"actor_name":"Gary Sinise","total_oscar_score":1,"imdb_score":60.0,"num_movies":1},{"actor_name":"Florence Pugh","total_oscar_score":1,"imdb_score":68.6,"num_movies":5},{"actor_name":"Dennis Hopper","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Emily Blunt","total_oscar_score":1,"imdb_score":68.0,"num_movies":5},{"actor_name":"Elliott Gould","total_oscar_score":1,"imdb_score":62.0,"num_movies":2},{"actor_name":"Eddie Murphy","total_oscar_score":1,"imdb_score":59.95,"num_movies":20},{"actor_name":"Dev Patel","total_oscar_score":1,"imdb_score":73.0,"num_movies":5},{"actor_name":"Hume Cronyn","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"Hailee Steinfeld","total_oscar_score":1,"imdb_score":69.5,"num_movies":2},{"actor_name":"Dan Aykroyd","total_oscar_score":1,"imdb_score":61.5,"num_movies":8},{"actor_name":"Harvey Keitel","total_oscar_score":1,"imdb_score":72.3333333333,"num_movies":3},{"actor_name":"Haley Joel Osment","total_oscar_score":1,"imdb_score":64.5,"num_movies":2},{"actor_name":"Hal Holbrook","total_oscar_score":1,"imdb_score":69.0,"num_movies":1},{"actor_name":"Greg Kinnear","total_oscar_score":1,"imdb_score":72.6666666667,"num_movies":3},{"actor_name":"Danny Aiello","total_oscar_score":1,"imdb_score":78.0,"num_movies":1},{"actor_name":"Brad Dourif","total_oscar_score":1,"imdb_score":54.0,"num_movies":1},{"actor_name":"Matt Dillon","total_oscar_score":1,"imdb_score":66.0,"num_movies":3},{"actor_name":"Mary J. Blige","total_oscar_score":1,"imdb_score":63.0,"num_movies":1},{"actor_name":"Naomie Harris","total_oscar_score":1,"imdb_score":68.0,"num_movies":1},{"actor_name":"Barbara Hershey","total_oscar_score":1,"imdb_score":66.0,"num_movies":1},{"actor_name":"Omar Sharif","total_oscar_score":1,"imdb_score":76.0,"num_movies":1},{"actor_name":"Lesley Manville","total_oscar_score":1,"imdb_score":73.0,"num_movies":1},{"actor_name":"Lily Tomlin","total_oscar_score":1,"imdb_score":71.0,"num_movies":1},{"actor_name":"Lauren Bacall","total_oscar_score":1,"imdb_score":69.0,"num_movies":1},{"actor_name":"Lakeith Stanfield","total_oscar_score":1,"imdb_score":71.0,"num_movies":2},{"actor_name":"Kodi Smit-McPhee","total_oscar_score":1,"imdb_score":64.0,"num_movies":4},{"actor_name":"Klaus Maria Brandauer","total_oscar_score":1,"imdb_score":67.0,"num_movies":1},{"actor_name":"Kirsten Dunst","total_oscar_score":1,"imdb_score":63.0,"num_movies":8},{"actor_name":"Ken Watanabe","total_oscar_score":1,"imdb_score":75.0,"num_movies":1},{"actor_name":"Lillian Gish","total_oscar_score":1,"imdb_score":61.0,"num_movies":2},{"actor_name":"Linda Blair","total_oscar_score":1,"imdb_score":51.5,"num_movies":2},{"actor_name":"Mark Wahlberg","total_oscar_score":1,"imdb_score":64.9285714286,"num_movies":28},{"actor_name":"Marianne Jean-Baptiste","total_oscar_score":1,"imdb_score":57.0,"num_movies":1},{"actor_name":"Maggie Gyllenhaal","total_oscar_score":1,"imdb_score":67.5,"num_movies":2},{"actor_name":"Burt Reynolds","total_oscar_score":1,"imdb_score":63.0,"num_movies":5},{"actor_name":"Abigail Breslin","total_oscar_score":1,"imdb_score":61.0,"num_movies":4}];


document.getElementById('actor-search').addEventListener('input', function() {
    const query = this.value.toLowerCase();
    const resultContainer = document.getElementById('result');
    resultContainer.innerHTML = '';

    if (query === '') return;

    const foundActor = actors.find(actor => actor.actor_name.toLowerCase().includes(query));
    if (foundActor) {
        resultContainer.innerHTML = `
            <h3>🧬 ${foundActor.actor_name}</h3>
            <p>🧪 IMDb Score: ${foundActor.imdb_score}</p>
            <p>🏆 Oscar Score: ${foundActor.total_oscar_score}</p>
            <p>🎥 Number of Movies: ${foundActor.num_movies}</p>
        `;
    } else {
        resultContainer.innerHTML = `<p style="color:red;">❌ Actor "${this.value}" not found.</p>`;
    }
});
</script>

{% endraw %}

Did you find what you were looking for? If not, try searching for another actor!
Our comprehensive database contains information on a wide range of actors, so you're sure to find what you need. 
Happy searching and we wish you alot of success for your movie, after knowing the secret formula, you are basically guaranteed an oscar! 🎬


<!-- BUDGET -->
# Money: budget, revenue, scores...

##  Box Office vs Average Score
Does a higher budget imply better revenue, ratings, and overall movie success? At first glance, it seems logical—like adding more ingredients to a chemical formula to achieve a better reaction. However, as revealed in our second milestone, the relationship between financial investment and movie performance is far more complex. To untangle this, we use two tools from our statistical lab: <i>Pearson and Spearman correlations</i>, which allow us to examine both linear trends and rank-based patterns.

What do these tools measure?

- Pearson Correlation tests the linear relationship between two variables—like how two chemicals react in perfect proportion.
- Spearman Correlation looks at the rank of the data, revealing patterns even when the relationship isn’t perfectly straight, making it robust against outliers or unexpected reactions.


By testing both, we uncover not just visible trends but hidden relationships, helping us determine whether budget truly acts as a key catalyst for a movie’s success. More speciically, if Spearman is higher than Pearson, it indicates a stronger rank-based relationship, suggesting subtle, non-linear patterns. Conversely, if Pearson is higher, the variables exhibit a clearer linear trend.


## Budget vs Revenue 

- Pearson Correlation: 0.769
- Spearman Correlation: 0.71

The results are clear! Budget and revenue show a strong positive relationship, much like increasing the concentration of a reagent leading to a stronger chemical yield. A higher budget enables grander production, wider distribution, and aggressive marketing—ingredients that often boost box office earnings.

<strong>But here’s the catch:</strong> revenue isn’t profit. Just as an expensive experiment might fail to yield a breakthrough, movies with massive budgets can still flop if audience reception or competition acts as a limiting factor.

## Budget vs Average Movie Score 

- Pearson Correlation: 0.097
- Spearman Correlation: 0.15

The weak correlations suggest that budget has minimal influence on movie quality, much like adding more expensive compounds without improving the end result. A big budget might produce spectacle, but it doesn’t necessarily generate a masterpiece. It’s like a flashy formula that works but doesn’t impress the critics—expensive, but ineffective at winning accolades. 

## Revenue vs Average Movie Score

- Pearson Correlation: 0.112
- Spearman Correlation: 0.21

Here, we observe a faint positive relationship—like a slight chemical reaction occurring under ideal conditions. Successful movies in terms of revenue sometimes rank higher in scores, but the relationship is weak. This suggests that external factors, such as hype, franchises, or marketing, often drive revenue more than the movie’s artistic or critical merit.

## Box Office vs Average Movie Score

- Pearson Correlation: 0.183
- Spearman Correlation: 0.12 

The results show a mild connection... box office performance and movie scores occasionally align, but not reliably. It’s like a formula producing small sparks of success—a positive reaction, but not a breakthrough. While higher scores can nudge box office earnings upward, other variables, like star power and audience trends, dominate the equation.

Success, much like in the lab, is not defined by a single variable. A blockbuster might generate impressive box office "yields" without winning critical acclaim, while a critically adored film may struggle financially. True success lies in understanding this multi-dimensional formula—balancing financial outcomes, audience reactions, and artistic merit. For this reason, let us continue our analysis even further...



<!-- TIMING -->
# Release Date Timing

The timing of a movie's release is like a catalyst in the formula for its success. Seasons like summer and the holidays often act as "peak reaction times," driving higher box office earnings and audience engagement. This raises the hypothesis: Do films released during these periods consistently perform better?

In this cinematic experiment, we’ll analyze how seasonality—whether during the "blockbuster summer," "holiday warmth," or "off-peak chill"—affects box office performance and ratings. By isolating these variables, we aim to uncover patterns explaining how timing influences a film's success.onal effects on a film's overall success.

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>


The fall season steals the spotlight, boasting the highest number of movie releases compared to any other season. It's the industry's blockbuster breeding ground!
<div id="movies_count_graph" style="height: 400px;"></div>

Sheer quantity isn't everything. Let's dive deeper—how does the revenue stack up across seasons.

<div id="revenue_graph" style="height: 400px;"></div>

Let's focus on the heartbeat of the film industry: ticket sales, Box office numbers.
<div id="box_office_graph" style="height: 400px;"></div>

Now, let’s break it down even further. How does movie revenue fluctuate week by week throughout the year. I would like to call that "the Christmas peak".

<div id="weekly_revenue_graph" style="height: 400px;"></div>

In summary, while fall sees the most releases, summer dominates revenue, highlighting the impact of timing. Weekly trends further show how holidays and key moments drive audience engagement and industry success.

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
    title: 'Weekly Movie Revenue per week through out the year',
    xaxis: { title: 'Week of the Year' },
    yaxis: { title: 'Average Revenue (in USD)' },
  };

  // Create the plots with specific layouts
  Plotly.newPlot('movies_count_graph', [movies_count_trace], movies_count_layout);
  Plotly.newPlot('revenue_graph', [revenue_trace], revenue_layout);
  Plotly.newPlot('box_office_graph', [box_office_trace], box_office_layout);
  Plotly.newPlot('weekly_revenue_graph', [weekly_revenue_trace], weekly_revenue_layout);

</script>

<!-- COUNTRY -->
# Production Country

Know that we know that budget and [insert previous section names] arent'the only factors that contribute to a movie's success, let us take a look at the global landscape of the movie industry. 

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
                    text: 'Value'
                },
                beginAtZero: true
            }
        }
    }
});
</script>

These results can be explained by the unique strengths and historical factors that shape each country’s film industry, almost like the specific conditions in a lab experiment that determine the outcome:

- United States: Hollywood is the powerhouse of global cinema, driven by its extensive infrastructure, immense funding, and deep ties to international markets. Think of Hollywood as a well-oiled machine that churns out films on a massive scale, ensuring widespread distribution and constant success across the globe. Like a carefully calibrated experiment, Hollywood’s reach and accessibility make U.S. films a dominant force in international box offices.

- India: Bollywood, the world’s largest film industry by output, thrives on the vast, culturally diverse audience of India. With a focus on genres like musicals and romantic dramas, Bollywood taps into the unique tastes of its domestic market, much like a perfect blend of ingredients that produces a high volume of films with deep local resonance. The sheer demand from its massive population fuels this powerhouse, making Bollywood a global contender.

- United Kingdom, France, and Italy: These countries have long-standing film traditions, backed by government support and policies that protect and promote local cinema. France, for example, with its historic roots in the early days of cinema, provides an environment where films are carefully nurtured to achieve both high quality and international recognition. This careful, supported approach allows for a strong domestic output that often garners critical acclaim on the global stage.


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

Not every experiment can yield perfect results. It’s important to note that the CMU Movies dataset contains a significant amount of missing data, particularly in the box office revenue field. These gaps in the data could influence the outcomes of our analysis, as the absence of information from certain movies or countries may skew the overall revenue figures. As a result, we observe a trend that largely reflects the movie count by country, with the USA and UK occupying the top spots. However, a key anomaly emerges in the form of Bollywood’s financial contribution, which is noticeably absent. This is due to the dataset’s lack of box office revenue data for many Indian films, creating a gap in the analysis that affects our global picture of film industry performance.


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


English takes a commanding lead in this dataset for two primary reasons. First, American films make up the largest portion of the dataset, which naturally skews the language distribution toward English. Hollywood’s global influence, with its high output of internationally distributed films, contributes significantly to this dominance.

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




<!-- TROPES -->
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
      b: 150 // Increase bottom margin to give more space for x-axis labels
    },
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
      b: 150 // Increase bottom margin to give more space for x-axis labels
    },
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

## Which trope is the best?
Now that we have visualized which tropes score best in each category, let us propose which trope could be the best predictor for a successful movie! Firstly, we make an observation that there is no overlap between the top 5 tropes ranked by Box Office revenue and the IMDb top 5. Similarly, no overlap between Box Office top 5 and Metacritic top 5. However, the Metacritic and IMDb top 5 share multiple tropes, namely: *Stupid Crooks*, *Doormat*, *Morally bankrupt banker*. Notice that all of these characters show silly or negative traits. It seems there is something people enjoy about these characteristics, perhaps they make us laugh. 

However, the Box Office top 5 include tropes such as: *Gadgeteer Genius* and *Child Prodigy*. These only ranked average in IMDb and Metacritic, but performed exceptionally well in the Box Office. Is it envy which makes us admire these characters, but only silently without ranking them well? Or are they not ranked well simply because it is a cliché? We would need a more thorough analysis to uncover the reasons, but the trend seems to point this way.

Finally, we conclude that the choice of the tropes ultimately comes down to what the movie is trying to optimize for. If it is Box Office revenue - make a movie about a genius! If audience or critics reception is the goal - a silly character which provides a good laugh could be a better choice.


## Remarks about the data used
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
    template: 'plotly_white'
  };

  Plotly.newPlot('genderDistributionPie', pieData, pieLayout);
</script>