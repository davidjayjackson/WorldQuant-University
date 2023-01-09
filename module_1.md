## Analysis of Mexico Housing Prices
#### Commands Used:
```
#### Library that need to be loaded
import pandas as pd
import matplotlib.pyplot as plt
import plotly.express as px

* df.from_csv() (create a data frame from csv file
* df.to_csv()   ( write data data to csv file)
* df.concat()   combine dart frame into single data frames (axis = 0 is for columns
df.str.split()
df.replace()
* df.to_csv("data/clean-mexico-real-estate.csv",index = False)
* df3.dropna(inplace = True)
* df = pd.concat([df1,df3,df3])  combine dart frame into single data frames (axis = 0 is for columns
* df1['price_usd'] = df1['price_usd'].str.replace("$","",regex=False).str.replace(",","",regex=False)
* df2['price_usd'] = (df2['price_mxn']/19).round(2)
* df2.drop(columns=['price_mxn'],inplace = True)
* df3[["lat","lon"]] = df3["lat-lon"].str.split(",",expand = True)

####  k1.1.2 Nested List and dictonary
houses_nested_list = [
    [115910.26, 128.0, 4.0],
    [48718.17, 210.0, 3.0],
    [28977.56, 58.0, 2.0],
    [36932.27, 79.0, 3.0],
    [83903.51, 111.0, 3.0],
]

for house in houses_nested_list:
    price_m2 = house[0]/house[1]
    house.append(price_m2)
houses_nested_list

#### 1.1.5 1.1.6: To calculate the mean price for houses_rowwise 

house_prices = []
for house in houses_rowwise:
    house_prices.append(house["price_aprox_usd"])


mean_house_price = sum(house_prices) / len(house_prices)
mean_house_price

for house in houses_rowwise:
    house["price_per_m2"]= house["price_aprox_usd"] / house["surface_covered_in_m2"]
    
#### 1.1.7: Calculate the mean house price in houses_columnwise
price = houses_columnwise["price_aprox_usd"]
area = houses_columnwise["surface_covered_in_m2"]

price_per_2m = []
for p,a in zip(price,area):
    price_2m = p/a
    price_per_2m.append(price_2m)
    
houses_columnwise["price_2m"] = price_per_2m
houses_columnwise

---------------------------------------------------------------------------
### Answers: https://chat.openai.com/

# Replace all the letters in the 'column' with a space
df['column'] = df['column'].replace(r'[a-zA-Z]', ' ', regex=True)

-----------------------------------------------------------------------------
#### Sort values column in bar plot:https://chat.openai.com/
houses['property_type'].value_counts().sort_values(ascending=True).plot(kind = 'barh')
plt.xlabel("Property Type")
plt.ylabel("Frequency")
plt.title("Count by Property Type");
```
#### Plot houses location
```
fig = px.scatter_mapbox(
    df,  # Our DataFrame
    lat=...,
    lon=...,
    center={"lat": 19.43, "lon": -99.13},  # Map will be centered on Mexico City
    width=600,  # Width of map
    height=600,  # Height of map
    hover_data=["price_usd"],  # Display price when hovering mouse over house
)

fig.update_layout(mapbox_style="open-street-map")

fig.show()
```
### 1.3.3 Categorical Variables: State
```
df['state'].nunique() # returns count
df['state'].unique() # returns unique list of states
df['state'].value_counts().head()

### 1.3.4 Numerical Data: "area_m2" and "price_usd"
df[['area_m2','price_usd']].describe()

### 1.3.5: Histograms and boxplots (area_m2)
#### 1.3.6
plt.hist(x='area_m2',data = df)
plt.xlabel("Area [sq meters]")
plt.ylabel("Frequency")
plt.title("Distribution of Home Sizes");

plt.boxplot(x='area_m2',data = df,vert = False)
plt.xlabel("Area [sq meters]")
plt.title("Distribution of Home Sizes")

### 1.3.7  Hisogram and boxplot (price_usd and price_m2)

plt.hist(x='price_usd',data = df)
plt.xlabel("Price [USD]")
plt.ylabel("Frequency")
plt.title("Distribution of Home Prices");

### 1.3.8
plt.boxplot(x='price_usd',data = df,vert = False)
plt.xlabel("Price [USD]")
plt.title("Distribution of Home Prices")

### 1.4.0 Location or Size: What Influences House Prices in Mexico?
#### 1.4.2 Calculate mean price by state
mean_price_by_state = df.groupby("state")["price_usd"].mean().sort_values(ascending = False)

#### 1.4.3 Bar plot by mean price by state
mean_price_by_state.plot(kind='barh',
            xlabel = "State",
            ylabel = "Mean Price [USD]",
            title = "Mean House Price by State");
            
#### 1.4.4 Price per square meter.
df["price_per_m2"] = df["price_usd"]/ df["area_m2"]

#### 1.4.5 Mean Price per M^2[USD]
(
    df.groupby("state")
    ["price_per_m2"].mean()
    .sort_values(ascending = False)
    .plot(
        kind = 'barh',
        ylabel = "State",
        xlabel = "Mean Price per M^2[USD]",
        title = "Mean House Price per M^2 by State"
    )
    );
	
#### 1.4.6  Is there a relationship between home size and price?
  
plt.scatter(x="area_m2",y="price_usd",data = df)
plt.xlabel("Area [sq meters]")
plt.ylabel("Price (USD]")
plt.title("Price vs Area")

plt.scatter(x="area_m2",y="price_per_m2",data = df)
plt.xlabel("Area [sq meters]")
plt.ylabel("Price per meter sq (USD]")
plt.title("Price per meter  vs Area")

#### 1.4.7 Using the corr method, calculate the Pearson correlation coefficient
for "area_m2" and "price_usd"

p_correlation = df["area_m2"].corr(df["price_usd"])
print(p_correlation)

p_correlation = df["area_m2"].corr(df["price_per_m2"])
print(p_correlation)

#### 1.4.8  State of Morelos

df_morelos = df[df["state"] =="Morelos" ]

#### 1.4.9 Morelos correlation

plt.scatter(x="area_m2",y="price_usd",data = df_morelos)
plt.xlabel("Area [sq meters]")
plt.ylabel("Price per meter sq (USD]")
plt.title("Morelos: Price vs. Area")

#### 1.5.0 Assignment Task 1.5.19
Create a dictionary south_states_corr, where the keys are the names of the three states in the "South" region of Brazil, and their associated values are the correlation coefficient between "area_m2" and "price_usd" in that state.

* Select only rows where "region" is "South"
df = df[df["region"] == "South"]

* Group data by "state" and calculate the correlation coefficient between "area_m2" and "price_usd" for each state
south_states_corr = df.groupby("state")[["area_m2", "price_usd"]].corr().iloc[0::2, -1]

* Convert Series to dictionary
south_states_corr = south_states_corr.to_dict()

* Print dictionary
print(south_states_corr)



```
