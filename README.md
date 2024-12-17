# Lights, Camera, Success: What Makes a Movie Shine at the Box Office?

Welcome to the Movie Lab, where we view movies as complex chemical reactions—blending various elements that can either explode into blockbuster success or fizzle into obscurity!

In the Blue Sweater lab, we dive deep into the science behind cinematic hits. From budgets and genres to audience ratings and box office numbers, each ingredient plays a crucial role in a movie's formula for success. Our goal: to uncover the secret ingredients that influence a movie’s journey to the top.

We'll break down each ingredient—ratings, actors, Oscar wins, and more—examining how they work individually and together to create the perfect cinematic "potion."

By approaching movies through a data-driven lens, we aim to decode the magic behind storytelling and reveal what resonates with audiences. This knowledge could even help filmmakers craft the next cultural phenomenon!

## Meet Our Scientists 

<div style="display: flex; gap: 15px; justify-content: center;">
    <div style="text-align: center;">
        <img src="scientists_pics/ivan.webp" alt="Ivan" style="width: 160px; height: auto;"/>
        <p style="margin-bottom: 5px;">Ivan</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in MTE</p>
    </div>
    <div style="text-align: center;">
        <img src="scientists_pics/david.webp" alt="David" style="width: 160px; height: auto;"/>
        <p style="margin-bottom: 5px;">David</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in CS</p>
    </div>
    <div style="text-align: center;">
        <img src="scientists_pics/adam.webp" alt="Adam" style="width: 160px; height: auto;"/>
        <p style="margin-bottom: 5px;">Adam</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in CS</p>
    </div>
    <div style="text-align: center;">
        <img src="scientists_pics/ali.webp" alt="Ali" style="width: 160px; height: auto;"/>
        <p style="margin-bottom: 5px;">Ali</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in EEE</p>
    </div>
    <div style="text-align: center;">
        <img src="scientists_pics/dana.webp" alt="Dana" style="width: 160px; height: auto;"/>
        <p style="margin-bottom: 5px;">Dana</p>
        <p style="margin-top: 0; margin-bottom: 0;">MSc in NX</p>
    </div>
</div>



<br>
<br>

## Let’s Derive Our Ingredients...

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

## Addressing Key Issues with Our Dataset

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
