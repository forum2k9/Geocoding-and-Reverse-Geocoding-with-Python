# Geocoding-and-Reverse-Geocoding-with-Python
Authorship Disclaimer: This tutorial titled 'Geocoding and Reverse Geocoding with Python' was originally by <a href='https://umar-yusuf.blogspot.com'>me</a> and submitted to DataCamp on "Jan 27, 2018". Since they didn't publish it on their platform, I have decided to do it here so that someone out there may find it useful.

<body>
  <div tabindex="-1" id="notebook" class="border-box-sizing">
    <div class="container" id="notebook-container">

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="DataCamp-Tutorial---Geocoding-and-Reverse-Geocoding-with-Python">DataCamp Tutorial - Geocoding and Reverse Geocoding with Python<a class="anchor-link" href="#DataCamp-Tutorial---Geocoding-and-Reverse-Geocoding-with-Python">&#182;</a></h2><p>The increasing use of location-aware data and technologies that are able to give directions relative to location and access geographically aware data has given rise to category of data scientists with strong knowledge of geospatial data - Geo-data Scientists.</p>
<p>In this tutorial, you will discover how to use PYTHON to carry out geocoding task. Specifically, you will learn to use GeoPy, Pandas and Folium PYTHON libraries to complete geocoding tasks. Because this is a geocoding tutorial, the article will cover more of GeoPy than Pandas. If you are not familiar with Pandas, you should definitely consider studying the <a href="https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python">Pandas Tutorial by Karlijn Willems</a> so also this <a href="http://datacamp-community.s3.amazonaws.com/9f0f2ae1-8bd8-4302-a67b-e17f3059d9e8">Pandas cheat sheet</a> will be handy to your learning.</p>
<h3 id="Tutorial-Overview">Tutorial Overview<a class="anchor-link" href="#Tutorial-Overview">&#182;</a></h3><p><ul>
  <li>What is Geocoding?</li>
  <li>Geocoding with Python</li>
  <li>Putting it all together – Bulk Geocoding</li>
  <li>Accuracy of the Result</li>
  <li>Mapping Geocoding Result</li>
  <li>Conclusion</li>
</ul></p>
<hr />


<h3 id="What-is-Geocoding?">What is Geocoding?<a class="anchor-link" href="#What-is-Geocoding?">&#182;</a></h3><p>A very common task faced by Geo-data Scientist is the conversion of physical human-readable addresses of places into latitude and longitude geographical coordinates. This process is known as “<a href="https://en.wikipedia.org/wiki/Geocoding">Geocoding</a>” while the reverse case (that is converting latitude and longitude coordinates into physical addresses) is known as “<a href="https://en.wikipedia.org/wiki/Reverse_geocoding">Reverse Geocoding</a>”. To clarify this explanation, here is an example using the datacamp USA office address:-</p>
<p><a href="https://www.datacamp.com/contact-us"><img src='https://1.bp.blogspot.com/-Enb6vk8j1kI/WkyRIhZDZEI/AAAAAAAACFI/dWOZSEEGsZQqtQhTu10Yj7dNBZbZM0d1wCLcBGAs/s1600/geocoding_datacamp.PNG' width="700px" height="700px" /></a></p>
<p><u>Geocoding:</u> is converting an address like “<b>Empire State Building 350 5th Ave, Floor 77 New York, NY 10118</b>” to “latitude <b>40.7484284</b>, longitude <b>-73.9856546</b>”.<br /> <br />
<u>Reverse Geocoding:</u> is converting “latitude <b>40.7484284</b>, longitude <b>-73.9856546</b>” to address “<b>Empire State Building 350 5th Ave, Floor 77 New York, NY 10118</b>”.</p>
<p>Now that you have seen how to do forward and reverse geocoding manually, let’s see how it can be done programmatically in PYTHON on larger dataset by calling some APIs.</p>
<hr />


<h3 id="Geocoding-with-Python">Geocoding with Python<a class="anchor-link" href="#Geocoding-with-Python">&#182;</a></h3><p>There is good number of PYTHON modules for Geocoding and Reverse Geocoding. In this tutorial, you will use the PYTHON Geocoding Toolbox named <strong>GeoPy</strong> which provides support for several popular geocoding web services including Google Geocoding API, OpenStreetMap Nominatim, ESRI ArcGIS, Bing Maps API etc.</p>
<p>You will make use of OpenStreetMap Nominatim API because it is completely open source and has no limit to the number of requests you can make. But first, you need to install the libraries (geopy, pandas and folium) on your PYTHON environment using “<i>pip install geopy, pandas, folium</i>”.</p>
<p>Let's import the libraries...</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Importing the necessary modules for this tutorial</span>
<span class="c1"># Folium Library for visualizing data on interactive map</span>
<span class="c1"># Pandas Library for fast, flexible, and expressive data structures designed</span>

<span class="kn">import</span> <span class="nn">folium</span>
<span class="kn">import</span> <span class="nn">pandas</span> <span class="k">as</span> <span class="nn">pd</span>
<span class="kn">from</span> <span class="nn">geopy.geocoders</span> <span class="k">import</span> <span class="n">Nominatim</span><span class="p">,</span> <span class="n">ArcGIS</span><span class="p">,</span> <span class="n">GoogleV3</span> <span class="c1"># Geocoder APIs</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Note: You don’t have to import all the three geocoding APIs namely Nominatim, ArcGIS and GoogleV3 from the geopy module. However, I did so you can test and compare the result from the different APIs to find out which is more accurate with your specific dataset. To follow along and to get you familiar with geocoding, make use of “OpenStreetMap Nominatim API” for this article.</p>
<p>To do forward geocoding (convert address to latitude/longitude), you first create a geocoder API object by calling the Nominatim() API class.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">g</span> <span class="o">=</span> <span class="n">Nominatim</span><span class="p">()</span> <span class="c1"># You can tryout ArcGIS or GoogleV3 APIs to compare the results</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>In the next few lines of code below, you will do forward Geocoding and Reverse Geocoding respectively.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Geocoding - Address to lat/long</span>

<span class="n">n</span> <span class="o">=</span> <span class="n">g</span><span class="o">.</span><span class="n">geocode</span><span class="p">(</span><span class="s1">&#39;Empire State Building New York&#39;</span><span class="p">,</span> <span class="n">timeout</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span> <span class="c1"># Address to geocode</span>
<span class="nb">print</span><span class="p">(</span><span class="n">n</span><span class="o">.</span><span class="n">latitude</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">longitude</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>40.7484284 -73.9856546198733
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>By calling the geocode() method on the defined API object, you will supply an address as the first parameter to get it corresponding latitude and longitude attributes.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[4]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Reverse Geocoding - lat/long to Address</span>

<span class="n">n</span> <span class="o">=</span> <span class="n">g</span><span class="o">.</span><span class="n">reverse</span><span class="p">((</span><span class="mf">40.7484284</span><span class="p">,</span> <span class="o">-</span><span class="mf">73.9856546198733</span><span class="p">),</span> <span class="n">timeout</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span> <span class="c1"># Lat, Long to reverse geocode</span>
<span class="nb">print</span><span class="p">(</span><span class="n">n</span><span class="o">.</span><span class="n">address</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>Empire State Building, 350, 5th Avenue, Korea Town, Manhattan Community Board 5, New York County, NYC, New York, 10018, United States of America
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>To reverse the process, you will call the reverse() method on the same API object and supply latitude and longitude coordinate values in that order to obtain their corresponding address attribute.</p>
<p>The process above is the very basic of geocoding a single address and reverse geocoding of a pair of latitude and longitude coordinate using PYTHON.</p>
<p>Now, let’s process a lager dataset in the next section. You will use Pandas library for the data handling/wrangling and Folium to subsequently visualize the geocoded result.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Putting-it-all-together-&#8211;-Bulk-Geocoding">Putting it all together &#8211; Bulk Geocoding<a class="anchor-link" href="#Putting-it-all-together-&#8211;-Bulk-Geocoding">&#182;</a></h3><p>In the previous section, you geocoded a single place/address; "Empire State Building, New York". Now, you will work with bulk dataset, which is broadened to contain list of similar places (buildings) in New York City.</p>
<p>On this <a href="https://en.wikipedia.org/wiki/List_of_tallest_buildings_in_New_York_City">wikipedia</a> page, there is an awesome list of tallest buildings in New York City. Unfortunately, the table has no detailed addresses or geographic coordinates of the buildings.</p>
<p><a href="https://en.wikipedia.org/wiki/List_of_tallest_buildings_in_New_York_City"><img src="https://4.bp.blogspot.com/-liAzTVSnx1s/WmP7GgHkVQI/AAAAAAAACPg/KNuUr5Gn6fMOo_waQRfwGUD3T2L74GLGACLcBGAs/s1600/wikipedia-Tallest_Buildings_in_NY.PNG" /></a></p>
<p>You will fix this missing data by applying geocoding technique you learned in the previous section. Specifically, you are going to look at the 'Name' column on the first table on the page where "Empire State Building" is the third ranked tallest building.</p>
<p>There are many methods of importing such a tabulated list into a PYTHON environment, in this case use pandas <b>read_clipboard()</b> method. Copy “Rank and Name” columns to your clipboard and create a dataframe.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Create a dataframe from the copied table columns on the clipboard and display its first 10 records</span>

<span class="n">df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">read_clipboard</span><span class="p">()</span>
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[5]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>One World Trade Center</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>432 Park Avenue</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Empire State Building</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Bank of America Tower</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Three World Trade Center*</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6=</td>
      <td>Chrysler Building</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6=</td>
      <td>The New York Times Building</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>One57</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Four World Trade Center</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>220 Central Park South</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Just like with any other data science dataset, you should do some clean up on the data. In particular, remove special characters (such as * “ ? # ‘ \ %) in the input dataset. This will enable the system read the names correctly without mixing there meaning.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Remove all characters except letters belonging to english alphabet, spaces and tabs</span>

<span class="n">df</span><span class="p">[</span><span class="s1">&#39;Name&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Name&#39;</span><span class="p">]</span><span class="o">.</span><span class="n">str</span><span class="o">.</span><span class="n">replace</span><span class="p">(</span><span class="s1">&#39;[^A-Za-z\s0-9]+&#39;</span><span class="p">,</span> <span class="s1">&#39;&#39;</span><span class="p">)</span>
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[6]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>One World Trade Center</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>432 Park Avenue</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Empire State Building</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Bank of America Tower</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Three World Trade Center</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6=</td>
      <td>Chrysler Building</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6=</td>
      <td>The New York Times Building</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>One57</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Four World Trade Center</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>220 Central Park South</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Also, the names may likely be in use in some other part of the world, you can help the system better know that you are primarily concerned with the building names in New York City by appending “New York City” to each building name as follow.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[7]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Create a new column &quot;Address_1&quot; to hold the updated building names</span>

<span class="n">df</span><span class="p">[</span><span class="s1">&#39;Address_1&#39;</span><span class="p">]</span> <span class="o">=</span> <span class="p">(</span><span class="n">df</span><span class="p">[</span><span class="s1">&#39;Name&#39;</span><span class="p">]</span> <span class="o">+</span> <span class="s1">&#39;, New York City&#39;</span><span class="p">)</span>
<span class="n">df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[7]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>Name</th>
      <th>Address_1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>One World Trade Center</td>
      <td>One World Trade Center, New York City</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>432 Park Avenue</td>
      <td>432 Park Avenue, New York City</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Empire State Building</td>
      <td>Empire State Building, New York City</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Bank of America Tower</td>
      <td>Bank of America Tower, New York City</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Three World Trade Center</td>
      <td>Three World Trade Center, New York City</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6=</td>
      <td>Chrysler Building</td>
      <td>Chrysler Building, New York City</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6=</td>
      <td>The New York Times Building</td>
      <td>The New York Times Building, New York City</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>One57</td>
      <td>One57, New York City</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Four World Trade Center</td>
      <td>Four World Trade Center, New York City</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>220 Central Park South</td>
      <td>220 Central Park South, New York City</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Next step is the loop through the each record on 'Address_1' column and get the corresponding address and geographic coordinates.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">add_list</span> <span class="o">=</span> <span class="p">[]</span> <span class="c1"># an empty list to hold the geocoded results</span>

<span class="k">for</span> <span class="n">add</span> <span class="ow">in</span> <span class="n">df</span><span class="p">[</span><span class="s1">&#39;Address_1&#39;</span><span class="p">]:</span>
    <span class="nb">print</span> <span class="p">(</span><span class="s1">&#39;Processing .... &#39;</span><span class="p">,</span> <span class="n">add</span><span class="p">)</span>
    
    <span class="k">try</span><span class="p">:</span>
        <span class="n">n</span> <span class="o">=</span> <span class="n">g</span><span class="o">.</span><span class="n">geocode</span><span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="n">timeout</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span>
        
        <span class="n">data</span> <span class="o">=</span> <span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">latitude</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">longitude</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">address</span><span class="p">)</span>
        <span class="n">add_list</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">data</span><span class="p">)</span>
        
    <span class="k">except</span> <span class="ne">Exception</span><span class="p">:</span>
        <span class="n">data</span> <span class="o">=</span> <span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">)</span>
        <span class="n">add_list</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">data</span><span class="p">)</span>
        
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>Processing ....  One World Trade Center, New York City
Processing ....  432 Park Avenue, New York City
Processing ....  Empire State Building, New York City
Processing ....  Bank of America Tower, New York City
Processing ....  Three World Trade Center, New York City
Processing ....  Chrysler Building, New York City
Processing ....  The New York Times Building, New York City
Processing ....  One57, New York City
Processing ....  Four World Trade Center, New York City
Processing ....  220 Central Park South, New York City
Processing ....  70 Pine Street, New York City
Processing ....  30 Park Place, New York City
Processing ....  40 Wall Street, New York City
Processing ....  Citigroup Center, New York City
Processing ....  10 Hudson Yards, New York City
Processing ....  8 Spruce Street, New York City
Processing ....  Trump World Tower, New York City
Processing ....  30 Rockefeller Plaza, New York City
Processing ....  56 Leonard Street, New York City
Processing ....  CitySpire Center, New York City
Processing ....  28 Liberty Street, New York City
Processing ....  4 Times Square, New York City
Processing ....  MetLife Building, New York City
Processing ....  731 Lexington Avenue, New York City
Processing ....  Woolworth Building, New York City
Processing ....  50 West Street, New York City
Processing ....  One Worldwide Plaza, New York City
Processing ....  Madison Square Park Tower, New York City
Processing ....  Carnegie Hall Tower, New York City
Processing ....  383 Madison Avenue, New York City
Processing ....  1717 Broadway, New York City
Processing ....  AXA Equitable Center, New York City
Processing ....  One Penn Plaza, New York City
Processing ....  1251 Avenue of the Americas, New York City
Processing ....  Time Warner Center South Tower, New York City
Processing ....  Time Warner Center North Tower, New York City
Processing ....  200 West Street, New York City
Processing ....  60 Wall Street, New York City
Processing ....  One Astor Plaza, New York City
Processing ....  7 World Trade Center, New York City
Processing ....  One Liberty Plaza, New York City
Processing ....  20 Exchange Place, New York City
Processing ....  200 Vesey Street, New York City
Processing ....  Bertelsmann Building, New York City
Processing ....  Times Square Tower, New York City
Processing ....  Metropolitan Tower, New York City
Processing ....  252 East 57th Street, New York City
Processing ....  100 East 53rd Street, New York City
Processing ....  500 Fifth Avenue, New York City
Processing ....  JP Morgan Chase World Headquarters, New York City
Processing ....  General Motors Building, New York City
Processing ....  3 Manhattan West, New York City
Processing ....  Metropolitan Life Insurance Company Tower, New York City
Processing ....  Americas Tower, New York City
Processing ....  Solow Building, New York City
Processing ....  Marine Midland Building, New York City
Processing ....  55 Water Street, New York City
Processing ....  277 Park Avenue, New York City
Processing ....  5 Beekman, New York City
Processing ....  Morgan Stanley Building, New York City
Processing ....  Random House Tower, New York City
Processing ....  Four Seasons Hotel New York, New York City
Processing ....  1221 Avenue of the Americas, New York City
Processing ....  Lincoln Building, New York City
Processing ....  Barclay Tower, New York City
Processing ....  Paramount Plaza, New York City
Processing ....  Trump Tower, New York City
Processing ....  One Court Square, New York City
Processing ....  Sky, New York City
Processing ....  1 Wall Street, New York City
Processing ....  599 Lexington Avenue, New York City
Processing ....  Silver Towers I, New York City
Processing ....  Silver Towers II, New York City
Processing ....  712 Fifth Avenue, New York City
Processing ....  Chanin Building, New York City
Processing ....  245 Park Avenue, New York City
Processing ....  Sony Tower, New York City
Processing ....  Tower 28, New York City
Processing ....  225 Liberty Street, New York City
Processing ....  1 New York Plaza, New York City
Processing ....  570 Lexington Avenue, New York City
Processing ....  MiMA, New York City
Processing ....  345 Park Avenue, New York City
Processing ....  400 Fifth Avenue, New York City
Processing ....  W R Grace Building, New York City
Processing ....  Home Insurance Plaza, New York City
Processing ....  1095 Avenue of the Americas, New York City
Processing ....  W New York Downtown Hotel and Residences, New York City
Processing ....  101 Park Avenue, New York City
Processing ....  One Dag Hammarskjld Plaza, New York City
Processing ....  Central Park Place, New York City
Processing ....  888 7th Avenue, New York City
Processing ....  Waldorf Astoria New York, New York City
Processing ....  1345 Avenue of the Americas, New York City
Processing ....  Trump Palace Condominiums, New York City
Processing ....  Olympic Tower, New York City
Processing ....  Mercantile Building, New York City
Processing ....  425 Fifth Avenue, New York City
Processing ....  One Madison, New York City
Processing ....  919 Third Avenue, New York City
Processing ....  New York Life Building, New York City
Processing ....  750 7th Avenue, New York City
Processing ....  The Epic, New York City
Processing ....  Eventi, New York City
Processing ....  Tower 49, New York City
Processing ....  555 10th Avenue, New York City
Processing ....  The Hub, New York City
Processing ....  Calyon Building, New York City
Processing ....  Baccarat Hotel and Residences, New York City
Processing ....  250 West 55th Street, New York City
Processing ....  The Orion, New York City
Processing ....  590 Madison Avenue, New York City
Processing ....  11 Times Square, New York City
Processing ....  1166 Avenue of the Americas, New York City
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Save the result into a dataframe.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># make a new dataframe to hold geocoded reult</span>

<span class="n">add_list_df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">add_list</span><span class="p">,</span> <span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s1">&#39;Address_1&#39;</span><span class="p">,</span> <span class="s1">&#39;Latitude&#39;</span><span class="p">,</span> <span class="s1">&#39;Longitude&#39;</span><span class="p">,</span> <span class="s1">&#39;Full Address&#39;</span><span class="p">])</span>
<span class="n">add_list_df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[9]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Address_1</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Full Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>One World Trade Center, New York City</td>
      <td>40.713</td>
      <td>-74.0132</td>
      <td>One World Trade Center, 1, Fulton Street, Batt...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>432 Park Avenue, New York City</td>
      <td>40.7615</td>
      <td>-73.9719</td>
      <td>432 Park Avenue, 432, Manhattan Community Boar...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Empire State Building, New York City</td>
      <td>40.7484</td>
      <td>-73.9857</td>
      <td>Empire State Building, 350, 5th Avenue, Korea ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bank of America Tower, New York City</td>
      <td>40.7555</td>
      <td>-73.9847</td>
      <td>Bank of America Tower, 115, West 42nd Street, ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Three World Trade Center, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Chrysler Building, New York City</td>
      <td>40.7516</td>
      <td>-73.9753</td>
      <td>Chrysler Building, East 43rd Street, Tudor Cit...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>The New York Times Building, New York City</td>
      <td>40.7559</td>
      <td>-73.9893</td>
      <td>The New York Times Building, 620, 8th Avenue, ...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>One57, New York City</td>
      <td>40.7655</td>
      <td>-73.9791</td>
      <td>One57, West 57th Street, Diamond District, Man...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Four World Trade Center, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>9</th>
      <td>220 Central Park South, New York City</td>
      <td>40.767</td>
      <td>-73.9806</td>
      <td>220 Central Park South, Manhattan Community Bo...</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Accuracy-of-the-Result">Accuracy of the Result<a class="anchor-link" href="#Accuracy-of-the-Result">&#182;</a></h3><p>A quick inspection of the latest data frame reveals that the obtained geographical coordinates of the buildings lies within the latitude and longitude territory of New York City (that is: 40°42′46″N, 74°00′21″W). There are some buildings that were not geocoded (their results were not found). This indicates that there geocode results are not available in the OpenStreetMap Nominatim API.</p>
<p>Now, you can make use of some other APIs to check if their geocode results are available within the new API.</p>
<p>First, use the pandas “loc” method to separate the records whose geocode results were found from those that were not found.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># Extract the records where value of Latitude and Longitude are the same (that is: None)</span>

<span class="n">geocode_found</span> <span class="o">=</span> <span class="n">add_list_df</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="n">add_list_df</span><span class="p">[</span><span class="s1">&#39;Latitude&#39;</span><span class="p">]</span> <span class="o">!=</span> <span class="n">add_list_df</span><span class="p">[</span><span class="s1">&#39;Longitude&#39;</span><span class="p">]]</span>

<span class="n">geocode_not_found</span> <span class="o">=</span> <span class="n">add_list_df</span><span class="o">.</span><span class="n">loc</span><span class="p">[</span><span class="n">add_list_df</span><span class="p">[</span><span class="s1">&#39;Latitude&#39;</span><span class="p">]</span> <span class="o">==</span> <span class="n">add_list_df</span><span class="p">[</span><span class="s1">&#39;Longitude&#39;</span><span class="p">]]</span>
<span class="n">geocode_not_found</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[10]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Address_1</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Full Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Three World Trade Center, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Four World Trade Center, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Madison Square Park Tower, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Time Warner Center South Tower, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Time Warner Center North Tower, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>49</th>
      <td>JP Morgan Chase World Headquarters, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>50</th>
      <td>General Motors Building, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Silver Towers I, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Silver Towers II, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Tower 28, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>87</th>
      <td>W New York Downtown Hotel and Residences, New ...</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>89</th>
      <td>One Dag Hammarskjld Plaza, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>92</th>
      <td>Waldorf Astoria New York, New York City</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>There are many ways to get this done, in this case you simply compare the latitude and longitude columns knowing that their numeric values can never be the same. Wherever the latitude and longitude cells have the same value, it will be a string value of “None”, which means a geocode result wasn’t found for that building’s name.</p>
<p>Now, will you redefine the geocoder API object to call a different API (ArcGIS API for example) by calling the ArcGIS() API class.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">g</span> <span class="o">=</span> <span class="n">ArcGIS</span><span class="p">()</span> <span class="c1"># redefine the API object</span>
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Then you can now loop through “geocode_not_found” data frame to see if you can get some results from the new API.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[12]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">add_list</span> <span class="o">=</span> <span class="p">[]</span>

<span class="k">for</span> <span class="n">add</span> <span class="ow">in</span> <span class="n">geocode_not_found</span><span class="p">[</span><span class="s1">&#39;Address_1&#39;</span><span class="p">]:</span>
    <span class="nb">print</span> <span class="p">(</span><span class="s1">&#39;Processing .... &#39;</span><span class="p">,</span> <span class="n">add</span><span class="p">)</span>
    
    <span class="k">try</span><span class="p">:</span>
        <span class="n">n</span> <span class="o">=</span> <span class="n">g</span><span class="o">.</span><span class="n">geocode</span><span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="n">timeout</span><span class="o">=</span><span class="mi">10</span><span class="p">)</span>
        
        <span class="n">data</span> <span class="o">=</span> <span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">latitude</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">longitude</span><span class="p">,</span> <span class="n">n</span><span class="o">.</span><span class="n">address</span><span class="p">)</span>
        <span class="n">add_list</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">data</span><span class="p">)</span>
        
    <span class="k">except</span> <span class="ne">Exception</span><span class="p">:</span>
        <span class="n">data</span> <span class="o">=</span> <span class="p">(</span><span class="n">add</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">,</span> <span class="s2">&quot;None&quot;</span><span class="p">)</span>
        <span class="n">add_list</span><span class="o">.</span><span class="n">append</span><span class="p">(</span><span class="n">data</span><span class="p">)</span>
        
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>Processing ....  Three World Trade Center, New York City
Processing ....  Four World Trade Center, New York City
Processing ....  Madison Square Park Tower, New York City
Processing ....  Time Warner Center South Tower, New York City
Processing ....  Time Warner Center North Tower, New York City
Processing ....  JP Morgan Chase World Headquarters, New York City
Processing ....  General Motors Building, New York City
Processing ....  Silver Towers I, New York City
Processing ....  Silver Towers II, New York City
Processing ....  Tower 28, New York City
Processing ....  W New York Downtown Hotel and Residences, New York City
Processing ....  One Dag Hammarskjld Plaza, New York City
Processing ....  Waldorf Astoria New York, New York City
</pre>
</div>
</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>Here you can see that ArcGIS was able to retrieve geocode results for the buildings that Nominatim API couldn’t retrieve.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[13]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">add_list_df</span> <span class="o">=</span> <span class="n">pd</span><span class="o">.</span><span class="n">DataFrame</span><span class="p">(</span><span class="n">add_list</span><span class="p">,</span> <span class="n">columns</span><span class="o">=</span><span class="p">[</span><span class="s1">&#39;Address_1&#39;</span><span class="p">,</span> <span class="s1">&#39;Latitude&#39;</span><span class="p">,</span> <span class="s1">&#39;Longitude&#39;</span><span class="p">,</span> <span class="s1">&#39;Full Address&#39;</span><span class="p">])</span>
<span class="n">add_list_df</span><span class="o">.</span><span class="n">head</span><span class="p">(</span><span class="mi">10</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[13]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Address_1</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Full Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Three World Trade Center, New York City</td>
      <td>40.709690</td>
      <td>-74.011670</td>
      <td>World Trade Center</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Four World Trade Center, New York City</td>
      <td>40.709900</td>
      <td>-74.012090</td>
      <td>Four World Trade Center</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Madison Square Park Tower, New York City</td>
      <td>40.741500</td>
      <td>-73.987580</td>
      <td>Madison Square</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Time Warner Center South Tower, New York City</td>
      <td>40.767857</td>
      <td>-73.982391</td>
      <td>Time Warner Ctr, New York, 10019</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Time Warner Center North Tower, New York City</td>
      <td>40.767857</td>
      <td>-73.982391</td>
      <td>Time Warner Ctr, New York, 10019</td>
    </tr>
    <tr>
      <th>5</th>
      <td>JP Morgan Chase World Headquarters, New York City</td>
      <td>40.727050</td>
      <td>-73.825910</td>
      <td>Headquarters</td>
    </tr>
    <tr>
      <th>6</th>
      <td>General Motors Building, New York City</td>
      <td>40.879330</td>
      <td>-73.871330</td>
      <td>GM</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Silver Towers I, New York City</td>
      <td>40.843822</td>
      <td>-73.847128</td>
      <td>Silver St, Bronx, New York, 10461</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Silver Towers II, New York City</td>
      <td>40.843822</td>
      <td>-73.847128</td>
      <td>Silver St, Bronx, New York, 10461</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Tower 28, New York City</td>
      <td>40.593850</td>
      <td>-74.186119</td>
      <td>28 Towers Ln, Staten Island, New York, 10314</td>
    </tr>
  </tbody>
</table>
</div>
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>You could also import the latitudes and longitudes as points unto Google maps to further validate their positional accuracy. As seen below, the latitude and longitude positions are at least more than 95% accurately geocoded.</p>
<p><a href='https://drive.google.com/open?id=1thU3u0lm_DgWxrTAGKmJ6DuMSlH8mRbc&usp=sharing'><img src='https://1.bp.blogspot.com/-toHB5CuJwco/WmPn5rOpO6I/AAAAAAAACPQ/QL0Yx4Pjoa47defSKrBPvE26E89-R9SWQCLcBGAs/s1600/GoogleMaps.PNG' /></a></p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Mapping-Geocoding-Result">Mapping Geocoding Result<a class="anchor-link" href="#Mapping-Geocoding-Result">&#182;</a></h3>
</div>
</div>
</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<p>An obvious purpose of geocoding is to visualize places/addresses on a map. Here, you will learn to visualize the “geocode_found” data frame on a simple interactive map using the folium library (recall you have imported the library at the beginning of this tutorial). Folium makes it easy to visualize data that's been manipulated in PYTHON on an interactive LeafletJS map.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="c1"># convert Full Address, Latitude and Longitude dataframe columns to list</span>
<span class="n">full_address_list</span> <span class="o">=</span> <span class="nb">list</span><span class="p">(</span><span class="n">geocode_found</span><span class="p">[</span><span class="s1">&#39;Full Address&#39;</span><span class="p">])</span>
<span class="n">long_list</span> <span class="o">=</span> <span class="nb">list</span><span class="p">(</span><span class="n">geocode_found</span><span class="p">[</span><span class="s2">&quot;Longitude&quot;</span><span class="p">])</span>
<span class="n">lat_list</span> <span class="o">=</span> <span class="nb">list</span><span class="p">(</span><span class="n">geocode_found</span><span class="p">[</span><span class="s2">&quot;Latitude&quot;</span><span class="p">])</span>


<span class="c1"># create folium map object</span>
<span class="n">geocoded_map</span> <span class="o">=</span> <span class="n">folium</span><span class="o">.</span><span class="n">Map</span><span class="p">(</span><span class="n">location</span><span class="o">=</span><span class="p">[</span><span class="mf">40.7484284</span><span class="p">,</span> <span class="o">-</span><span class="mf">73.9856546</span><span class="p">],</span> <span class="n">zoom_start</span><span class="o">=</span><span class="mi">13</span><span class="p">)</span> <span class="c1"># location=[Lat, Long]</span>


<span class="c1"># loop through the lists and create markers on the map object</span>
<span class="k">for</span> <span class="n">long</span><span class="p">,</span> <span class="n">lat</span><span class="p">,</span> <span class="n">address</span> <span class="ow">in</span> <span class="nb">zip</span><span class="p">(</span><span class="n">long_list</span><span class="p">,</span> <span class="n">lat_list</span><span class="p">,</span> <span class="n">full_address_list</span><span class="p">):</span>
    <span class="n">geocoded_map</span><span class="o">.</span><span class="n">add_child</span><span class="p">(</span><span class="n">folium</span><span class="o">.</span><span class="n">Marker</span><span class="p">(</span><span class="n">location</span><span class="o">=</span><span class="p">[</span><span class="n">lat</span><span class="p">,</span> <span class="n">long</span><span class="p">],</span> <span class="n">popup</span><span class="o">=</span><span class="n">address</span><span class="p">))</span>
    <span class="n">geocoded_map</span><span class="o">.</span><span class="n">add_child</span><span class="p">(</span><span class="n">folium</span><span class="o">.</span><span class="n">CircleMarker</span><span class="p">(</span><span class="n">location</span><span class="o">=</span><span class="p">[</span><span class="n">lat</span><span class="p">,</span> <span class="n">long</span><span class="p">],</span> <span class="n">popup</span><span class="o">=</span><span class="n">address</span><span class="p">,</span> <span class="n">radius</span><span class="o">=</span><span class="mi">5</span><span class="p">,</span> <span class="n">color</span><span class="o">=</span><span class="s1">&#39;green&#39;</span><span class="p">,</span> <span class="n">fill_color</span><span class="o">=</span><span class="s1">&#39;green&#39;</span><span class="p">,</span> <span class="n">fill_opacity</span><span class="o">=.</span><span class="mi">2</span><span class="p">))</span>


<span class="c1"># Display the map inline</span>
<span class="n">geocoded_map</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">


<div class="output_area"><div class="prompt output_prompt">Out[14]:</div>

<div class="output_html rendered_html output_subarea output_execute_result">
</div>

</div>

</div>
</div>

</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="Conclusion">Conclusion<a class="anchor-link" href="#Conclusion">&#182;</a></h3><p>You have just learned about geocoding and reverse geocoding in Python primarily using third party GeoPy module. The knowledge you have learned here will definitely help to locate addresses and places when working on datasets that are amenable to maps.
Geocoding is useful for plotting and extracting places/addresses on a map for obvious reasons which may include:-</p>
<ul>
<li>To visualize distances such as roads and pipelines</li>
<li>To deliver insight into public health information, </li>
<li>To determine voting demographics, </li>
<li>To analyze law enforcement and intelligence data, etc</li>
</ul>
<p>Be skeptical of your geocoding results.  Always inspect actual address match locations against other data sources, like street basemaps.  Compare your results to more than one geocode API sources if possible.  For example, if geocoded in OpenStreetMap Nominatim, import the results to Google Maps to see if they match its basemap.</p>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h3 id="References">References<a class="anchor-link" href="#References">&#182;</a></h3><ul>
<li><p><a href="https://geopy.readthedocs.io">https://geopy.readthedocs.io</a></p>
</li>
<li><p><a href="https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python">https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python</a></p>
</li>
<li><p><a href="https://www.directionsmag.com/article/3536">https://www.directionsmag.com/article/3536</a></p>
</li>
<li><p><a href="http://gis.harvard.edu/services/blog/geocoding-best-practices">http://gis.harvard.edu/services/blog/geocoding-best-practices</a></p>
</li>
</ul>

</div>
</div>
</div>
<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In&nbsp;[&nbsp;]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span> 
</pre></div>

</div>
</div>
</div>

</div>
    </div>
  </div>



