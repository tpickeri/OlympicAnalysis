# OlympicAnalysis
This example uses Olympic medal data from the 2016 Summer games to explore the relationship between GDP and Olympic success.

##### SOURCE BEGIN #####
%% Does GDP Affect a Country's Olympic Success?
% In this example, we'll look at the relationship between a country's gross 
% domestic product (GDP) and its Olympic success.  We will use data from the Summer 
% 2016 games in this example.
%% Read Medals Data
% We'll start by reading the medals won by each country from an Excel file.  
% We can plot these values on a geobubble chart where the size of the bubble indicates 
% the number of medals won by that country.  We can see that the United States, 
% China, Russia, and the United Kingdom did quite well.

medals = readtable('olympic.xlsx');
f = figure;
f.Position = f.Position.*[1 1 1.8 1.2];
geobubble(medals, 'Latitude','Longitude','SizeVariable','Total');
title('2016 Summer Olympic Medals')
%% Read GDP data
% Read the data for the country's gross domestic product (GDP) from an Excel 
% file.

gdp = readtable('gdp.xlsx')
%% Join the Medals Data with the GDP Data
% We would like to see if there is a relationship between GDP and Olympic success.  
% To do that, we need to join the medals table with the table that contains the 
% GDP data.  A _join_ operation is a way to combine two tables of data using a 
% common key variable REPLACE_WITH_DASH_DASH in this case the country name.  Here we'll use the _*Join 
% Tables*_ Live Editor Task to combine the data.

% Join tables
medalsVsGDP = innerjoin(medals,gdp,'Keys','Country')
medalsVsGDP = sortrows(medalsVsGDP, 'Total', 'descend');
%% Plot Olympic Medals vs. GDP
% The last step is to plot the number of medals won against GDP for each country.  
% We can see that countries with high GDP tend to do better in the Olympics maybe 
% because they have more resources to spend on their athletes.

topNum = 10;
topMedals = medalsVsGDP(1:topNum,:);

f = figure;
f.Position = f.Position.*[1 1 1.8 1.2];
h_sc = scatter(medalsVsGDP.GDP, medalsVsGDP.Total);
h_sc.Parent.XScale = 'log';
hold on
h_sc = scatter(topMedals.GDP, topMedals.Total, 'om');
text(1.09*topMedals.GDP, topMedals.Total+2, string(topMedals.Country), ...
    'Color', [0.4 0.4 0.4], 'FontSize', 10)
hold off
xlabel('GDP'); ylabel('Number of Medals')
title(sprintf('Top %2d Medal-Winning Countries (Summer 2016)',topNum))
xlim([1000 100000000])
%% 
% Copyright (c) 2019, The MathWorks, Inc.
%% Attribution
% This example uses Olympic medal data from the Wikipedia article <https://en.wikipedia.org/wiki/2016_Summer_Olympics_medal_table 
% 2016_Summer_Olympics_medal_table> which is released under the <https://creativecommons.org/licenses/by-sa/3.0/ 
% Creative Commons Attribution-Share-Alike License 3.0>.  It also uses 2016 GDP 
% data from the Wikipedia article <https://en.wikipedia.org/wiki/List_of_countries_by_past_and_projected_GDP_(nominal) 
% List_of_countries_by_past_and_projected_GDP_(nominal)> which is released under 
% the <https://creativecommons.org/licenses/by-sa/3.0/ Creative Commons Attribution-Share-Alike 
% License 3.0>.
##### SOURCE END #####
