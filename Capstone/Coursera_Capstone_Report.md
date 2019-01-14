     
  <div id="readme" class="readme blob instapaper_body js-code-block-container">
    <article class="markdown-body entry-content" itemprop="text"><h2><a id="user-content-simplifying-your-house-search-in-london" class="anchor" aria-hidden="true" href="#simplifying-your-house-search-in-london"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Simplifying your house search in London</h2>
<h3><a id="user-content-introduction-business-problem" class="anchor" aria-hidden="true" href="#introduction-business-problem"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M4 9h1v1H4c-1.5 0-3-1.69-3-3.5S2.55 3 4 3h4c1.45 0 3 1.69 3 3.5 0 1.41-.91 2.72-2 3.25V8.59c.58-.45 1-1.27 1-2.09C10 5.22 8.98 4 8 4H4c-.98 0-2 1.22-2 2.5S3 9 4 9zm9-3h-1v1h1c1 0 2 1.22 2 2.5S13.98 12 13 12H9c-.98 0-2-1.22-2-2.5 0-.83.42-1.64 1-2.09V6.25c-1.09.53-2 1.84-2 3.25C6 11.31 7.55 13 9 13h4c1.45 0 3-1.69 3-3.5S14.5 6 13 6z"></path></svg></a>Introduction/ Business Problem</h3>
<p>It's very challenging to find a house in London, UK. There are many websites out there that try to help you out with searching your house like Zoopla, Rightmove. But even then narrowing down area to look for is a manual task that these websites do not automate/provide. Narrowing down area can be very tedious and time consuming process.</p>
<p>This project attempts to help you with identifying or narrowing down the search for the house according to your preferences about the neighborhood.
For example, one might want to look at all areas which has proximity to pubs, cafes, public transport etc. This project attempts cluster areas based on your provided preferences/categories. We then print those clusters on the map to distinguish between areas. That should make it easy to choose a certain areas to look for and narrow down your search.</p>
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
<p><a target="_blank" rel="noopener noreferrer" href="/poonamarora86/Coursera_Capstone/blob/master/Sectors_FH.png"><img src="/poonamarora86/Coursera_Capstone/raw/master/Sectors_FH.png" alt="Sectors_FH" style="max-width:100%;"></a></p>
<p><a target="_blank" rel="noopener noreferrer" href="/poonamarora86/Coursera_Capstone/blob/master/Clusters_FH.png"><img src="/poonamarora86/Coursera_Capstone/raw/master/Clusters_FH.png" alt="Clusters_FH" style="max-width:100%;"></a></p>
<p>Since our objective is to ease the search for  people who have preferences in terms of nearby venues, I tried to explore all the sectors under Forest Hill ward using Four square API and obtained multiple venues names, their geographical coordinates and category for each sector within 500m radius and put a limit on the number of venues as 100 for each sector.</p>
<p>From four square API, I obtained 46 different venue categories like Café, Pubs, Public transport, Park, Nature Reserve, Vintage stores etc.</p>
<ul>
<li><strong>Machine Learning</strong>: I decide to use K means Clustering for the problem because after the exploratory analysis it was obvious that we can categorise postcodes into different homogenous clusters based on the venue categories.
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
<p>As demonstrated in the Result section, we can effectively simplify the house search process for our customers by clustering the area based on the venue categories and filtering the cluster to narrow down their search area. Customers can then prefer to focus their house search by analysing various trade offs. Those who are confused can efficiently compare various areas and choose the one which is most suitable for them.</p>
</article>
  </div>

    </div>

  

  <details class="details-reset details-overlay details-overlay-dark">
    <summary data-hotkey="l" aria-label="Jump to line"></summary>
    <details-dialog class="Box Box--overlay d-flex flex-column anim-fade-in fast linejump" aria-label="Jump to line">
      <!-- '"` --><!-- </textarea></xmp> --></option></form><form class="js-jump-to-line-form Box-body d-flex" action="" accept-charset="UTF-8" method="get"><input name="utf8" type="hidden" value="&#x2713;" />
        <input class="form-control flex-auto mr-3 linejump-input js-jump-to-line-field" type="text" placeholder="Jump to line&hellip;" aria-label="Jump to line" autofocus>
        <button type="submit" class="btn" data-close-dialog>Go</button>
</form>    </details-dialog>
  </details>



  </div>
  <div class="modal-backdrop js-touch-events"></div>
</div>

    </div>
  </div>

  </div>

        
<div class="footer container-lg px-3" role="contentinfo">
  <div class="position-relative d-flex flex-justify-between pt-6 pb-2 mt-6 f6 text-gray border-top border-gray-light ">
    <ul class="list-style-none d-flex flex-wrap ">
      <li class="mr-3">&copy; 2019 <span title="0.58731s from unicorn-54ddcc9498-bwjp7">GitHub</span>, Inc.</li>
        <li class="mr-3"><a data-ga-click="Footer, go to terms, text:terms" href="https://github.com/site/terms">Terms</a></li>
        <li class="mr-3"><a data-ga-click="Footer, go to privacy, text:privacy" href="https://github.com/site/privacy">Privacy</a></li>
        <li class="mr-3"><a href="/security" data-ga-click="Footer, go to security, text:security">Security</a></li>
        <li class="mr-3"><a href="https://githubstatus.com/" data-ga-click="Footer, go to status, text:status">Status</a></li>
        <li><a data-ga-click="Footer, go to help, text:help" href="https://help.github.com">Help</a></li>
    </ul>

    <a aria-label="Homepage" title="GitHub" class="footer-octicon mr-lg-4" href="https://github.com">
      <svg height="24" class="octicon octicon-mark-github" viewBox="0 0 16 16" version="1.1" width="24" aria-hidden="true"><path fill-rule="evenodd" d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"/></svg>
</a>
   <ul class="list-style-none d-flex flex-wrap ">
        <li class="mr-3"><a data-ga-click="Footer, go to contact, text:contact" href="https://github.com/contact">Contact GitHub</a></li>
        <li class="mr-3"><a href="https://github.com/pricing" data-ga-click="Footer, go to Pricing, text:Pricing">Pricing</a></li>
      <li class="mr-3"><a href="https://developer.github.com" data-ga-click="Footer, go to api, text:api">API</a></li>
      <li class="mr-3"><a href="https://training.github.com" data-ga-click="Footer, go to training, text:training">Training</a></li>
        <li class="mr-3"><a href="https://blog.github.com" data-ga-click="Footer, go to blog, text:blog">Blog</a></li>
        <li><a data-ga-click="Footer, go to about, text:about" href="https://github.com/about">About</a></li>

    </ul>
  </div>
  <div class="d-flex flex-justify-center pb-6">
    <span class="f6 text-gray-light"></span>
  </div>
</div>



  <div id="ajax-error-message" class="ajax-error-message flash flash-error">
    <svg class="octicon octicon-alert" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M8.893 1.5c-.183-.31-.52-.5-.887-.5s-.703.19-.886.5L.138 13.499a.98.98 0 0 0 0 1.001c.193.31.53.501.886.501h13.964c.367 0 .704-.19.877-.5a1.03 1.03 0 0 0 .01-1.002L8.893 1.5zm.133 11.497H6.987v-2.003h2.039v2.003zm0-3.004H6.987V5.987h2.039v4.006z"/></svg>
    <button type="button" class="flash-close js-ajax-error-dismiss" aria-label="Dismiss error">
      <svg class="octicon octicon-x" viewBox="0 0 12 16" version="1.1" width="12" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48L7.48 8z"/></svg>
    </button>
    You can’t perform that action at this time.
  </div>


    
    <script crossorigin="anonymous" integrity="sha512-e75uP9DbVCArqtvE9g/NIrHuHCZqowHgDXCnwC5YoiVb3Ihn8iFyr4qHpLRTRAFbi0qoIpBMHKDFegjBmaW+CQ==" type="application/javascript" src="https://github.githubassets.com/assets/frameworks-86a0354a63afd3be29a60890d0d96cb9.js"></script>
    
    <script crossorigin="anonymous" async="async" integrity="sha512-7v9Y68aiOoz10CvgdLtOhMTu9pMxR3uBGrksgCVo+tacoIAgJqaCHmoDfiX23TjQYrTneTls0THFsFguuWclJw==" type="application/javascript" src="https://github.githubassets.com/assets/github-ccaf3225c60771fd43ca704771528a8f.js"></script>
    
    
    
  <div class="js-stale-session-flash stale-session-flash flash flash-warn flash-banner d-none">
    <svg class="octicon octicon-alert" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M8.893 1.5c-.183-.31-.52-.5-.887-.5s-.703.19-.886.5L.138 13.499a.98.98 0 0 0 0 1.001c.193.31.53.501.886.501h13.964c.367 0 .704-.19.877-.5a1.03 1.03 0 0 0 .01-1.002L8.893 1.5zm.133 11.497H6.987v-2.003h2.039v2.003zm0-3.004H6.987V5.987h2.039v4.006z"/></svg>
    <span class="signed-in-tab-flash">You signed in with another tab or window. <a href="">Reload</a> to refresh your session.</span>
    <span class="signed-out-tab-flash">You signed out in another tab or window. <a href="">Reload</a> to refresh your session.</span>
  </div>
  <div class="facebox" id="facebox" style="display:none;">
  <div class="facebox-popup">
    <div class="facebox-content" role="dialog" aria-labelledby="facebox-header" aria-describedby="facebox-description">
    </div>
    <button type="button" class="facebox-close js-facebox-close" aria-label="Close modal">
      <svg class="octicon octicon-x" viewBox="0 0 12 16" version="1.1" width="12" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48L7.48 8z"/></svg>
    </button>
  </div>
</div>

  <template id="site-details-dialog">
  <details class="details-reset details-overlay details-overlay-dark lh-default text-gray-dark" open>
    <summary aria-haspopup="dialog" aria-label="Close dialog"></summary>
    <details-dialog class="Box Box--overlay d-flex flex-column anim-fade-in fast">
      <button class="Box-btn-octicon m-0 btn-octicon position-absolute right-0 top-0" type="button" aria-label="Close dialog" data-close-dialog>
        <svg class="octicon octicon-x" viewBox="0 0 12 16" version="1.1" width="12" height="16" aria-hidden="true"><path fill-rule="evenodd" d="M7.48 8l3.75 3.75-1.48 1.48L6 9.48l-3.75 3.75-1.48-1.48L4.52 8 .77 4.25l1.48-1.48L6 6.52l3.75-3.75 1.48 1.48L7.48 8z"/></svg>
      </button>
      <div class="octocat-spinner my-6 js-details-dialog-spinner"></div>
    </details-dialog>
  </details>
</template>

  <div class="Popover js-hovercard-content position-absolute" style="display: none; outline: none;" tabindex="0">
  <div class="Popover-message Popover-message--bottom-left Popover-message--large Box box-shadow-large" style="width:360px;">
  </div>
</div>

<div id="hovercard-aria-description" class="sr-only">
  Press h to open a hovercard with more details.
</div>

  <div aria-live="polite" class="js-global-screen-reader-notice sr-only"></div>

  </body>
</html>

