# ENM3440HW2

## Question
In this project, we seek to explore the answer to the causal question: What is the effect of rising grocery prices on the utilization of the WICS  program in the state of California?
## Introduction
The Women, Infants, and Children Supplemental Nutrition Program (WICS) is instrumental in offering essential nutritional assistance to vulnerable communities. As the cost of groceries escalates, it's imperative to ascertain how this upward trend impacts the dependence on the WICS program, especially in areas like California. By juxtaposing the Consumer Price Index (CPI) - a metric of grocery price fluctuations - with WICS program utilization data, this study aims to uncover potential causal connections. Such insights are pivotal for policymakers, ensuring that amidst rising living costs, the program remains effective and accessible.

## Data
The data for this study was acquired from data.gov, a website sponsored by the US government to provide free access data. 
### WICS Data
From the government website: 
The Women, Infants and Children (WIC) Program is a federally-funded health and nutrition program that provides assistance to pregnant women, new mothers, infants and children under age five. WIC helps California families by providing food benefits to individual participants, which can be used to purchase healthy supplemental foods from about 4,000 WIC authorized vendor stores throughout the State. WIC also provides nutritional education, breastfeeding support, healthcare referrals and other community services. Participants must meet income guidelines and other criteria. Currently, 84 WIC agencies provide services monthly to approximately one million participants at over 500 sites in local communities throughout the State.

To view the dataset and related information: https://catalog.data.gov/dataset/california-women-infants-and-children-program-redemption-by-county-7cf50

### CPI Data
From the government website: 
The Consumer Price Index (CPI) is a measure of the average change over time in the prices paid by urban consumers for a market basket of consumer goods and services. Indexes are available for the U.S. and various geographic areas. Average price data for select utility, automotive fuel, and food items are also available. Prices for the goods and services used to calculate the CPI are collected in 75 urban areas throughout the country and from about 23,000 retail and service establishments. Data on rents are collected from about 43,000 landlords or tenants.

To view the dataset and related information: https://catalog.data.gov/dataset/consumer-price-index-cpi-ee18b

## Data Loading and Preliminary Exploration

To start, we must clean the datasets and briefly analyzed their structure.

Code used: 

```
# Load the cleaned datasets
wics_cleaned = pd.read_csv('/mnt/data/WICS Data Original.csv')
cpi_cleaned = pd.read_excel('/mnt/data/CPI Data Correct.xlsx')

# Display the first few rows of each dataset to understand their structure
wics_cleaned.head(), cpi_cleaned.head()
```
The WICS dataset provides yearly data on the number of families that have redeemed benefits, among other metrics.
The CPI dataset gives the annual percentage change for different food categories, with the "All food" column indicating the overall change in food prices.
## Data Visualization
```
# Visualizing WICS program usage over time
plt.figure(figsize=(14, 7))
plt.plot(wics_grouped_cleaned['Year'], wics_grouped_cleaned['Number of Families Redeemed'], marker='o', color='blue')
plt.title('WICS Program Usage Over Time')
plt.xlabel('Year')
plt.ylabel('Number of Families Redeemed')
plt.grid(True)
plt.tight_layout()
plt.show()
```

General WICS Program Usage by Year:
![image](https://github.com/anjanabegur/ENM3440HW2/assets/143770425/90c7dc9a-b077-4432-9034-56d53aea62e5)

Visualizing Annual Percentage Change in Overall Food Prices:
```
# Visualizing annual percentage changes in overall food prices
plt.figure(figsize=(14, 7))
plt.plot(cpi_cleaned['Year'], cpi_cleaned['All food'], marker='o', color='green')
plt.title('Annual Percentage Change in Overall Food Prices')
plt.xlabel('Year')
plt.ylabel('Percentage Change in Food Prices')
plt.grid(True)
plt.tight_layout()
plt.show()

```
![image](https://github.com/anjanabegur/ENM3440HW2/assets/143770425/740d0f7e-8894-4b31-a158-7b2642402597)

## Merging Datasets and Further Visualization
To get a holistic view, we'll merge the WICS and CPI datasets based on the year and then visualize the relationship between the number of families redeemed and the percentage change in food prices.
```
# Merge the cleaned WICS and CPI datasets based on the year
merged_cleaned_data = wics_grouped_cleaned.merge(cpi_cleaned[['Year', 'All food']], on='Year', how='inner')

merged_cleaned_data.head()
```
To visually represent the relationship between the number of families using the WICS program and the changes in food prices, we'll create a scatter plot with a regression line. This will help us discern any patterns or correlations between the two metrics.
```
import seaborn as sns

# Scatter plot with regression line
plt.figure(figsize=(14, 7))
sns.regplot(x='All food', y='Number of Families Redeemed', data=merged_cleaned_data, scatter_kws={'s':100}, ci=None, color='purple')
plt.title('Relationship Between Food Price Changes and WICS Program Usage')
plt.xlabel('Percentage Change in Food Prices')
plt.ylabel('Number of Families Redeemed')
plt.grid(True)
plt.tight_layout()
plt.show()

```
![image](https://github.com/anjanabegur/ENM3440HW2/assets/143770425/7300cd1e-c6c2-4c91-9eef-ecd05dbe941f)

The scatter plot offers a visual representation of the relationship between the percentage change in food prices and the number of families utilizing the WICS program. The regression line hints at a positive correlation, suggesting that as food prices increase, there's a potential uptick in the number of families turning to the WICS program for support.

## Potential Confounders:
While brainstorming with ChatGPT, I was able to come up with various confounders:
Economic Conditions: Broader economic factors, such as unemployment rates, recessions, or economic booms, can influence both grocery prices and the number of families seeking assistance. For instance, during an economic downturn or when unemployment rates are high, more families might require assistance through the WICS program, irrespective of the current grocery prices.
Inflation or deflation can impact both grocery prices and the purchasing power of families.

Policy Changes: Changes in the eligibility criteria for the WICS program, funding levels, or the benefits provided can influence the number of families using the program. For example, if the criteria become more lenient, more families might be eligible, leading to increased program utilization.
State or federal policies that influence food prices, such as agricultural subsidies or tariffs on imported goods, could simultaneously affect grocery prices.

Outreach and Awareness: Increased outreach efforts, awareness campaigns, or streamlined application processes for the WICS program could boost its utilization, independent of grocery price dynamics.

Other Assistance Programs: The introduction, removal, or alteration of other assistance programs (like SNAP) can impact WICS program utilization. For example, if another program becomes more generous, it might reduce the reliance on WICS.

Local Events or Disasters: Natural disasters, strikes, or other localized events can disrupt food supply chains, leading to temporary spikes in grocery prices. These events can also lead to increased short-term dependence on assistance programs.

Demographic Changes: Changes in the population structure, like an increase in the number of children or pregnant women (key beneficiaries of WICS), could affect program utilization. Similarly, migration patterns, both within California and from outside, can influence the number of eligible participants.

Cost of Living: Areas with a higher cost of living might see more families seeking assistance, even if their incomes are relatively higher. This could be related to but distinct from grocery prices.

For the purposes of this project, we will consider the confounders of economic conditions and potential outreach efforts. 

Code for analysis:
```
import statsmodels.api as sm

# Define the independent variable (X) and dependent variable (y)
X = merged_cleaned_data['All food']
y = merged_cleaned_data['Number of Families Redeemed']

# Add a constant to the independent variable (standard practice in regression modeling)
X = sm.add_constant(X)

# Fit the regression model
model = sm.OLS(y, X).fit()

# Get the summary of the regression
model_summary = model.summary()
model_summary
```

The regression analysis provides us with the following insights:

The coefficient for "All food" (representing the percentage change in overall food prices) is approximately 1.617 x 10^6. This suggests that for every 1% increase in grocery prices, there is an estimated increase of approximately 1.617 million families using the WICS program.

The p-value for this coefficient is 0.146, which means that at the standard significance level of 0.05, the relationship between grocery prices and WICS program usage is not statistically significant. However, the p-value is still relatively low, indicating some evidence of a relationship.


## Potential Colliders:
A collider is a variable influenced by both the cause (rising grocery prices) and the effect (WICS program usage). Adjusting for colliders without caution can introduce bias in causal inference. Here are some potential colliders I came up with using ChatGPT:

Policy Changes in Response to Both Factors:
If both rising grocery prices and increased WICS program utilization lead to changes in state policies (like increased funding for the program or adjustments in eligibility criteria), then these policy changes would be colliders.

Public Perception or Media Coverage:
The media might increase coverage of the WICS program as a response to rising grocery prices. If this increased media attention then also drives more people to use the program (because they are now more aware of it), media coverage acts as a collider.

Economic Growth:
Suppose both grocery prices and WICS program utilization are rising due to rapid economic growth (grocery prices because of increased demand, and WICS utilization due to changes in the job market, like the gig economy, that don't offer stable benefits). In that case, economic growth could be a collider.

Local Food Production:
If areas with more WICS utilization also see increased local food production (as a way to support the community), and this increased production leads to rising food prices (maybe due to organic or specialty goods), then local food production could be a collider.

Community Initiatives:
Suppose communities with rising grocery prices start local initiatives or programs to support families, and these initiatives simultaneously increase awareness or accessibility of the W

## Conclusion:

The escalating cost of groceries and its potential impact on programs like WICS is a matter of significant concern. Our preliminary analysis suggests a possible correlation between rising grocery prices and increased reliance on the WICS program. As prices soar, more families might find themselves turning to these assistance programs. However, while this study provides insights, it's crucial to account for other influencing factors, like broader economic conditions and policy shifts. Policymakers should remain attuned to these dynamics to ensure assistance programs are robust, well-funded, and geared to meet the evolving needs of the community. Further studies, especially those considering potential confounders and colliders, would offer a more comprehensive understanding.

