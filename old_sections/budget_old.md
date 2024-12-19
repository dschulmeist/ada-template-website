<h2>Box Office vs Average Score</h2>

<p>In this section, we will explore an important question: Does a higher budget imply better revenue and ratings, and therefore, greater movie success? While it might seem intuitive that a larger budget would lead to greater success, the relationship between a movie's financial investment and its performance is more complex. We'll investigate whether bigger budgets truly correlate with higher box office earnings and better ratings, or if other factors play a more significant role in determining a movie's success. By understanding this connection, we can gain insights into the financial dynamics of the film industry.</p>

<p>To begin our exploration, we will first create a master ratings table by combining the ratings data from multiple sources—Metacritic, IMDb, and Rotten Tomatoes. This will give us a comprehensive view of how a movie is received across different platforms. Additionally, we will gather the budget and revenue information from the IMDb table, as it contains the most reliable financial data for our analysis. By combining these datasets, we aim to better understand whether movies with higher budgets tend to perform better in terms of both revenue and ratings, providing a clearer picture of what factors contribute to a film's success.</p>

<script src="https://cdn.jsdelivr.net/npm/plotly.js-dist@2.11.1/plotly.min.js"></script>

<div id="chart1"></div> 

<div id="chart2"></div>

<div id="chart3"></div> 

<div id="chart4"></div> 

<script>
  // Fetching and processing the data
  fetch('/json_data/budget_section.json')
    .then(response => response.text())
    .then(text => {
      const validData = [];
      text.split('\n').forEach(line => {
        try {
          const parsedLine = JSON.parse(line.trim());
          if (
            parsedLine.avg_score != null &&
            parsedLine.box_office != null &&
            parsedLine.movie_name != null
          ) {
            parsedLine.avg_score = Math.round(parsedLine.avg_score * 10) / 10;
            parsedLine.box_office = Math.round(parsedLine.box_office * 10) / 10;
            parsedLine.revenue = Math.round(parsedLine.revenue * 10) / 10;
            parsedLine.budget = Math.round(parsedLine.budget * 10) / 10;
            validData.push(parsedLine);
          }
        } catch (error) {
          console.error('Error parsing line:', line, error);
        }
      });

      
      createBoxOfficePlot(validData);
      createRevenuePlot(validData);
      createBudgetPlot(validData);
      createBudgetVsRevenuePlot(validData);
    })
    .catch(error => console.error('Error loading the data:', error));

  // Box Office vs Average Score
  function createBoxOfficePlot(data) {
    const avg_score = data.map(row => row.avg_score);
    const box_office = data.map(row => row.box_office);
    const movie_names = data.map(row => row.movie_name);

    const trace = {
      x: avg_score,
      y: box_office,
      mode: 'markers',
      text: movie_names,
      hovertemplate:
        '<b>Movie:</b> %{text}<br>' +
        '<b>Average Score:</b> %{x}<br>' +
        '<b>Box Office:</b> $%{y}<br>' +
        '<extra></extra>',
    };

    const layout = {
      title: 'Box Office vs Average Score',
      xaxis: { title: 'Average Score' },
      yaxis: { title: 'Box Office (USD)' },
    };

    Plotly.newPlot('chart1', [trace], layout);
  }

  // Revenue vs Average Score
  function createRevenuePlot(data) {
    const avg_score = data.map(row => row.avg_score);
    const revenue = data.map(row => row.revenue);
    const movie_names = data.map(row => row.movie_name);

    const trace = {
      x: avg_score,
      y: revenue,
      mode: 'markers',
      text: movie_names,
      hovertemplate:
        '<b>Movie:</b> %{text}<br>' +
        '<b>Average Score:</b> %{x}<br>' +
        '<b>Revenue:</b> $%{y}<br>' +
        '<extra></extra>',
    };

    const layout = {
      title: 'Revenue vs Average Score',
      xaxis: { title: 'Average Score' },
      yaxis: { title: 'Revenue (USD)' },
    };

    Plotly.newPlot('chart2', [trace], layout);
  }

  //Budget vs Average Score
  function createBudgetPlot(data) {
    const avg_score = data.map(row => row.avg_score);
    const budget = data.map(row => row.budget);
    const movie_names = data.map(row => row.movie_name);

    const trace = {
      x: avg_score,
      y: budget,
      mode: 'markers',
      text: movie_names,
      hovertemplate:
        '<b>Movie:</b> %{text}<br>' +
        '<b>Average Score:</b> %{x}<br>' +
        '<b>Budget:</b> $%{y}<br>' +
        '<extra></extra>',
    };

    const layout = {
      title: 'Budget vs Average Score',
      xaxis: { title: 'Average Score' },
      yaxis: { title: 'Budget (USD)' },
    };

    Plotly.newPlot('chart3', [trace], layout);
  }

  //Budget vs Revenue
  function createBudgetVsRevenuePlot(data) {
    const budget = data.map(row => row.budget);
    const revenue = data.map(row => row.revenue);
    const movie_names = data.map(row => row.movie_name);

    const trace = {
      x: budget,
      y: revenue,
      mode: 'markers',
      text: movie_names,
      hovertemplate:
        '<b>Movie:</b> %{text}<br>' +
        '<b>Budget:</b> %{x}<br>' +
        '<b>Revenue:</b> $%{y}<br>' +
        '<extra></extra>',
    };

    const layout = {
      title: 'Budget vs Revenue',
      xaxis: { title: 'Budget (USD)' },
      yaxis: { title: 'Revenue (USD)' },
    };

    Plotly.newPlot('chart4', [trace], layout);
  }
</script>

<h2>Section Conclusion</h2>
<p>In conclusion, while budget, revenue, and box office numbers are important indicators of a film's commercial success, they do not always correlate with the critical or audience reception, as reflected in average scores. A movie can perform well financially despite low ratings, or struggle at the box office despite strong reviews, highlighting that factors such as effective marketing, star power, release timing, and cultural trends play significant roles in a movie's success. The complexity of movie success underscores that while financial metrics and ratings provide useful insights, they alone cannot predict the full impact a film may have.</p>
