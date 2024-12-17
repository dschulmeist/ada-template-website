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






