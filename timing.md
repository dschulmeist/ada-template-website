# Release Date Timing

The timing of a movie's release plays a crucial role in its commercial performance and critical reception. Certain seasons, such as summer and the holiday period, are traditionally seen as more favorable for movie releases, with the potential for higher box office earnings and increased audience engagement. The question arises: Do movies released during these peak times tend to earn more revenue or receive better ratings from critics and audiences? 

In this section, we will explore the impact of release dates on a movie's success, focusing on how seasonality—whether movies are released in summer, during the holiday season, or in off-peak months—can influence box office performance and ratings. By analyzing trends in movie releases and comparing the performance of films across different seasons, we aim to uncover patterns that may explain the seasonal effects on a film's overall success.

<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>

<div id="movies_count_graph" style="height: 400px;"></div>
<div id="avg_score_graph" style="height: 400px;"></div>
<div id="revenue_graph" style="height: 400px;"></div>
<div id="box_office_graph" style="height: 400px;"></div>
<div id="weekly_revenue_graph" style="height: 400px;"></div>

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
    marker: { color: 'skyblue' },
  };

  // Average Movie Score for Each Season
  var avg_scores_trace = {
    x: seasons,
    y: avg_scores,
    type: 'bar',
    marker: { color: 'lightgreen' },
  };

  // Revenue by Each Season
  var revenue_trace = {
    x: seasons,
    y: revenue,
    type: 'bar',
    marker: { color: 'orange' },
  };

  // Average Box Office for Each Season
  var box_office_trace = {
    x: seasons,
    y: box_office,
    type: 'bar',
    marker: { color: 'salmon' },
  };

  // Weekly Movie Revenue Line Plot
  var weekly_revenue_trace = {
    x: weeks,
    y: weekly_revenue,
    mode: 'lines+markers',
    line: { color: 'blue', shape: 'spline' },
    name: 'Weekly Revenue',
  };

  // Layout for each graph
  var movies_count_layout = {
    title: 'Number of Movies in Each Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Number of Movies' },
  };

  var avg_scores_layout = {
    title: 'Average Movie Score for Each Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Average Score' },
  };

  var revenue_layout = {
    title: 'Revenue by Each Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Revenue (in USD)' },
  };

  var box_office_layout = {
    title: 'Average Box Office for Each Season',
    xaxis: { title: 'Season' },
    yaxis: { title: 'Box Office (in USD)' },
  };

  var weekly_revenue_layout = {
    title: 'Weekly Movie Revenue for Each Week of the Year',
    xaxis: { title: 'Week of the Year' },
    yaxis: { title: 'Average Revenue (in USD)' },
  };

  // Create the plots with specific layouts
  Plotly.newPlot('movies_count_graph', [movies_count_trace], movies_count_layout);
  Plotly.newPlot('avg_score_graph', [avg_scores_trace], avg_scores_layout);
  Plotly.newPlot('revenue_graph', [revenue_trace], revenue_layout);
  Plotly.newPlot('box_office_graph', [box_office_trace], box_office_layout);
  Plotly.newPlot('weekly_revenue_graph', [weekly_revenue_trace], weekly_revenue_layout);
</script>
