
<h1 align="center"> Lending Club Loan Analysis </h1> <br>


## Company Information:
Lending Club is a  peer to peer lending company based in the United States, in which investors provide funds for potential borrowers and investors earn a profit depending on the risk they take (the borrowers credit score). Lending Club provides the "bridge" between investors and borrowers. For more basic information about the company please check out the wikipedia article about the company. <br><br>


<a src="https://en.wikipedia.org/wiki/Lending_Club"> Lending Club Information </a>




## How Lending Club Works?
<img src="http://echeck.org/wp-content/uploads/2016/12/Showing-how-the-lending-club-works-and-makes-money-1.png"><br><br>


## Outline: <br><br>
I. Introduction <br>
a) [General Information](#general_information)<br>
b) [Similar Distributions](#similar_distributions)<br><br>

II. <b>Good Loans vs Bad Loans</b><br>
a) [Types of Loans](#types_of_loans)<br>
b) [Loans issued by Region](#by_region)<br>
c) [A Deeper Look into Bad Loans](#deeper_bad_loans)<br><br>

III. <b>The Business Perspective</b><br>
a) [Understanding the Operative side of Business](#operative_side)<br>
b) [Analysis by Income Category](#income_category) <br><br>

IV. <b>Assesing Risks</b><br>
a) [Understanding the Risky Side of Business](#risky_side)<br>
b) [The importance of Credit Scores](#credit_scores)<br>
c) [What determines a bad loan](#determines_bad_loan)<br>
d) [Defaulted Loans](#defaulted_loans)<br>
e) [Risks by Purposes](#loan_condition)



# Introduction:
## General Information:
<a id="general_information"></a>


```python
df = pd.read_csv('../lending-club-loan-dat.csv', low_memory=False)

# Copy of the dataframe
original_df = df.copy()

df.head()
```


        <script type="text/javascript">
        window.PlotlyConfig = {MathJaxConfig: 'local'};
        if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
        if (typeof require !== 'undefined') {
        require.undef("plotly");
        requirejs.config({
            paths: {
                'plotly': ['https://cdn.plot.ly/plotly-latest.min']
            }
        });
        require(['plotly'], function(Plotly) {
            window._Plotly = Plotly;
        });
        }
        </script>






<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>member_id</th>
      <th>loan_amnt</th>
      <th>funded_amnt</th>
      <th>funded_amnt_inv</th>
      <th>term</th>
      <th>int_rate</th>
      <th>installment</th>
      <th>grade</th>
      <th>sub_grade</th>
      <th>...</th>
      <th>hardship_payoff_balance_amount</th>
      <th>hardship_last_payment_amount</th>
      <th>disbursement_method</th>
      <th>debt_settlement_flag</th>
      <th>debt_settlement_flag_date</th>
      <th>settlement_status</th>
      <th>settlement_date</th>
      <th>settlement_amount</th>
      <th>settlement_percentage</th>
      <th>settlement_term</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>2500</td>
      <td>2500</td>
      <td>2500.0</td>
      <td>36 months</td>
      <td>13.56</td>
      <td>84.92</td>
      <td>C</td>
      <td>C1</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cash</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>30000</td>
      <td>30000</td>
      <td>30000.0</td>
      <td>60 months</td>
      <td>18.94</td>
      <td>777.23</td>
      <td>D</td>
      <td>D2</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cash</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>5000</td>
      <td>5000</td>
      <td>5000.0</td>
      <td>36 months</td>
      <td>17.97</td>
      <td>180.69</td>
      <td>D</td>
      <td>D1</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cash</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>4000</td>
      <td>4000</td>
      <td>4000.0</td>
      <td>36 months</td>
      <td>18.94</td>
      <td>146.51</td>
      <td>D</td>
      <td>D2</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cash</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>30000</td>
      <td>30000</td>
      <td>30000.0</td>
      <td>60 months</td>
      <td>16.14</td>
      <td>731.78</td>
      <td>C</td>
      <td>C4</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Cash</td>
      <td>N</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 145 columns</p>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2260668 entries, 0 to 2260667
    Columns: 145 entries, id to settlement_term
    dtypes: float64(105), int64(4), object(36)
    memory usage: 2.4+ GB


## Similar Distributions:
<a id="similar_distributions"></a>
We will start by exploring the distribution of the loan amounts and see when did the loan amount issued increased significantly. <br>

<h4> What we need to know: </h4> <br>
<ul>
<li> Understand what amount was <b>mostly issued</b> to borrowers. </li>
<li> Which <b>year</b> issued the most loans. </li>
<li> The distribution of loan amounts is a <b>multinomial distribution </b>.</li>
</ul>



<h4> Summary: </h4><br>
<ul>
<li> Most of the <b>loans issued</b> were in the range of 10,000 to 20,000 USD. </li>
<li> The <b>year of 2018</b> was the year were most loans were issued.</li>
<li> Loans were issued in an <b>incremental manner</b>. (Possible due to a recovery in the U.S economy) </li>
<li> The loans <b>applied</b> by potential borrowers, the amount <b>issued</b> to the borrowers and the amount <b>funded</b> by investors are similarly distributed, <b>meaning</b> that it is most likely that qualified borrowers are going to get the loan they had applied for. </li>

</ul>

![png](output_6_2.png)


```python
# The year of 2018 was the year were the highest amount of loans were issued
# This is an indication that the economy is quiet recovering itself.
plt.figure(figsize=(12,8))
sns.barplot(x = 'year', y ='loan_amount', data = df, palette ='tab10')
plt.title('Issuance of Loans', fontsize=16)
plt.xlabel('Year', fontsize=14)
plt.ylabel('Average loan amount issued', fontsize=14)
```

![png](output_8_2.png)


<h1 align="center"> Good Loans vs Bad Loans: </h1>
<h2>Types of Loans: </h2>
<a id="types_of_loans"></a>

<br><br>
In this section, we will see what is the amount of bad loans Lending Club has declared so far, of course we have to understand that there are still loans that are at a risk of defaulting in the future.

<h4> What we need to know: </h4>
<ul>
<li> The amount of bad loans could <b>increment</b> as the days pass by, since we still have a great amount of current loans. </li>
<li> <b>Average annual income</b> is an important key metric for finding possible opportunities of investments in a specific region. </li>

</ul>

<h4> Summary: </h4>
<ul>
<li> Currently, <b>bad loans</b> consist 13.14% of total loans but remember that we still have <b>current loans</b> which have the risk of becoming bad loans. (So this percentage is subjected to possible changes.) </li>
<li> The <b> NorthEast </b> region seems to be the most attractive in term of funding loans to borrowers. </li>
<li> The <b> SouthWest </b> and <b> West</b> regions have experienced a slight increase in the "median income" in the past years. </li>
<li> <b>Average interest</b> rates have declined since 2012 but this might explain the <b>increase in the volume</b> of loans.  </li>
<li> <b>Employment Length</b> tends to be greater in the regions of the <b>SouthWest</b> and <b>West</b></li>
<li> Clients located in the regions of <b>NorthEast</b> and <b>MidWest</b> have not experienced a drastic increase in debt-to-income(dti) as compared to the other regions. </li>
</ul>


```python
df["loan_status"].value_counts()
```




    Fully Paid                                             1041952
    Current                                                 919695
    Charged Off                                             261655
    Late (31-120 days)                                       21897
    In Grace Period                                           8952
    Late (16-30 days)                                         3737
    Does not meet the credit policy. Status:Fully Paid        1988
    Does not meet the credit policy. Status:Charged Off        761
    Default                                                     31
    Name: loan_status, dtype: int64


![png](output_12_2.png)


<h2> Loans Issued by Region</h2>
<a id="by_region"></a>
In this section we want to analyze loans issued by region in order to see region patters that will allow us to understand which region gives Lending Club.<br><br>

## Summary: <br>
<ul>
<li> <b> SouthEast</b> , <b>West </b> and <b>NorthEast</b> regions had the highest amount lof loans issued. </li>
<li> <b>West </b> and <b>SouthWest </b> had a rapid increase in debt-to-income starting in 2012. </li>
<li><b>West </b> and <b>SouthWest </b>  had a rapid decrease in interest rates (This might explain the increase in debt to income). </li>
</ul>


![png](output_16_1.png)


    <matplotlib.legend.Legend at 0x1302ee390>

![png](output_18_1.png)


## A Deeper Look into Bad Loans:
<a id="deeper_bad_loans"></a>

<h4> What we need to know: </h4>
<ul>
<li>The number of loans that were classified as bad loans for each region by its <b>loan status</b>. (This will be shown in a dataframe below.)</li>
<li> This won't give us the exact reasons why a loan is categorized as a bad loan (other variables that might have influence the condition of the loan) but it will give us a <b> deeper insight on the level of risk </b> in a particular region. </li>
</ul>

<h4> Summary: </h4>
<ul>
<li>The regions of the <b> West </b> and <b> SouthEast </b> had a higher percentage in most of the b "bad" loan statuses.</li>
<li> The <b>NorthEast</b> region had a higher percentage in <b>Grace Period</b> and <b>Does not meet Credit Policy</b> loan status. However, both of these are not considered as bad as <b>default</b> for instance. </li>
<li> Based on this small and brief summary we can conclude that the <b>West</b> and <b>SouthEast</b> regions have the most undesirable loan status, but just by a slightly higher percentage compared to the <b>NorthEast</b> region. </li>
<li> Again, this does not tell us what causes a loan to be a <b> bad loan </b>, but it gives us some idea about <b>the level of risk</b> within the regions across the United States. </li>
</ul>

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>loan_status</th>
      <th>Charged Off</th>
      <th>Default</th>
      <th>Does not meet the credit policy. Status:Charged Off</th>
      <th>In Grace Period</th>
      <th>Late (16-30 days)</th>
      <th>Late (31-120 days)</th>
      <th>Total</th>
    </tr>
    <tr>
      <th>region</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>MidWest</th>
      <td>45202</td>
      <td>9</td>
      <td>142</td>
      <td>1449</td>
      <td>550</td>
      <td>3609</td>
      <td>50961</td>
    </tr>
    <tr>
      <th>NorthEast</th>
      <td>60827</td>
      <td>7</td>
      <td>190</td>
      <td>2356</td>
      <td>957</td>
      <td>5356</td>
      <td>69693</td>
    </tr>
    <tr>
      <th>SouthEast</th>
      <td>65460</td>
      <td>5</td>
      <td>184</td>
      <td>2359</td>
      <td>974</td>
      <td>5596</td>
      <td>74578</td>
    </tr>
    <tr>
      <th>SouthWest</th>
      <td>31833</td>
      <td>4</td>
      <td>79</td>
      <td>1009</td>
      <td>463</td>
      <td>2794</td>
      <td>36182</td>
    </tr>
    <tr>
      <th>West</th>
      <td>58333</td>
      <td>6</td>
      <td>166</td>
      <td>1779</td>
      <td>793</td>
      <td>4542</td>
      <td>65619</td>
    </tr>
  </tbody>
</table>
</div>



<div>


            <div id="0db65c78-4fab-4d32-b2fe-d3347bcd4b6b" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("0db65c78-4fab-4d32-b2fe-d3347bcd4b6b")) {
                    Plotly.newPlot(
                        '0db65c78-4fab-4d32-b2fe-d3347bcd4b6b',
                        [{"marker": {"color": "rgb(192, 148, 246)"}, "name": "Charged Off", "text": "%", "type": "bar", "uid": "87969956-17dc-45b9-8ab7-e5a3218fcca7", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [17.28, 23.25, 25.02, 12.17, 22.29]}, {"marker": {"color": "rgb(176, 26, 26)"}, "name": "Defaults", "text": "%", "type": "bar", "uid": "6f2ee606-578a-4c96-b8b9-55c73593f98a", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [29.03, 22.58, 16.13, 12.9, 19.35]}, {"marker": {"color": "rgb(229, 121, 36)"}, "name": "Does not meet Credit Policy", "text": "%", "type": "bar", "uid": "621d50b6-2b69-4ea4-95c3-6d96b200715b", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [18.66, 24.97, 24.18, 10.38, 21.81]}, {"marker": {"color": "rgb(147, 147, 147)"}, "name": "Grace Period", "text": "%", "type": "bar", "uid": "09c211b0-e547-40a1-a9fa-12493480e52e", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [16.19, 26.32, 26.35, 11.27, 19.87]}, {"marker": {"color": "rgb(246, 157, 135)"}, "name": "Late Payment (16-30 days)", "text": "%", "type": "bar", "uid": "a5177a7f-8688-4eec-8832-c5c3e7579ff3", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [14.72, 25.61, 26.06, 12.39, 21.22]}, {"marker": {"color": "rgb(238, 76, 73)"}, "name": "Late Payment (31-120 days)", "text": "%", "type": "bar", "uid": "acf7497e-fae9-432d-87d0-1ac788ce19fd", "x": ["MidWest", "NorthEast", "SouthEast", "SouthWest", "West"], "y": [16.48, 24.46, 25.56, 12.76, 20.74]}],
                        {"barmode": "stack", "title": {"text": "% of Bad Loan Status by Region"}, "xaxis": {"title": {"text": "US Regions"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('0db65c78-4fab-4d32-b2fe-d3347bcd4b6b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
# Average interest rates clients pay
df['interest_rate'].mean()
```




    13.09291294431558




```python
# Average annual income of clients
df['annual_income'].mean()
```




    77992.42868706721



<h1 align="center"> The Business Perspective </h1>
<h2 > Understanding the Operative Side of Business </h2>
<a id="operative_side"></a>


Now we will have a closer look at the <b> operative side </b> of business by state. This will give us a clearer idea in which state we have a higher operating activity. This will allow us to ask further questions such as Why do we have a higher level of operating activity in this state? Could it be because of economic factors? or the risk level is low and returns are fairly decent? Let's explore!

<h4> What we need to know: </h4>
<ul>
<li> We will focus on <b>three key metrics</b>: Loans issued by state (Total Sum), Average interest rates charged to customers and average annual income of all customers by state. </li>
<li> The purpose of this analysis is to see states that give high returns at a descent risk. </li>

</ul>

<h4> Summary: </h4>
<ul>
<li> <b>California, Texas, New York and Florida</b> are the states in which the highest amount of loans were issued. </li>
<li> Interesting enough, all four states have a approximate <b>interest rate of 13%</b> which is at the same level of the average interest rate for all states (13.09%) </li>
<li> California, Texas and New York are <b>all above the average annual income</b> (with the exclusion of Florida), this might give possible indication why most loans are issued in these states. </li>
</ul>




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state_codes</th>
      <th>issued_loans</th>
      <th>interest_rate</th>
      <th>annual_income</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>IA</td>
      <td>114075</td>
      <td>12.63</td>
      <td>44756.21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>IL</td>
      <td>1410451950</td>
      <td>12.96</td>
      <td>79884.92</td>
    </tr>
    <tr>
      <th>2</th>
      <td>IN</td>
      <td>550776675</td>
      <td>13.18</td>
      <td>70365.19</td>
    </tr>
    <tr>
      <th>3</th>
      <td>KS</td>
      <td>283877825</td>
      <td>13.00</td>
      <td>71456.55</td>
    </tr>
    <tr>
      <th>4</th>
      <td>MI</td>
      <td>841646100</td>
      <td>13.16</td>
      <td>71660.97</td>
    </tr>
  </tbody>
</table>
</div>



<div>


            <div id="8f1abd8f-86cf-4627-ad87-36d4a47c4380" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("8f1abd8f-86cf-4627-ad87-36d4a47c4380")) {
                    Plotly.newPlot(
                        '8f1abd8f-86cf-4627-ad87-36d4a47c4380',
                        [{"autocolorscale": false, "colorbar": {"title": {"text": "$s USD"}}, "colorscale": [[0.0, "rgb(210, 241, 198)"], [0.2, "rgb(188, 236, 169)"], [0.4, "rgb(171, 235, 145)"], [0.6, "rgb(140, 227, 105)"], [0.8, "rgb(105, 201, 67)"], [1.0, "rgb(59, 159, 19)"]], "locationmode": "USA-states", "locations": ["IA", "IL", "IN", "KS", "MI", "MN", "MO", "ND", "NE", "OH", "SD", "WI", "CT", "MA", "MD", "ME", "NH", "NJ", "NY", "PA", "RI", "VT", "AL", "AR", "DC", "DE", "FL", "GA", "KY", "LA", "MS", "NC", "SC", "TN", "VA", "WV", "AZ", "NM", "OK", "TX", "AK", "CA", "CO", "HI", "ID", "MT", "NV", "OR", "UT", "WA", "WY"], "marker": {"line": {"color": "rgb(255,255,255)", "width": 2}}, "text": ["IA<br>Average loan interest rate: 12.63<br>Average annual income: 44756.21", "IL<br>Average loan interest rate: 12.96<br>Average annual income: 79884.92", "IN<br>Average loan interest rate: 13.18<br>Average annual income: 70365.19", "KS<br>Average loan interest rate: 13.0<br>Average annual income: 71456.55", "MI<br>Average loan interest rate: 13.16<br>Average annual income: 71660.97", "MN<br>Average loan interest rate: 12.97<br>Average annual income: 73239.38", "MO<br>Average loan interest rate: 13.05<br>Average annual income: 70177.88", "ND<br>Average loan interest rate: 13.25<br>Average annual income: 76433.85", "NE<br>Average loan interest rate: 13.18<br>Average annual income: 65789.44", "OH<br>Average loan interest rate: 13.14<br>Average annual income: 69322.61", "SD<br>Average loan interest rate: 13.22<br>Average annual income: 65890.03", "WI<br>Average loan interest rate: 12.85<br>Average annual income: 69053.08", "CT<br>Average loan interest rate: 13.05<br>Average annual income: 86531.03", "MA<br>Average loan interest rate: 12.68<br>Average annual income: 82788.66", "MD<br>Average loan interest rate: 13.24<br>Average annual income: 86919.01", "ME<br>Average loan interest rate: 12.82<br>Average annual income: 67713.61", "NH<br>Average loan interest rate: 12.7<br>Average annual income: 79110.57", "NJ<br>Average loan interest rate: 12.99<br>Average annual income: 89920.51", "NY<br>Average loan interest rate: 13.26<br>Average annual income: 81057.05", "PA<br>Average loan interest rate: 13.15<br>Average annual income: 73940.35", "RI<br>Average loan interest rate: 13.03<br>Average annual income: 74607.28", "VT<br>Average loan interest rate: 12.99<br>Average annual income: 66577.67", "AL<br>Average loan interest rate: 13.57<br>Average annual income: 70854.75", "AR<br>Average loan interest rate: 13.35<br>Average annual income: 67588.15", "DC<br>Average loan interest rate: 12.58<br>Average annual income: 94553.07", "DE<br>Average loan interest rate: 13.2<br>Average annual income: 76689.29", "FL<br>Average loan interest rate: 13.16<br>Average annual income: 73171.61", "GA<br>Average loan interest rate: 13.21<br>Average annual income: 77857.57", "KY<br>Average loan interest rate: 13.19<br>Average annual income: 69534.68", "LA<br>Average loan interest rate: 13.23<br>Average annual income: 75669.09", "MS<br>Average loan interest rate: 13.45<br>Average annual income: 71625.13", "NC<br>Average loan interest rate: 13.19<br>Average annual income: 73853.2", "SC<br>Average loan interest rate: 13.27<br>Average annual income: 73137.99", "TN<br>Average loan interest rate: 13.29<br>Average annual income: 70665.98", "VA<br>Average loan interest rate: 13.11<br>Average annual income: 86071.97", "WV<br>Average loan interest rate: 13.12<br>Average annual income: 69514.91", "AZ<br>Average loan interest rate: 12.96<br>Average annual income: 74186.97", "NM<br>Average loan interest rate: 13.16<br>Average annual income: 71566.73", "OK<br>Average loan interest rate: 13.26<br>Average annual income: 72413.97", "TX<br>Average loan interest rate: 13.0<br>Average annual income: 82728.81", "AK<br>Average loan interest rate: 13.33<br>Average annual income: 79823.04", "CA<br>Average loan interest rate: 12.98<br>Average annual income: 83847.98", "CO<br>Average loan interest rate: 12.89<br>Average annual income: 76948.31", "HI<br>Average loan interest rate: 13.79<br>Average annual income: 74696.93", "ID<br>Average loan interest rate: 13.3<br>Average annual income: 65500.36", "MT<br>Average loan interest rate: 12.96<br>Average annual income: 64079.48", "NV<br>Average loan interest rate: 13.21<br>Average annual income: 73480.53", "OR<br>Average loan interest rate: 12.95<br>Average annual income: 68737.99", "UT<br>Average loan interest rate: 13.1<br>Average annual income: 75169.24", "WA<br>Average loan interest rate: 13.16<br>Average annual income: 77462.93", "WY<br>Average loan interest rate: 13.21<br>Average annual income: 72941.19"], "type": "choropleth", "uid": "13cf63e9-5a91-456a-9ba3-32de16455264", "z": ["114075", "1410451950", "550776675", "283877825", "841646100", "577447675", "524098650", "55622875", "110941450", "1077142925", "64670375", "431766050", "547556950", "811746725", "856883500", "73320525", "166469500", "1316207975", "2767160700", "1131987950", "142960100", "68206775", "400699250", "240650500", "84707250", "96450975", "2333034500", "1136790825", "313557625", "382046275", "186427150", "930436100", "418481900", "522342125", "1013015875", "125954800", "780966125", "178280525", "310665375", "2931133525", "90419900", "4808480100", "727724225", "169562100", "62088425", "88170825", "470165300", "379466875", "227578525", "721608300", "74153325"]}],
                        {"geo": {"lakecolor": "rgb(255, 255, 255)", "projection": {"type": "albers usa"}, "scope": "usa", "showlakes": true}, "title": {"text": "Lending Clubs Issued Loans <br> (A Perspective for the Business Operations)"}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('8f1abd8f-86cf-4627-ad87-36d4a47c4380');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>


## Analysis by Income Category:
<a id="income_category"></a>
In this section we will create different <b> income categories </b> in order to detect important patters and go more into depth in our analysis.

**What we need to know:** <br>
<ul>
<li><b>Low income category:</b> Borrowers that have an annual income lower or equal to 100,000 usd.</li>
<li> <b> Medium income category:</b> Borrowers that have an annual income higher than 100,000 usd but lower or equal to 200,000 usd. </li>
<li><b> High income category: </b> Borrowers that have an annual income higher tha 200,000 usd. </li>
</ul>

**Summary:**
<ul>
<li>Borrowers that made part of the <b>high income category</b> took higher loan amounts than people from <b>low</b> and <b>medium income categories.</b> Of course, people with higher annual incomes are more likely to pay loans with a higher amount. (First row to the left of the subplots) </li>
<li> Loans that were borrowed by the <b>Low income category</b> had a slightly higher change of becoming a bad loan. (First row to the right of the subplots) </li>
<li>Borrowers with <b>High</b> and <b> Medium</b> annual incomes had a longer employment length than people with lower incomes.(Second row to the left of the subplots) </li>
<li> Borrowers with a lower income had on average <b>higher interest rates</b> while people with a higher annual income had <b>lower interest rates</b> on their loans. (Second row to the right of the subplots)</li>

</ul>




![png](output_30_2.png)


<h1 align="center"> Assesing Risks </h1>
<h2> Understanding the Risky side of Business </h2>
<a id="risky_side"></a>

Although the <b> operative side of business </b> is important, we have to also analyze the level of risk in each state. Credit scores are important metrics to analyze the level of risk of an individual customer. However, there are also other important metrics to somehow estimate the level of risk of other states. <br><br>

<h4> What we need to know: </h4>
<ul>
<li> <b>Debt-to-income</b> is an important metric since it says approximately the level of debt of each individual consumer with respect to its total income. </li>
<li> The <b>average length of employment</b> tells us a better story about the labor market in each state which is helpful to assess the levelof risk. </li>
</ul>

<h4> Summary: </h4>
<ul>
<li> <b>IOWA</b> has the highest level of default ratio neverthless, the amount of loans issued in that state is <b>too low</b>. (Number of Bad loans is equal to 3) </li>
<li> California and Texas seem to have the lowest risk and the highest possible return for investors. However, I will look more deeply into these states and create other metrics analyze the level of risk for each state. </li>

</ul>



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>state_codes</th>
      <th>default_ratio</th>
      <th>badloans_amount</th>
      <th>percentage_of_badloans</th>
      <th>average_dti</th>
      <th>average_emp_length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AK</td>
      <td>0.153</td>
      <td>696</td>
      <td>0.234</td>
      <td>13.656</td>
      <td>6.136</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AL</td>
      <td>0.188</td>
      <td>4308</td>
      <td>1.450</td>
      <td>18.739</td>
      <td>6.463</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AR</td>
      <td>0.183</td>
      <td>2642</td>
      <td>0.889</td>
      <td>20.327</td>
      <td>6.254</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AZ</td>
      <td>0.150</td>
      <td>7028</td>
      <td>2.366</td>
      <td>20.414</td>
      <td>5.680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CA</td>
      <td>0.155</td>
      <td>42319</td>
      <td>14.247</td>
      <td>19.240</td>
      <td>5.947</td>
    </tr>
  </tbody>
</table>
</div>


<div>


            <div id="a8cbbb9a-0e9b-4501-9e45-86234f0ceb92" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("a8cbbb9a-0e9b-4501-9e45-86234f0ceb92")) {
                    Plotly.newPlot(
                        'a8cbbb9a-0e9b-4501-9e45-86234f0ceb92',
                        [{"autocolorscale": false, "colorbar": {"title": {"text": "%"}}, "colorscale": [[0.0, "rgb(202, 202, 202)"], [0.2, "rgb(253, 205, 200)"], [0.4, "rgb(252, 169, 161)"], [0.6, "rgb(247, 121, 108  )"], [0.8, "rgb(232, 70, 54)"], [1.0, "rgb(212, 31, 13)"]], "locationmode": "USA-states", "locations": ["AK", "AL", "AR", "AZ", "CA", "CO", "CT", "DC", "DE", "FL", "GA", "HI", "IA", "ID", "IL", "IN", "KS", "KY", "LA", "MA", "MD", "ME", "MI", "MN", "MO", "MS", "MT", "NC", "ND", "NE", "NH", "NJ", "NM", "NV", "NY", "OH", "OK", "OR", "PA", "RI", "SC", "SD", "TN", "TX", "UT", "VA", "VT", "WA", "WI", "WV", "WY"], "marker": {"line": {"color": "rgb(255,255,255)", "width": 2}}, "text": ["AK<br>Number of Bad Loans: 696<br>Percentage of all Bad Loans: 0.234%<br>Average Debt-to-Income Ratio: 13.656<br>Average Length of Employment: 6.136", "AL<br>Number of Bad Loans: 4308<br>Percentage of all Bad Loans: 1.45%<br>Average Debt-to-Income Ratio: 18.739<br>Average Length of Employment: 6.463", "AR<br>Number of Bad Loans: 2642<br>Percentage of all Bad Loans: 0.889%<br>Average Debt-to-Income Ratio: 20.327<br>Average Length of Employment: 6.254", "AZ<br>Number of Bad Loans: 7028<br>Percentage of all Bad Loans: 2.366%<br>Average Debt-to-Income Ratio: 20.414<br>Average Length of Employment: 5.68", "CA<br>Number of Bad Loans: 42319<br>Percentage of all Bad Loans: 14.247%<br>Average Debt-to-Income Ratio: 19.24<br>Average Length of Employment: 5.947", "CO<br>Number of Bad Loans: 5075<br>Percentage of all Bad Loans: 1.709%<br>Average Debt-to-Income Ratio: 19.205<br>Average Length of Employment: 5.389", "CT<br>Number of Bad Loans: 3917<br>Percentage of all Bad Loans: 1.319%<br>Average Debt-to-Income Ratio: 20.083<br>Average Length of Employment: 6.185", "DC<br>Number of Bad Loans: 524<br>Percentage of all Bad Loans: 0.176%<br>Average Debt-to-Income Ratio: 21.424<br>Average Length of Employment: 4.841", "DE<br>Number of Bad Loans: 833<br>Percentage of all Bad Loans: 0.28%<br>Average Debt-to-Income Ratio: 20.614<br>Average Length of Employment: 6.29", "FL<br>Number of Bad Loans: 22842<br>Percentage of all Bad Loans: 7.69%<br>Average Debt-to-Income Ratio: 20.25<br>Average Length of Employment: 5.765", "GA<br>Number of Bad Loans: 8923<br>Percentage of all Bad Loans: 3.004%<br>Average Debt-to-Income Ratio: 21.567<br>Average Length of Employment: 6.002", "HI<br>Number of Bad Loans: 1506<br>Percentage of all Bad Loans: 0.507%<br>Average Debt-to-Income Ratio: 19.925<br>Average Length of Employment: 6.506", "IA<br>Number of Bad Loans: 3<br>Percentage of all Bad Loans: 0.001%<br>Average Debt-to-Income Ratio: 17.728<br>Average Length of Employment: 3.464", "ID<br>Number of Bad Loans: 372<br>Percentage of all Bad Loans: 0.125%<br>Average Debt-to-Income Ratio: 17.263<br>Average Length of Employment: 5.591", "IL<br>Number of Bad Loans: 10325<br>Percentage of all Bad Loans: 3.476%<br>Average Debt-to-Income Ratio: 18.301<br>Average Length of Employment: 6.062", "IN<br>Number of Bad Loans: 5063<br>Percentage of all Bad Loans: 1.705%<br>Average Debt-to-Income Ratio: 21.043<br>Average Length of Employment: 6.177", "KS<br>Number of Bad Loans: 2055<br>Percentage of all Bad Loans: 0.692%<br>Average Debt-to-Income Ratio: 19.43<br>Average Length of Employment: 6.168", "KY<br>Number of Bad Loans: 2940<br>Percentage of all Bad Loans: 0.99%<br>Average Debt-to-Income Ratio: 17.349<br>Average Length of Employment: 6.298", "LA<br>Number of Bad Loans: 3966<br>Percentage of all Bad Loans: 1.335%<br>Average Debt-to-Income Ratio: 16.837<br>Average Length of Employment: 6.024", "MA<br>Number of Bad Loans: 6559<br>Percentage of all Bad Loans: 2.208%<br>Average Debt-to-Income Ratio: 19.717<br>Average Length of Employment: 5.873", "MD<br>Number of Bad Loans: 7538<br>Percentage of all Bad Loans: 2.538%<br>Average Debt-to-Income Ratio: 17.825<br>Average Length of Employment: 6.175", "ME<br>Number of Bad Loans: 342<br>Percentage of all Bad Loans: 0.115%<br>Average Debt-to-Income Ratio: 20.857<br>Average Length of Employment: 6.091", "MI<br>Number of Bad Loans: 7857<br>Percentage of all Bad Loans: 2.645%<br>Average Debt-to-Income Ratio: 20.635<br>Average Length of Employment: 6.311", "MN<br>Number of Bad Loans: 5195<br>Percentage of all Bad Loans: 1.749%<br>Average Debt-to-Income Ratio: 21.045<br>Average Length of Employment: 5.897", "MO<br>Number of Bad Loans: 4999<br>Percentage of all Bad Loans: 1.683%<br>Average Debt-to-Income Ratio: 15.727<br>Average Length of Employment: 6.119", "MS<br>Number of Bad Loans: 1971<br>Percentage of all Bad Loans: 0.664%<br>Average Debt-to-Income Ratio: 19.532<br>Average Length of Employment: 6.442", "MT<br>Number of Bad Loans: 705<br>Percentage of all Bad Loans: 0.237%<br>Average Debt-to-Income Ratio: 18.934<br>Average Length of Employment: 5.931", "NC<br>Number of Bad Loans: 8642<br>Percentage of all Bad Loans: 2.909%<br>Average Debt-to-Income Ratio: 19.261<br>Average Length of Employment: 5.985", "ND<br>Number of Bad Loans: 382<br>Percentage of all Bad Loans: 0.129%<br>Average Debt-to-Income Ratio: 20.262<br>Average Length of Employment: 5.139", "NE<br>Number of Bad Loans: 1008<br>Percentage of all Bad Loans: 0.339%<br>Average Debt-to-Income Ratio: 19.95<br>Average Length of Employment: 5.774", "NH<br>Number of Bad Loans: 1066<br>Percentage of all Bad Loans: 0.359%<br>Average Debt-to-Income Ratio: 20.964<br>Average Length of Employment: 6.101", "NJ<br>Number of Bad Loans: 11317<br>Percentage of all Bad Loans: 3.81%<br>Average Debt-to-Income Ratio: 19.343<br>Average Length of Employment: 6.057", "NM<br>Number of Bad Loans: 1726<br>Percentage of all Bad Loans: 0.581%<br>Average Debt-to-Income Ratio: 20.031<br>Average Length of Employment: 6.145", "NV<br>Number of Bad Loans: 4796<br>Percentage of all Bad Loans: 1.615%<br>Average Debt-to-Income Ratio: 20.527<br>Average Length of Employment: 5.907", "NY<br>Number of Bad Loans: 26918<br>Percentage of all Bad Loans: 9.062%<br>Average Debt-to-Income Ratio: 19.055<br>Average Length of Employment: 6.001", "OH<br>Number of Bad Loans: 9779<br>Percentage of all Bad Loans: 3.292%<br>Average Debt-to-Income Ratio: 21.031<br>Average Length of Employment: 6.247", "OK<br>Number of Bad Loans: 3150<br>Percentage of all Bad Loans: 1.06%<br>Average Debt-to-Income Ratio: 19.12<br>Average Length of Employment: 5.985", "OR<br>Number of Bad Loans: 2634<br>Percentage of all Bad Loans: 0.887%<br>Average Debt-to-Income Ratio: 20.958<br>Average Length of Employment: 5.713", "PA<br>Number of Bad Loans: 10447<br>Percentage of all Bad Loans: 3.517%<br>Average Debt-to-Income Ratio: 20.621<br>Average Length of Employment: 6.111", "RI<br>Number of Bad Loans: 1164<br>Percentage of all Bad Loans: 0.392%<br>Average Debt-to-Income Ratio: 19.894<br>Average Length of Employment: 6.285", "SC<br>Number of Bad Loans: 2938<br>Percentage of all Bad Loans: 0.989%<br>Average Debt-to-Income Ratio: 19.293<br>Average Length of Employment: 6.063", "SD<br>Number of Bad Loans: 641<br>Percentage of all Bad Loans: 0.216%<br>Average Debt-to-Income Ratio: 17.234<br>Average Length of Employment: 5.914", "TN<br>Number of Bad Loans: 4831<br>Percentage of all Bad Loans: 1.626%<br>Average Debt-to-Income Ratio: 18.811<br>Average Length of Employment: 6.045", "TX<br>Number of Bad Loans: 24278<br>Percentage of all Bad Loans: 8.174%<br>Average Debt-to-Income Ratio: 19.569<br>Average Length of Employment: 5.742", "UT<br>Number of Bad Loans: 1864<br>Percentage of all Bad Loans: 0.628%<br>Average Debt-to-Income Ratio: 22.258<br>Average Length of Employment: 5.768", "VA<br>Number of Bad Loans: 8395<br>Percentage of all Bad Loans: 2.826%<br>Average Debt-to-Income Ratio: 20.563<br>Average Length of Employment: 6.009", "VT<br>Number of Bad Loans: 425<br>Percentage of all Bad Loans: 0.143%<br>Average Debt-to-Income Ratio: 19.105<br>Average Length of Employment: 6.264", "WA<br>Number of Bad Loans: 5108<br>Percentage of all Bad Loans: 1.72%<br>Average Debt-to-Income Ratio: 18.565<br>Average Length of Employment: 5.84", "WI<br>Number of Bad Loans: 3654<br>Percentage of all Bad Loans: 1.23%<br>Average Debt-to-Income Ratio: 20.158<br>Average Length of Employment: 6.182", "WV<br>Number of Bad Loans: 823<br>Percentage of all Bad Loans: 0.277%<br>Average Debt-to-Income Ratio: 18.741<br>Average Length of Employment: 6.439", "WY<br>Number of Bad Loans: 544<br>Percentage of all Bad Loans: 0.183%<br>Average Debt-to-Income Ratio: 21.204<br>Average Length of Employment: 6.039"], "type": "choropleth", "uid": "e61a450a-74d9-4694-ac70-b5ed67308240", "z": ["0.153", "0.188", "0.183", "0.15", "0.155", "0.118", "0.123", "0.108", "0.148", "0.164", "0.137", "0.164", "0.273", "0.095", "0.128", "0.156", "0.12", "0.155", "0.182", "0.145", "0.162", "0.074", "0.154", "0.151", "0.161", "0.185", "0.126", "0.16", "0.119", "0.148", "0.106", "0.158", "0.168", "0.172", "0.169", "0.15", "0.18", "0.109", "0.157", "0.132", "0.117", "0.164", "0.158", "0.15", "0.142", "0.154", "0.094", "0.122", "0.139", "0.109", "0.129"]}],
                        {"geo": {"lakecolor": "rgb(255, 255, 255)", "projection": {"type": "albers usa"}, "scope": "usa", "showlakes": true}, "title": {"text": "Lending Clubs Default Rates <br> (Analyzing Risks)"}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('a8cbbb9a-0e9b-4501-9e45-86234f0ceb92');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>


## The Importance of Credit Scores:
<a id="credit_scores"></a>
Credit scores are important metrics for assesing the overall level of risk. In this section we will analyze the level of risk as a whole and how many loans were bad loans by the type of grade received in the credit score of the customer.

<h4> What we need to know: </h4>
<ul>
<li> The lower the grade of the credit score, the higher the risk for investors. </li>
<li> There are different factors that influence on the level of risk of the loan.</li>
</ul>

<h4> Summary: </h4>
<ul>
<li> The scores that has a lower grade received a larger amounts of loans (which might had contributed to a higher level of risk). </li>
<li> Logically, the <b>lower the grade the higher the interest</b> the customer had to pay back to investors.</li>
<li> Interstingly, customers with a <b>grade</b> of "C" were more likely to default on the loan </li>
<ul>

![png](output_35_1.png)



![png](output_36_1.png)


<h2>What Determines a Bad Loan </h2>
<a id="determines_bad_loan"></a>
My main aim in this section is to find the main factors that causes for a loan to be considered a <b>"Bad Loan"</b>. Logically, we could assume that factors such as a low credit grade or a high debt to income could be possible contributors in determining whether a loan is at a high risk of being defaulted. <br><br>

<h4> What we need to know: </h4>
<ul>
<li> There might be possible factors that contribute in whether a loan is bad or not. </li>
<li> Factors that increase risk include: low annual income, high debt to income, high interest rates, low grade, among others. </li>
</ul>

<h4> Summary: </h4>
<ul>
<li> The types of bad loans in the last year are having a tendency to<b> decline</b>, except for late payments (might indicate an economical recovery.) </li>
<li> <b>Mortgage </b> was the variable from the home ownership column that used the highest amount borrowed within loans that were considered to be bad.</li>
<li> There is a slight <b>increase</b> on people who have mortgages that are applying for a loan.</li>
<li>People who have a mortgage (depending on other factors as well within the mortgage) are more likely to ask for <bhigher loan amounts than other people who have other types of home ownerships. </li>
</ul>







<div>


            <div id="6e4b1229-96b2-4ef4-af96-02f79c683799" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("6e4b1229-96b2-4ef4-af96-02f79c683799")) {
                    Plotly.newPlot(
                        '6e4b1229-96b2-4ef4-af96-02f79c683799',
                        {"title": {"text": "Types of Bad Loans <br> (Amount Borrowed Throughout the Years)"}, "xaxis": {"title": {"text": "Year"}}, "yaxis": {"title": {"text": "Amount Issued"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('6e4b1229-96b2-4ef4-af96-02f79c683799');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>





![png](output_42_0.png)


## Defaulted Loans and Level of Risk:
<a id="defaulted_loans"></a>
From all the bad loans the one we are most interested about are the loans that are defaulted. Therefore, in this section we will implement an in-depth analysis of these types of Loans and see if we can gain any insight as to which features have a high correlation with the loan being defaulted.

## Main Aim:
<ul>
<li> Determine patters that will allow us to understand somehow factors that contribute to a loan being <b>defaulted</b> </li>
</ul>

## Summary:
<ul>
<li>In the last year recorded, the <b>Midwest </b>  and <b> SouthEast </b> regions had the most defaults. </li>
<li>Loans that have a <b>high interest rate</b>(above 13.09%) are more likely to become a <b>bad loan </b>. </li>
<li>Loans that have a longer <b> maturity date (60 months) </b> are more likely to be a bad loan. </li>
</ul>





<div>


            <div id="b2770b69-29cd-4519-86cb-08f2f3e573fc" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("b2770b69-29cd-4519-86cb-08f2f3e573fc")) {
                    Plotly.newPlot(
                        'b2770b69-29cd-4519-86cb-08f2f3e573fc',
                        [{"fill": "tonexty", "hoverinfo": "x+text", "line": {"color": "rgb(131, 90, 241)", "width": 0.5}, "mode": "lines", "name": "NorthEast", "text": ["$35000", "$22000", "$6150", "$6000", "$8000", "$24000", "$8500"], "type": "scatter", "uid": "b4bdff25-ca26-455f-ab4f-df032ba92fc1", "x": [2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018], "y": [35000, 22000, 6150, 6000, 8000, 24000, 8500]}, {"fill": "tonexty", "hoverinfo": "x+text", "line": {"color": "rgb(255, 140, 0)", "width": 0.5}, "mode": "lines", "name": "SouthWest", "text": ["$30000", "$30000", "$6000", "$10400"], "type": "scatter", "uid": "3ba72402-5b93-4b7c-9ac8-0f8686e17e8b", "x": [2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018], "y": [65000, 52000, 12150, 16400]}, {"fill": "tonexty", "hoverinfo": "x+text", "line": {"color": "rgb(240, 128, 128)", "width": 0.5}, "mode": "lines", "name": "SouthEast", "text": ["$40000", "$24000", "$16000", "$2000", "$15000"], "type": "scatter", "uid": "d2615c09-bb52-4ef4-bfe9-384af4c8a357", "x": [2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018], "y": [105000, 76000, 28150, 18400]}, {"fill": "tonexty", "hoverinfo": "x+text", "line": {"color": "rgb(135, 206, 235)", "width": 0.5}, "mode": "lines", "name": "West", "text": ["$1000", "$12800", "$21000", "$18000", "$15625", "$18000"], "type": "scatter", "uid": "ed0ccc67-3fa4-4624-b0ed-ec6762d9fc52", "x": [2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018], "y": [106000, 88800, 49150, 36400]}, {"fill": "tonexty", "hoverinfo": "x+text", "line": {"color": "rgb(240, 230, 140)", "width": 0.5}, "mode": "lines", "name": "MidWest", "text": ["$7500", "$18000", "$16000", "$1500", "$5000", "$23000", "$7325", "$24000", "$18000"], "type": "scatter", "uid": "74ef60c8-eef5-4eb0-a7d4-d42a1827b0cd", "x": [2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018], "y": [113500, 106800, 65150, 37900]}],
                        {"title": {"text": "Amount Defaulted by Region"}, "xaxis": {"title": {"text": "Year"}}, "yaxis": {"title": {"text": "Amount Defaulted"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('b2770b69-29cd-4519-86cb-08f2f3e573fc');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>



```python
df['interest_rate'].describe()
```




    count    2.260668e+06
    mean     1.309291e+01
    std      4.832114e+00
    min      5.310000e+00
    25%      9.490000e+00
    50%      1.262000e+01
    75%      1.599000e+01
    max      3.099000e+01
    Name: interest_rate, dtype: float64




```python
# Average interest is 13.09% Anything above this will be considered of high risk let's see if this is true.
df['interest_payments'] = np.nan
lst = [df]

for col in lst:
    col.loc[col['interest_rate'] <= 13.09, 'interest_payments'] = 'Low'
    col.loc[col['interest_rate'] > 13.09, 'interest_payments'] = 'High'

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>loan_amount</th>
      <th>funded_amount</th>
      <th>investor_funds</th>
      <th>term</th>
      <th>interest_rate</th>
      <th>installment</th>
      <th>grade</th>
      <th>sub_grade</th>
      <th>emp_length</th>
      <th>home_ownership</th>
      <th>...</th>
      <th>settlement_percentage</th>
      <th>settlement_term</th>
      <th>year</th>
      <th>loan_condition</th>
      <th>region</th>
      <th>complete_date</th>
      <th>emp_length_int</th>
      <th>income_category</th>
      <th>loan_condition_int</th>
      <th>interest_payments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2500</td>
      <td>2500</td>
      <td>2500.0</td>
      <td>36 months</td>
      <td>13.56</td>
      <td>84.92</td>
      <td>C</td>
      <td>C1</td>
      <td>10+ years</td>
      <td>RENT</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2018</td>
      <td>Good Loan</td>
      <td>NorthEast</td>
      <td>2018-12-01</td>
      <td>10.0</td>
      <td>Low</td>
      <td>0</td>
      <td>High</td>
    </tr>
    <tr>
      <th>1</th>
      <td>30000</td>
      <td>30000</td>
      <td>30000.0</td>
      <td>60 months</td>
      <td>18.94</td>
      <td>777.23</td>
      <td>D</td>
      <td>D2</td>
      <td>10+ years</td>
      <td>MORTGAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2018</td>
      <td>Good Loan</td>
      <td>SouthEast</td>
      <td>2018-12-01</td>
      <td>10.0</td>
      <td>Low</td>
      <td>0</td>
      <td>High</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5000</td>
      <td>5000</td>
      <td>5000.0</td>
      <td>36 months</td>
      <td>17.97</td>
      <td>180.69</td>
      <td>D</td>
      <td>D1</td>
      <td>6 years</td>
      <td>MORTGAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2018</td>
      <td>Good Loan</td>
      <td>MidWest</td>
      <td>2018-12-01</td>
      <td>6.0</td>
      <td>Low</td>
      <td>0</td>
      <td>High</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4000</td>
      <td>4000</td>
      <td>4000.0</td>
      <td>36 months</td>
      <td>18.94</td>
      <td>146.51</td>
      <td>D</td>
      <td>D2</td>
      <td>10+ years</td>
      <td>MORTGAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2018</td>
      <td>Good Loan</td>
      <td>West</td>
      <td>2018-12-01</td>
      <td>10.0</td>
      <td>Low</td>
      <td>0</td>
      <td>High</td>
    </tr>
    <tr>
      <th>4</th>
      <td>30000</td>
      <td>30000</td>
      <td>30000.0</td>
      <td>60 months</td>
      <td>16.14</td>
      <td>731.78</td>
      <td>C</td>
      <td>C4</td>
      <td>10+ years</td>
      <td>MORTGAGE</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2018</td>
      <td>Good Loan</td>
      <td>NorthEast</td>
      <td>2018-12-01</td>
      <td>10.0</td>
      <td>Low</td>
      <td>0</td>
      <td>High</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 146 columns</p>
</div>




```python
df['term'].value_counts()
```




     36 months    1609754
     60 months     650914
    Name: term, dtype: int64





![png](output_48_1.png)


## Risk Assesment:
The main aim in this section is to compare the average interest rate for the loan status belonging to each type of loans (Good loan or bad loan) and see if there is any significant difference in the average of interest rate for each of the groups.

## Summary:
<ul>
<li> <b> Bad Loans: </b>  Most of the loan statuses belonging to this group pay a interest ranging from 15% - 16%. </li>
<li><b>Good Loans:</b> Most of the loan statuses belonging to this group pay interest ranging from 12% - 13%.  </li>
<li>There has to be a better assesment of risk since there is not that much of a difference in interest payments from <b>Good Loans</b> and <b>Bad Loans</b>. </li>
<li> Remember, most loan statuses are <b>Current</b> so there is a risk that at the end of maturity some of these loans might become bad loans. </li>
</ul>

<br>





<div>


            <div id="a9966310-a95f-446d-a250-ea6cecb93a6f" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("a9966310-a95f-446d-a250-ea6cecb93a6f")) {
                    Plotly.newPlot(
                        'a9966310-a95f-446d-a250-ea6cecb93a6f',
                        [{"fill": "toself", "line": {"color": "#63AF63"}, "marker": {"color": "#B3FFB3", "size": 8, "symbol": "square"}, "mode": "lines+markers", "name": "Good Loans", "r": [12.64, 12.76, null, 13.98], "subplot": "polar", "theta": ["Fully Paid", "Current", "Issued", "No C.P. Fully Paid"], "type": "scatterpolar", "uid": "43b3a774-b4f3-451f-9590-c55346c64b06"}, {"fill": "toself", "line": {"color": "#C31414"}, "marker": {"color": "#FF5050", "size": 8, "symbol": "square"}, "mode": "lines+markers", "name": "Bad Loans", "r": [16.73, 15.71, 14.6, 15.23, 15.49, 15.71], "subplot": "polar2", "theta": ["Default Rate", "Charged Off", "C.P. Charged Off", "In Grace Period", "Late (16-30 days)", "Late (31-120 days)"], "type": "scatterpolar", "uid": "b3efb6d6-f24c-4ca4-84c0-0f2202389443"}],
                        {"paper_bgcolor": "rgb(255, 248, 243)", "polar": {"angularaxis": {"direction": "counterclockwise", "rotation": 90, "tickfont": {"size": 8}}, "domain": {"x": [0, 0.4], "y": [0, 1]}, "radialaxis": {"tickfont": {"size": 8}}}, "polar2": {"angularaxis": {"direction": "clockwise", "rotation": 90, "tickfont": {"size": 8}}, "domain": {"x": [0.6, 1], "y": [0, 1]}, "radialaxis": {"tickfont": {"size": 8}}}, "showlegend": false, "title": {"text": "Average Interest Rates <br> Loan Status Distribution"}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('a9966310-a95f-446d-a250-ea6cecb93a6f');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>


## Condition of Loans and Purpose:
<a id="loan_condition"></a>
In this section we will go into depth regarding the <b>reasons for clients to apply for a loan. </b> Our main aim is to see if there are purposes that contribute to a <b> "higher" </b> risk whether the loan will be repaid or not.

### Summary:
<ul>
<li> <b>Bad Loans Count: </b> People that apply for educational and small business purposed tend to have a higher risk of being a bad loan. (% wise) </li>
<li><b>Most frequent Purpose: </b> The reason that clients applied the most for a loan was to consolidate debt. </li>
<li><b>Less frequent purpose:</b> Clients applied less for educational purposes for all three income categories.  </li>
<li><b>Interest Rates: </b> In all reasons for application except (medical, small business and credi card), the low income category has a higher interest rate. Something that could possibly explain this is the amount of capital that is needed from other income categories that might explain why the low income categories interest rate for these puposes are lower.  </li>
<li><b>Bad/Good Ratio:</b> Except for educational purposes (we see a spike in high income this is due to the reasons that only two loans were issued and one was a bad loan which caused this ratio to spike to 50%.), but we can see that in all other purposed the bad good ratio is lower the higher your income category.  </li>

</ul>




<div>


            <div id="a19eab95-b097-47fe-8cd3-49c4db70e95e" class="plotly-graph-div" style="height:525px; width:100%;"></div>
            <script type="text/javascript">
                require(["plotly"], function(Plotly) {
                    window.PLOTLYENV=window.PLOTLYENV || {};
                    window.PLOTLYENV.BASE_URL='https://plot.ly';

                if (document.getElementById("a19eab95-b097-47fe-8cd3-49c4db70e95e")) {
                    Plotly.newPlot(
                        'a19eab95-b097-47fe-8cd3-49c4db70e95e',
                        [{"marker": {"color": "rgba(219, 64, 82, 0.7)", "line": {"color": "rgba(219, 64, 82, 1.0)", "width": 2}}, "name": "Bad Loans", "text": "%", "type": "bar", "uid": "2583daaa-35fb-4ecf-bf30-18d4abd5105b", "x": ["car", "credit_card", "debt_consolidation", "educational", "home_improvement", "house", "major_purchase", "medical", "moving", "other", "renewable_energy", "small_business", "vacation", "wedding"], "y": [10.02, 10.68, 14.2, 20.75, 11.61, 12.94, 12.27, 13.85, 15.91, 13.28, 16.4, 20.52, 12.44, 12.4]}, {"marker": {"color": "rgba(50, 171, 96, 0.7)", "line": {"color": "rgba(50, 171, 96, 1.0)", "width": 2}}, "name": "Good Loans", "text": "%", "type": "bar", "uid": "87aa6ecb-63b0-4ec7-9160-217c3f17876c", "x": ["car", "credit_card", "debt_consolidation", "educational", "home_improvement", "house", "major_purchase", "medical", "moving", "other", "renewable_energy", "small_business", "vacation", "wedding"], "y": [89.98, 89.32, 85.8, 79.25, 88.39, 87.06, 87.73, 86.15, 84.09, 86.72, 83.6, 79.48, 87.56, 87.6]}],
                        {"paper_bgcolor": "#FFF8DC", "plot_bgcolor": "#FFF8DC", "showlegend": true, "title": {"text": "Condition of Loan by Purpose"}, "xaxis": {"title": {"text": ""}}, "yaxis": {"title": {"text": "% of the Loan"}}},
                        {"showLink": false, "linkText": "Export to plot.ly", "plotlyServerURL": "https://plot.ly", "responsive": true}
                    ).then(function(){

var gd = document.getElementById('a19eab95-b097-47fe-8cd3-49c4db70e95e');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })
                };
                });
            </script>
        </div>






<style  type="text/css" >
    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col2 {
            background-color:  #4b64d5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col3 {
            background-color:  #edd2c3;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow0_col7 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col2 {
            background-color:  #94b6ff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col3 {
            background-color:  #ec7f63;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow1_col7 {
            background-color:  #445acc;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col2 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col3 {
            background-color:  #c73635;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col4 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col5 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col6 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow2_col7 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col2 {
            background-color:  #9dbdff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col3 {
            background-color:  #c53334;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col4 {
            background-color:  #4257c9;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col5 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col6 {
            background-color:  #4257c9;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow3_col7 {
            background-color:  #4b64d5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col2 {
            background-color:  #7b9ff9;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col3 {
            background-color:  #d95847;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col4 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col5 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col6 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow4_col7 {
            background-color:  #4a63d3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col2 {
            background-color:  #edd1c2;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col3 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow5_col7 {
            background-color:  #485fd1;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col2 {
            background-color:  #f7b79b;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col3 {
            background-color:  #ee8669;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow6_col7 {
            background-color:  #465ecf;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col2 {
            background-color:  #7699f6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col3 {
            background-color:  #88abfd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow7_col7 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col2 {
            background-color:  #efcebd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col3 {
            background-color:  #f7b99e;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow8_col7 {
            background-color:  #445acc;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col2 {
            background-color:  #dedcdb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col3 {
            background-color:  #f39778;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow9_col7 {
            background-color:  #4e68d8;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col2 {
            background-color:  #f7ad90;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col3 {
            background-color:  #cb3e38;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow10_col7 {
            background-color:  #7ea1fa;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col2 {
            background-color:  #a9c6fd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col3 {
            background-color:  #b2ccfb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow11_col7 {
            background-color:  #5673e0;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col2 {
            background-color:  #f29274;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col3 {
            background-color:  #f7b497;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow12_col7 {
            background-color:  #5a78e4;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col2 {
            background-color:  #cbd8ee;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col3 {
            background-color:  #efcebd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow13_col7 {
            background-color:  #536edd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col2 {
            background-color:  #e8765c;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col3 {
            background-color:  #b3cdfb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col4 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col5 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col6 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow14_col7 {
            background-color:  #7093f3;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col2 {
            background-color:  #b7cff9;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col3 {
            background-color:  #6282ea;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col4 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col5 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col6 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow15_col7 {
            background-color:  #5673e0;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col2 {
            background-color:  #96b7ff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col3 {
            background-color:  #b1cbfc;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col4 {
            background-color:  #c5d6f2;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col5 {
            background-color:  #9fbfff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col6 {
            background-color:  #c0d4f5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow16_col7 {
            background-color:  #5d7ce6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col2 {
            background-color:  #f5c1a9;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col3 {
            background-color:  #bed2f6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col4 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col5 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col6 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow17_col7 {
            background-color:  #799cf8;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col2 {
            background-color:  #aac7fd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col3 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow18_col7 {
            background-color:  #a9c6fd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col2 {
            background-color:  #d5dbe5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col3 {
            background-color:  #a1c0ff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col4 {
            background-color:  #5b7ae5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col5 {
            background-color:  #5470de;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col6 {
            background-color:  #5a78e4;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow19_col7 {
            background-color:  #6384eb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col2 {
            background-color:  #dadce0;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col3 {
            background-color:  #8badfd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col4 {
            background-color:  #465ecf;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col5 {
            background-color:  #445acc;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col6 {
            background-color:  #455cce;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow20_col7 {
            background-color:  #6788ee;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col2 {
            background-color:  #f6bfa6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col3 {
            background-color:  #6180e9;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col4 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col5 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col6 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow21_col7 {
            background-color:  #7597f6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col2 {
            background-color:  #f29072;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col3 {
            background-color:  #6c8ff1;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col4 {
            background-color:  #5e7de7;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col5 {
            background-color:  #5a78e4;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col6 {
            background-color:  #5d7ce6;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow22_col7 {
            background-color:  #6f92f3;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col2 {
            background-color:  #e0654f;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col3 {
            background-color:  #6e90f2;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow23_col7 {
            background-color:  #88abfd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col2 {
            background-color:  #b40426;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col3 {
            background-color:  #bad0f8;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col4 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col5 {
            background-color:  #4257c9;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col6 {
            background-color:  #3f53c6;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow24_col7 {
            background-color:  #aec9fc;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col2 {
            background-color:  #f2c9b4;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col3 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col4 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col5 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col6 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow25_col7 {
            background-color:  #6788ee;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col2 {
            background-color:  #f7aa8c;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col3 {
            background-color:  #7396f5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow26_col7 {
            background-color:  #6485ec;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col2 {
            background-color:  #de614d;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col3 {
            background-color:  #4f69d9;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col4 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col5 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col6 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow27_col7 {
            background-color:  #88abfd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col2 {
            background-color:  #5f7fe8;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col3 {
            background-color:  #abc8fd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow28_col7 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col2 {
            background-color:  #5977e3;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col3 {
            background-color:  #f6a385;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col4 {
            background-color:  #5875e1;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col5 {
            background-color:  #485fd1;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col6 {
            background-color:  #5572df;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow29_col7 {
            background-color:  #4358cb;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col2 {
            background-color:  #cbd8ee;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col3 {
            background-color:  #f39778;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col4 {
            background-color:  #81a4fb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col5 {
            background-color:  #6b8df0;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col6 {
            background-color:  #7ea1fa;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow30_col7 {
            background-color:  #5a78e4;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col2 {
            background-color:  #cedaeb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col3 {
            background-color:  #688aef;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow31_col7 {
            background-color:  #516ddb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col2 {
            background-color:  #a1c0ff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col3 {
            background-color:  #f2cbb7;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col4 {
            background-color:  #455cce;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col5 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col6 {
            background-color:  #445acc;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow32_col7 {
            background-color:  #516ddb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col2 {
            background-color:  #88abfd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col3 {
            background-color:  #97b8ff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow33_col7 {
            background-color:  #8db0fe;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col2 {
            background-color:  #f5c2aa;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col3 {
            background-color:  #a7c5fe;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow34_col7 {
            background-color:  #5977e3;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col2 {
            background-color:  #dddcdc;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col3 {
            background-color:  #adc9fd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col4 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col6 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow35_col7 {
            background-color:  #5b7ae5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col2 {
            background-color:  #ecd3c5;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col3 {
            background-color:  #f7af91;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow36_col7 {
            background-color:  #506bda;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col2 {
            background-color:  #e9d5cb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col3 {
            background-color:  #cfdaea;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col4 {
            background-color:  #4055c8;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col5 {
            background-color:  #3e51c5;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col6 {
            background-color:  #3f53c6;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow37_col7 {
            background-color:  #5673e0;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col2 {
            background-color:  #f7ba9f;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col3 {
            background-color:  #dcdddd;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col4 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col5 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col6 {
            background-color:  #3b4cc0;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow38_col7 {
            background-color:  #799cf8;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col2 {
            background-color:  #ee8468;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col3 {
            background-color:  #f7aa8c;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col4 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col5 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col6 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow39_col7 {
            background-color:  #92b4fe;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col2 {
            background-color:  #9bbcff;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col3 {
            background-color:  #e9d5cb;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col4 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col5 {
            background-color:  #3c4ec2;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col6 {
            background-color:  #3d50c3;
            color:  #f1f1f1;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow40_col7 {
            background-color:  #5875e1;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow41_col2 {
            background-color:  #ec8165;
            color:  #000000;
        }    #T_e503294a_77a0_11e9_8c25_086d41c3011erow41_col3 {
            </tr>
