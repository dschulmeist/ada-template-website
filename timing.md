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
  // Data for each dataset
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
    text: movies_count,  // Add the numbers as labels
    textposition: 'auto',  // Position the labels automatically on the bars
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
