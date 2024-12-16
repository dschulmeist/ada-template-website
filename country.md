# Production Country

In this section, we delve into the global landscape of the movie industry, examining key trends in film production, box office performance, language distribution, and genre preferences across the top 20 most influential countries to highlight the regions with the greatest impact and activity in the global film industry.

As mentioned previously, the Box Office Revenue presents a significant challenge, with a large portion of data being null (73,340 missing entries). While we could attempt to use machine learning/data science techniques to predict the missing values, the nature of Box Office Revenue makes it difficult to fill in reliably. This metric is highly dependent on various factors such as distribution, marketing, and cultural context, which aren't easily inferred or predicted with standard data imputation methods. 

Additionally, our helper datasets differ significantly in size (with a maximum of 10k entries compared to over 80k in this dataset), and they do not significantly reduce the number of null values. Therefore, when analyzing box office revenue, we will focus only on the rows where this data is available. We will handle missing data appropriately, without attempting to impute values, as doing so could introduce significant bias or inaccuracies.


<canvas id="countryBarChart" width="400" height="200"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
const ctx = document.getElementById('countryBarChart').getContext('2d');
const countryBarChart = new Chart(ctx, {
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
<table border="1" cellpadding="5" cellspacing="0">
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
            <td>World </td>
        </tr>
    </tbody>
</table>
