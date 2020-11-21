# -Most-expensive-imports-from-Mexico-in-2019
I would like to know the 10 most expensive imports that Mexico made during 2019, registered by the World Trade Organization, taking into account the type of product or services, in addition to the countries from which it obtained them.

During 2019, there were interesting phenomenon in the international economy, from the rise in price of some important currencies in the world, to the beginning of one of the most deadly diseases that the world could experience. This disease resulted in a pandemic that caused a drop in international trade, for which Mexico had to resort to importing many products that were made in the national territory for reasons of suspension of activities due to health risk.

# -Explanation.
I decided to use the information from the World Trade Organization, which has a very interesting API, as well as accessible, which would allow me to obtain important, truthful and truthful information about the transactions carried out.

After looking for which area of trade information I would like to use, I began to search through everything that the WTO API offers, until I found a dataset on the most valuable or most expensive exports reported annually, where I chose the period 2019 -2020.

I used the CSV file WtoData_20201121045833, which was the one I downloaded from the WTO API with the data on exports of products and services for 2019 registered by the WTO. 
I used data = pd.read_csv("WtoData_20201121045833.csv", encoding='latin-1') to give a first look.

I looked for the amount of null data (null_cols = data.isnull().sum()), and then eliminated the columns that contained it (drop_cols = list(null_cols[null_cols > 0].index))(data = data.drop(drop_cols, axis=1)).

After that, I began to review the countries that are named as Reporting Economies and Associated Economies, in addition to the products for sector:
data['Economía declarante'].value_counts()
data['Economía asociada'].value_counts()
data['Producto/Sector'].value_counts()

I looked for the outliers to take them into account and remove the last columns that I would not need to get my final dataframe:
outliers = pd.DataFrame(columns=data.columns)

for col in stats.index:
    iqr = stats.at[col,'IQR']
    cutoff = iqr * 1.5
    lower = stats.at[col,'25%'] - cutoff
    upper = stats.at[col,'75%'] + cutoff
    results = data[(data[col] < lower) |
                   (data[col] > upper)].copy()
    results['Outlier'] = col
    outliers = outliers.append(results)

outliers

With the dataset defined, I continued cleaning the data until I got the 10 most expensive imports made by Mexico, and from which countries the products or services come:
filtered = data[(data['Economía asociada']=='México') &
                (data['Producto/Sector'] )]

filtered.head


I ordered everything by the Valor column:
filtered_ordenado_por_valor = filtered.sort_values('Valor',ascending=False)

And finally, find the 10 most expensive imports made by Mexico:
filtered_ordenado_por_valor[:10]

# -Results.

I obtained the information I was looking for with relative ease, since the WTO has very defined categories and filters with which you can obtain everything you need.The incredible surprise is the type of product or service that was imported and from which countries, highlighting China as one of the main commercial partners.

# -Obstacles-encountered.

I knew it would be a bit complicated for me because of the type of information I decided to look for so I got some different datasets with which to work. And how I planned it, I had several obstacles along the way, such as the fact that I could not work with some datasets because I could not read them, or those that gave me information, did not allow me to perform some functions because data did not allow it.

# -Lessons-learned.

The lessons learned during the development of the project were various. First of all, to realize that the research carried out is always deeper than one can imagine. Second, I didn't think it would take me so much trouble to manipulate the information until I got what I was looking for. Third, it is amazing what can be achieved with the little we have learned thus far.

And something very important, the support of colleagues and TAs always allows us to get ahead of the problems that may arise during the development of any exercise.
