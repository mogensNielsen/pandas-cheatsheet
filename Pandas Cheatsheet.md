# Pandas Cheatsheet

- [Pandas Cheatsheet](#pandas-cheatsheet)
  - [Filoperasjoner](#filoperasjoner)
    - [Lese fra CSV](#lese-fra-csv)
    - [Skrive til CSV](#skrive-til-csv)
  - [Metadata on dataframe](#metadata-on-dataframe)
    - [DataFrame.info()](#dataframeinfo)
    - [DataFrame.describe()](#dataframedescribe)
    - [Antall rader/elementer i dataframe](#antall-raderelementer-i-dataframe)
  - [Filtrere data](#filtrere-data)
    - [Enkel filtrering](#enkel-filtrering)
    - [DataFrame.loc[]](#dataframeloc)
    - [Dataframe.query()](#dataframequery)
  - [Endre options](#endre-options)
    - [Vise alle rader i en dataframe](#vise-alle-rader-i-en-dataframe)
  - [Rense data](#rense-data)
    - [Finne duplikater i dataframe](#finne-duplikater-i-dataframe)
    - [Fjerne kolonner](#fjerne-kolonner)
    - [Fjerne rader](#fjerne-rader)
  - [Merge data](#merge-data)
  - [Gruppere data](#gruppere-data)
  - [Dataframe.apply()](#dataframeapply)
  - [Utforske data](#utforske-data)
    - [Fordeling](#fordeling)
  - [Python substring](#python-substring)
  - [Python Lambda](#python-lambda)
    - [Introduksjon](#introduksjon)
    - [Eksempel på bruk i pandas](#eksempel-på-bruk-i-pandas)

## Filoperasjoner

### Lese fra CSV

```python
df_dataframe = pd.read_csv(
  input_file_path,
  sep = ';', # Separator
  encoding = 'UTF-8', # Character set encoding
  decimal = '.', # Decimal character
  dtype = dict_data_types # Dictionary with datatype pr column
)
```

### Skrive til CSV

```python
df_dataframe.to_csv(
  output_file_path,
  sep = ';',
  index = False # Ikke skrive index-kolonnen til fil,
  encoding = 'UTF-8'
)
```

## Metadata on dataframe

### DataFrame.info()

Lister ut kolonnenavn med:

- Antall rader som er ikke-NULL
- Datatype

```python
df_dataframe.info()
```

### DataFrame.describe()

Lister ut kolonner med statistikk:

- For numeriske kolonner:
  - count
  - mean
  - standard deviation
  - min
  - 25%
  - 50% (samme som median)
  - 75%
  - max
- For object data (f.eks. strenger, timestamps):
  - count
  - unique (antall unike forekomster i kolonnen)
  - top (mest brukte verdi i kolonnen)
  - freq (antall av top)

```python
df_dataframe.describe()
```

### Antall rader/elementer i dataframe

Antall rader

```python
len(df_dataframe)
```

Antall rader og kolonner

```python
df_dataframe.shape
```

Antall elements, dvs. celler, i dataframe

```python
df_dataframe.size
```

## Filtrere data

### Enkel filtrering

```python
df_dataframe[
  # Filtrering med AND. For OR bruk '|' (pipe symbol)
  (df_dataframe[column1 == 'value']) & (df_dataframe[column2 == 'value'])
]
```

### DataFrame.loc[]

Filtrere og spesifisere hvilke kolonner som skal vises i resultatet

```python
df_dataframe.loc[
  df_dataframe.column.isna(), # Filter
  #df_dataframe.column == [X] # Alternativt filter
  ['col1', 'col2'] # Liste med kolonner som skal vises i resultatet
]
```

### Dataframe.query()

Ganske SQL-lik spørring

Spørringen må "pakkes inn i" '. Kolonnenavn er ikke "pakket inn". Strengverdier er pakket inn i ".

```python
df_dataframe.query('col1 == "abc" & col2 == "cde"')
```

## Endre options

### Vise alle rader i en dataframe

Standard er å vise 10 rader

```python
# Endre 10 --> None gir ingen begrensning
pd.set_option('display.max_rows', 10)
```

## Rense data

### Finne duplikater i dataframe

```python
df_dataframe[
  df_dataframe.duplicated(
    ['WMI', 'WMI_PART_TWO'], # Kolonner som skal sjekkes
    keep=False # Markerer alle duplikater som True. Alt. 'last' eller 'first'
  ) == True
]
```

### Fjerne kolonner

Fjerne kolonner (eller rader med axis=0) i dataframe

```python
df_heavy_manufacturers.drop(
    # Alle kolonner bortsett fra disse
    df_heavy_manufacturers.columns.difference(['VIN', 'X-12_14', 'Mh']),
    # Axis = 1 --> kolonner
    axis=1,
    # Ikke lag ny dataframe, gjør operasjonen på den eksisterende
    inplace=True
)
```

### Fjerne rader

Fjerne rader ved å spesifisere index-verdi.

Kan også bruke inplace=True for å slette rett i dataframen i stedet for å returnere en ny en. Bruk av inplace==True gir en advarsel, men operasjonen gjennomføres

```python
df_dataframe_deleted = df_dataframe.drop(
    [174,
    12477]
)
```

## Merge data

```python
df_merged = df_dataframe_1.merge(
  df_dataframe_2,
  how = 'outer',    # Outer, inner osv.
  on = 'VIN',       # Kolonne som skal sammenlignes
  suffixes = ('_original', '_korrigert'), # Kolonner med like navn postfikses
  indicator = True  # Legger på en kolonne "_merge" som viser hvor data kommer fra; left_only eller both i dette eksempelet
)
```

## Gruppere data

Grupperer på en kolonne og aggregerer

```python
df_dataframe.groupby(['kolonne']).count()
```

Denne funker også

```python
groupby(['Ct']).size()
```

## Dataframe.apply()

Kjører en funksjon langs en akse på en dataframe. Axis 0 er kolonner og axis 1 er rader.

F.eks:

```python
# Henter maks-verdi for hver kolonne
dataframe.apply(np.max)
```

## Utforske data

### Fordeling

value_counts gir antall pr kategori

```python
dataframe['kolonne'].value_counts()
```

Med normalize for man andel av totalen

```python
dataframe['kolonne'].value_counts(normalize = True)
```

## Python substring

I Python slicer man strenger (strenger er egentlig arrays med tegn).

```python
# start er starten på substringen, default er 0
# end er *ikke* inkludert. Default er strengens lengde
# step hopp over hvert [step] tegn. F.eks. hopp over annenhvert tegn ```streng = 'python' --> streng[::2] --> 'pto'```Default er 1
streng[start: end: step]
```

## Python Lambda

### Introduksjon

Syntax:

```python
lambda arguments : expression
```

F.eks:

```python
# x blir en funksjon som tar ett argument, a, og returnerer a addert med 10
x = lambda a : a + 10
```

### Eksempel på bruk i pandas

```python
# Denne gir ut det første tegnet i strengen som sendes til lambda-funksjonen
lambda sakstype : sakstype[0]
```

Lambda kan f.eks. brukes sammen med ```df.apply()``` for å kjøre en lambda-funksjon på alle kolonner

```python
df[ # Filtrerer dataframe
  df['kolonne'].apply(lambda verdi : verdi[0] == 'M') # Der hvor første tegn i strengen er lik 'M'
]
```

```python
dict_ting = {
  'eple': {
    'antall': 3,
  },
  'banan': 5
}

df_ting = pd.DataFrame(data=dict_ting)

# Transponerer dataframe fordi antall ligger i rader og navn i kolonner
df_ting = df_ting.transpose()

# Setter inn ny kolonne som er formel basert på eksisterende kolonner
df_ting.insert(
  1, # Kolonneposisjon, første kolonne har posisjon 0
  'Dobbelt antall', # Navn på ny kolonne
  df_ting.apply( # Kolonnens verdier
    # Tar i mot antallkolonnen og returnerer antallkolonnen mulitplisert med to
    lambda antall : df_ting['antall'] * 2)
  )
```
