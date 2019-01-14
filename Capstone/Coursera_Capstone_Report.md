     
  <div id="readme" class="readme blob instapaper_body js-code-block-container">
    <article class="markdown-body entry-content" itemprop="text"><h2><a id="user-content-simplifying-your-house-search-in-london" class="anchor" aria-hidden="true" href="#Searching houses available in london"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Searching houses available in london</h2>
<h3><a id="user-content-introduction-business-problem" class="anchor" aria-hidden="true" href="#introduction-business-problem"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Introduction/ Business Problem</h3>
<p>It's very challenging to find a house in London, UK. There are many websites out there that try to help you out with searching your house like Zoopla, Rightmove. But even then narrowing down area to look for is a manual task that these websites do not automate/provide. Narrowing down area can be very tedious and time consuming process.</p>
<p>This project attempts to help you with identifying or narrowing down the search for the house according to your preferences about the neighborhood.
For example, one might want to look at all areas which has proximity to pubs, cafes, public transport etc. This solution attempts cluster areas based on your provided preferences/categories. We then print those clusters on the map to distinguish between areas. That should make it easy to choose a certain areas to look for and narrow down your search.</p>
<p><strong>Stakeholders/Target Audience</strong>: People who are looking to find house in London, especially the people who are starting their house search and has certain preferences.
Customers who are confused about the area they would like to live and want to know the trade-offs between different areas.</p>
<p>Note: This project attempts to solve the first step of your house search only, not everything. Websites mentioned above are still relevant and should be seen as complementary to this project.</p>
<h3><a id="user-content-data" class="anchor" aria-hidden="true" href="#data"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Data</h3>
<p>I used two datasets for the above problem:</p>
<p>a. London Lewisham council postcodes data downloaded from <a href="https://www.doogal.co.uk/AdministrativeAreasCSV.ashx?district=E09000023" rel="nofollow">Doogle</a>. This dataset contains all postcodes present in the Lewisham council in London. We narrow down to Lewisham council (and mainly focus on forest hill ward) to minimize the calls to Four square API.</p>
<p>Lewisham council postcodes data has many columns out of which only Postcode, Longitude, Latitude and Ward of the postcode are of our interest. Snapshot of the data is shown below:</p>
<pre><code>Postcode	In Use?	Latitude	Longitude	Easting	Northing	Grid Ref	Ward	Parish	Introduced	Terminated	Altitude	Country	Last Updated	Quality
BR1 4BY	Yes	51.417289	-0.001741	539050	170591	TQ390705	Downham	Lewisham, unparished area	1980-01-01		35	England	2018-11-15	Within the building of the matched address closest to the postcode mean
</code></pre>
<p>b. The second dataset I used is Foursquare venues data with categories, this will be used in collaboration with the above dataset.</p>
<p><strong>Assumptions</strong>: Data is downloaded from Doogle and it comprises of postcodes in Lewisham council In London, UK. Assuming that this data is limited and doesn't represent actual availability of the house. But that should be easy as we could just focus on the area here. I am going to focus on only Forest Hill part of the Lewisham as FourSquare api has aggressive rate limit for explore api of 500 calls/day</p>
<h3><a id="user-content-methodology" class="anchor" aria-hidden="true" href="#methodology"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Methodology</h3>
<p>This section is divided into three main parts, data wrangling/Cleansing, Exploratory analysis and machine learning method used to solve the problem.</p>
<ul>
<li>
<p><strong>Data Wrangling/ Cleansing</strong>: Data obtained from <a href="https://www.doogal.co.uk/AdministrativeAreasCSV.ashx?district=E09000023" rel="nofollow">Doogle</a> for Lewisham council has many columns but I concentrated on few columns only (Postcode, Latitude, Longitude and Ward). Also to obtain uniqueness on the different location, I explored the postcodes and learned that every postcode in Lewisham is divided into two major parts, District and Sector. I created a new column which contain Sectors and added the new column to the cleansed data. Also, I filtered the data for Forest Hill ward to minimize the calls to Four square API.</p>
</li>
<li>
<p><strong>Exploratory Analysis</strong>: Lewisham data has 18 unique wards and I filtered on a single ward as a point of interest namely Forest Hill. As explained in the cleansing step that postcode is comprised of two components, district and sector, I obtained 2 districts (SE23 and SE26) and 369 unique sectors from Forest Hill. I also plotted all the sectors on the map and then I created five different clusters based on the venue categories provided by Foursquare API data. I noticed that one of the clusters is giving imprecise points as shown in second map. I also explored top five venues in each sector.</p>
</li>
</ul>

<p><a target="_blank" rel="noopener noreferrer" href="/skanagasabap/Coursera_Capstone/blob/master/Capstone/Clusters_FH.png"><img src="/skanagasabap/Coursera_Capstone/blob/master/Capstone/Clusters_FH.png" alt="Clusters_FH" style="max-width:100%;"></a></p>


<p>Since our objective is to ease the search for  people who have preferences in terms of nearby venues, I tried to explore all the sectors under Forest Hill ward using Four square API and obtained multiple venues names, their geographical coordinates and category for each sector within 500m radius and put a limit on the number of venues as 100 for each sector.</p>
<p>From four square API, I obtained 46 different venue categories like Café, Pubs, Public transport, Park, Nature Reserve, Vintage stores etc.</p>
<ul>
<li><strong>Machine Learning</strong>: K means Clustering is used for the problem because after the exploratory analysis it was obvious that we can categorise postcodes into different homogenous clusters based on the venue categories.
Further on, we need to create our dataset in a format where rows will represent the sectors and all columns will be venue categories and we will calculate the score for every category in each sector as per their presence or absence in the sector. We sort the categories for each sector in order of their occurence, for example if a sector has more cafes and pubs, the sector will get higher score for the cafes and pubs and less for other categories.
The previous step will be sufficient to execute data mining technique I chose to accomplish this task(K mean clustering). K means clustering will segment the data into groups where groups are similar among themselves but different from other groups in terms of occurence of venue categories. For our dataset, K mean clustering will append another column to the dataset which will depict the cluster number, similar sectors will be grouped together.</li>
</ul>
<h3><a id="user-content-result" class="anchor" aria-hidden="true" href="#result"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Result</h3>
<p>I obtained 5 clusters from K means clustering executed in the previous step which are explained below:-</p>
<ul>
<li>Cluster 0. Contains houses with proximity to Indian Restaurants, Bus Stops, Train stations etc.</li>
<li>Cluster 1. For youngsters - The cluster highlights all houses in the proximity to cafes, pubs and nightlife etc.</li>
<li>Cluster 2. It contains houses with proximity to Vintage stores, nature reserves etc.</li>
<li>Cluster 3. It contains houses with proximity to Japanese restaurant, Grocery Stores, Mobile shops etc.</li>
<li>Cluster 4. Is suitable for those who like greenery, nearby forest, parks etc.</li>
</ul>
<p>The above clusters segment the data into 5 homogeneous groups which exhibit similarities among themselves in terms of venue categories are being heterogenous from the other groups. For example, If an individual is interested in staying near to places with proximity to pubs and cafes, Cluster1 is a great choice or if greenery, forest and parks are of interest to someone, sectors under cluster 5 are the most suitable.</p>
<h3><a id="user-content-conclusion" class="anchor" aria-hidden="true" href="#conclusion"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Conclusion</h3>
<p>As demonstrated in the Result section, we can effectively simplify the house search process for our customers by clustering the area based on the venue categories and filtering the cluster to narrow down their search area. Customers can then prefer to focus their house search by analysing various trade offs.</p>
</article>
  </div>

  </body>
</html>

