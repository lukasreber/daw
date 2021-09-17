# Pandas Cheatsheet

First import

    import pandas as pd
    import numpy as np
    import matplotlib as plt

    plt.style.use('seaborn')

https://towardsdatascience.com/all-the-pandas-read-csv-you-should-know-to-speed-up-your-data-analysis-1e16fe1039f3

Encoding prüfen

    def get_filre_encoding(src_file_path):
    with open(src_file_path) as src_file:
        return src_file.encoding

    csv_file_path = './Exam_HS20/DAW_Exam_Material/diabetes.xlsx'
    print('Endcoding: ' + str(get_file_encoding(csv_file_path)))

CSV einlesen

    pd.read_csv('pathtocsv', encoding='', header=None, parse_dates=['datevar'])

Dataframe overview

    df.head()
    df.describe()
    df.info()
    df.shape

Pandas Datentyp anzeigen

    df.dtypes

Dataframe Column Typ ändern

    df['col] = df['col'].astype('....')

Mögliche Typen:

- object
- int64
- float64
- bool
- datetime64
- timedelta[ns]
- category

Ordered Categories definieren

    df['col'] = df['col'].cat.set_categories(['Category1','Category2'], ordered=True)

Gruppieren und prozentualer Anteil ausgeben

    df.groupby('var').agg({'other_var': 'sum'})[['other_var']].apply(lambda x: 100*x /float(x.sum()))

Columns renamen

    df.rename(columns={'oldname':'newname'})

Spalten zu Zeilen umformen

    df.melt('NameOfIndexColumn')

Zeilen zu spalten?

    pd.pivot_table

Json umformen

    pd.json_normalize

## Fehlende Werte

Nan finden mit

    np.isnull().sum()
    np.isna().any()

oder eine ähnliche Kombination...
Eine gute Idee ist auch den Hist plot auszugeben

    df.hist(figsize=(10,10))

Falsche Werte durch NaN ersetzen

    df[col] = df[col].replace(0,np.nan)

Daten binning

    pd.cut(x=df[col], bins=[list], labels=[list])

Zeilen mit fehlenden Werten löschen

    df.dropna()

Fehlende Werte ersetzen

    df.variable = df.variable.fillna(df.variable.median())

Kategorische Werte definieren

    df.variable = pd.Categorical(df.variable, categories=['Value1','Value2'], ordered=True)

Kategorische Werte definieren - alternativ:

    df.variable = df.variable.astype('category')
    df.variable = df.variable.cat.set_categories(["Value1","Value2"], ordered=True)

Neuer Wert berechnen aus bestehenden Werten

    df.variable = df.var1 * / + df.var2...

## Dataframe umformen

Von Spalten zu Zeilen (Variablen welche als Zeilen bestehen bleiben sollen als Liste)

    df = df.melt(id_vars=['variable1','variableN'], var_name="var", value_name="var") 

Variable splitten

    df[['var1','var2']] = df.variable.str.split('_', expand=True)

Duplikate entfernen

    df.drop_duplicates(subset=['var1', 'var'], keep='first')

## Moving Averages (https://towardsdatascience.com/moving-averages-in-python-16170e20f6c)

Simple moving average

    df['new_avg_val'] = df.variable.rolling(INT_WINDOW, min_periods=1).mean()

Cumulative moving average

    df['new_avg_val'] = df.variable.expanding(INT_WINDOW, min_periods=1).mean()

Iterating rows

    for index,row in df.iterrows():
        print(json_normalize(row['values']))

drop column

    df.drop(['values'],axis=1)

# Datetime

daterange erstellen

    pd.date_range(start='', end='')

datum formatieren

    var.strftime('%d-%m-%Y')

Dataframes kombinieren:

    df = pd.DataFrame([])
    for entry in list:
        df_temp = pd.read_csv(entry)
        df = df.append(df_temp)

Ordnen order sort

    df.sort_values(by=['val1','val2'])

Differenz zu vorheriger Spalte

    df['variable'].diff()

Pandas apply

    def bmi_class (row):
        if row['bmi'] < 18.5 :
            return 'underweight'
        if row['bmi'] < 25 :
            return 'normal'
        if row['bmi'] < 30 :
            return 'overweight'
        if row['bmi'] >= 30 :
            return 'obese'
        return 'no bmi'

    df['bmi_class'] = df.apply(lambda row: bmi_class(row), axis=1)

Join, merge Dataframes

    pd.merge(df1, df2, on = 'variable', how = 'inner/outer')

Werte kleiner als 0 auf Null setzen

    df[df < 0] = 0
