# COVID-19-TRACKER
import requests
import pandas as pd
import matplotlib.pyplot as plt
import plotly.express as px

# Fetch global summary
global_response = requests.get("https://disease.sh/v3/covid-19/all").json()
print("Global COVID-19 Summary:")
global_response
# Fetch per-country COVID data
countries_data = requests.get("https://disease.sh/v3/covid-19/countries").json()

# Convert to DataFrame
df = pd.json_normalize(countries_data)
df = df[['country', 'cases', 'deaths', 'recovered', 'active', 'population']]
df.sort_values('cases', ascending=False, inplace=True)
df.head(10)
top10 = df.head(10)

plt.figure(figsize=(12,6))
plt.bar(top10['country'], top10['cases'], color='tomato')
plt.title("Top 10 Countries by COVID-19 Cases")
plt.xlabel("Country")
plt.ylabel("Cases")
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
fig = px.choropleth(df,
                    locations="country",
                    locationmode="country names",
                    color="cases",
                    hover_name="country",
                    color_continuous_scale="reds",
                    title=" Global COVID-19 Case Distribution")
fig.show()
def get_country_stats(name):
    row = df[df['country'].str.lower() == name.lower()]
    return row if not row.empty else f"No data found for {name}"

get_country_stats("Kenya")




