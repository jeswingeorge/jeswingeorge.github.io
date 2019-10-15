# Wrangling Open Street Map Data

## Map Area

The area selected for this project is New Delhi, India as i am familiar with the city and interested to work on this project using OSM data for this city.

* https://www.openstreetmap.org/node/16173236
* https://mapzen.com/data/metro-extracts/metro/new-delhi_india/

After downloading the compressed file of OSM data from the above mentioned link, the OSM file is extracted. A small sample file of the large extracted OSM file is retrieved using k = 100.

## Challenges Encountered

* Some street names have characters such as

      * left barcket (

      * right bracket )

      * comma ,

      * hyphen -

      * Full stop .

      * Slashes /,\.

* Street names have the postal code of that place attached to the name. Eg : __Delhi-110021__

* Key “k” tags of some nodes have their values "v" written in different languages such as Urdu, Hindi,etc to denote a location so had problem in creating the db files from csv.


The function __update_name(name, mapping)__ is used to remove the problems encountered during the cleaning process. It rectifies the human errors caused in typing the names. Along with the commonly used names such as Street, Road, Avenue ,etc., many of the streets are named in the local languages such as Marg, Vihar , etc. Streets near the highway are named as NH - XY where XY denotes a particular number according to the concerned area. Eg : NH-24, etc.

## Function used for correcting the street names

```
expected = ["Street", "Avenue", "Plaza", "Colony", "Lane", "Road", "Vihar", "University", "Patparganj","Marg",
         "Area", "Market", "Enclave"];

mapping = { "St": "Street", "St.": "Street", "Rd": "Road", "Ave": "Avenue",  "Raod": "Road" }

def update_name(name, mapping):

    unwanted = ['(',')','/']  # List of unwanted characters
    ch_name = ''                  # Create an empty string

    # for loop to remove unwanted characters
    for i in range(len(name)):
        if name[i] not in unwanted:
            ch_name = ch_name + name[i]

    # Slicing to remove '-'
    if ch_name[0]=='-':
        ch_name = ch_name[1:]
    if ch_name[-1]=='-' or ch_name[-1]==',':
        ch_name = ch_name[:-1]

    # To remove postal codes from street name
    if '-' in ch_name:
        ch = ch_name.split('-')
        if len(ch[1])>=4:
            ch_name = ch[0]

     # Capitalize the first letter of each street name and convert other letters to lower case
    low_name = ch_name.lower()
    if ' ' in low_name:
        ch_name = ''
        t = low_name.split(' ')
        for i in t:
            ch_name = ch_name + ' ' + i.capitalize()
    else:
        ch_name = low_name.capitalize()

    # Mapping
    k = mapping.keys()
    key_list = list(k)
    for abrev in key_list:
        if abrev in ch_name.split():
            ch_name = ch_name.replace(abrev,mapping[abrev])

    return ch_name
```    
After the cleaning has been completed, CSV files are made from the cleaned data in the OSM files. The CSV files are namely nodes.csv, nodes_tags.csv, ways.csv, ways_tags.csv and ways_nodes.csv.

The csv files are imported into a database called __delhi.db__ as tables with the names as nodes, nodes_tags, ways, ways_tags and ways_nodes.

 # Data Overview and Additional Ideas

## Files size
```
new-delhi_india.osm.bz2 (compressed) --------- 43.9 MB  
new-delhi_india.osm -------------------------- 718 MB  
delhi.db (database)--------------------------- 418 MB  
nodes.csv ------------------------------------ 274 MB  
nodes_tags.csv ------------------------------- 1.49 MB  
ways.csv ------------------------------------- 40.8 MB  
ways_tags.csv -------------------------------- 25.1 MB  
ways_nodes.csv ------------------------------- 101 MB  
sample-delhi.osm (in repo)-------------------- 7.25 MB  
```



### Number of nodes

```sql
sqlite> SELECT COUNT(*) FROM nodes;
```


3415796


### Number of tags in nodes

```sql
sqlite> SELECT COUNT(*) FROM nodes_tags;
```

42658


### Number of ways

```sql
sqlite> SELECT COUNT(*) FROM ways;
```

695247


### Number of tags in ways

```sql
sqlite> SELECT COUNT(*) FROM ways_tags;
```

767411


### Number of nodes defined in ways

```sql
sqlite> SELECT COUNT(*) FROM ways_nodes;
```

4226229

### Number of unique users

```sql

sqlite> SELECT COUNT(DISTINCT(k.uid))

        FROM (SELECT uid FROM nodes UNION ALL SELECT uid FROM ways) as k;
```

1501


### Top 10 contributing users to the OSM data

```sql
sqlite> SELECT k.user, COUNT(*) as number_of_contributions

        FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) as k

        GROUP BY k.user

        ORDER BY number_of_contributions DESC

        LIMIT 10;

```

```
Oberaffe     |269770
premkumar    |164032
saikumar     |159906
Naresh08     |136333
anushap      |133391
sdivya       |129895
anthony1     |125833
himabindhu   |122729
sathishshetty|122242
Apreethi     |114110
```


### Number of users havng only 1 post

```sql
sqlite> SELECT COUNT(*)

        FROM

        (SELECT k.user, COUNT(*) as num

        FROM (SELECT user FROM nodes UNION ALL SELECT user FROM ways) as k

        GROUP BY k.user

        HAVING num=1);
```

415


## Additional ideas for improvement of OSM dataset

### Contribution Statistics

The statistics below shows the contribution of different users for OSM data of New Delhi, India.  

* User having the maximum contribution (Oberaffe) : 6.56%
* Contribution by the top 2 users ('Oberaffe' and 'premkumar'): 10.55%
* Contibution by the top 10 users  : 35.95%
* Contribution of total number of users having only 1 posts : 0.01%
* So the rest of the users(i.e., not mentioned above) have a fairly large contibution : 64.04%

From the statistics above it is clear that contribution by approximately 10 users to the OSM data for New Delhi is more than the rest of the users involved. So more encouragement must be provided to different users to contribute to the dataset.

The contribution by different users can be improved using techniques such as [gamification](https://en.wikipedia.org/wiki/Gamification). Here the mundane task of collecting data by the contributing users can be made more enjoyable by doing it as a part of a game. The user can collect the necessary data and can cross-verify it with other databases such as Google Maps or the regional navigation system of India - [NAVIC](https://en.wikipedia.org/wiki/Indian_Regional_Navigation_Satellite_System). So if the data given by the user matches that of these databases  then the user can be given an incentive such as point or a badge to keep his interest in collecting these datas.

```sql
sqlite> SELECT n.lat, n.lon, nt.value
        FROM nodes as n, nodes_tags as nt
        WHERE (nt.id = n.id) and (nt.value = 'Talkatora Stadium');
```

```
28.622325|77.1930203|Talkatora Stadium
```  

 [Data obtained from Google Map](https://www.google.co.in/maps/place/Talkatora+Indoor+Stadium/@28.6228037,77.1916556,16.04z/data=!4m5!3m4!1s0x0:0xf08c226605e04fa!8m2!3d28.6224523!4d77.1938777)
```
28.62249 | 77.19382 | Talkotra Indoor Stadium
```


### Benefits and Issues of the mentioned improvement

One more advantage of this method is that user will have a chance of verifying the data  that he is entering with other databse data so errors in places' Name, pincodes, latitude and longtitude., etc can be minimized.
But an issue with this method is that there is a chance that the contributing users can simply copy data from the preexisting databases to their database so an error existing in other database can enter into the OSM data too. So credibility can be a minor issue.
Also it is always good to have the data collected by a local person of that area as their most of the places' will have its name in the local/regional language.  

Also few of the places' names in New Delhi have been put up in the regional language of Hindu or Urdu. Also some nodes have been marked in different languages so a mapping between the language key and its name can be helpful for tourists or people who are not much familiar with the regional language.


### Examples of types of language used to denote a node

```sql
sqlite> SELECT key, value

        FROM nodes_tags

        WHERE type = 'name'

        LIMIT 10;
```
```
ace       |New delhi
af        |Nieu-Delhi
am        |ßèÆßïì ßï┤ßêè
an        |Nueva Delhi
ang       |N─½╞┐e Delhi
ar        |┘å┘è┘ê╪»┘ä┘ç┘è
bat-smg   |Naujas─ùs Del─ùs
be        |╨¥╤î╤Ä-╨ö╤ì╨╗╤û
be-tarask |╨¥╤î╤Ä-╨ö╤ì╨╗╤û
bg        |╨¥╤Ä ╨ö╨╡╨╗╤à╨╕
```


### Invalid Postal codes and Cities in the database

For some key 'k' tags of 'postalcode' instead of the numbers names of the places have been given in the 'value'. Also some postal codes were not of the New-Delhi region and some are invalid postcodes.

### Postal codes

The SQL Query below is used to find the postal codes in the database and find their number of occurences.

```sql
sqlite> SELECT tags.value, COUNT(*) as Number
        FROM (SELECT * FROM nodes_tags
        UNION ALL
        SELECT * FROM ways_tags) as tags
        WHERE tags.key='postcode'
        GROUP BY tags.value
        ORDER BY Number ASC;
```
(Here in the answer I have just written the faulty values of postcode as the full list have taken a whole lot of space.)

```
420420              |1
560102              |1
78703               |1
Sunpat House Village|1
sdf                 |1
110031v             |2
2010                |1
```

From the above results, we can infer that in addition to the faulty values some postcodes might be of some other place and have crept into the OSM file bu human error.

### Sort using the cities

```sql
sqlite> SELECT tags.value, COUNT(*) as count
        FROM (SELECT * FROM nodes_tags UNION ALL
        SELECT * FROM ways_tags) as tags
        WHERE tags.key = 'city'
        GROUP BY tags.value
        ORDER BY count ASC;
```

(Here again in the answer I have just written the cities or places which are not in New Delhi and the user had made errors while entering these data for New Delhi.)

```
ad                  |1
Austin              |1
bangalore           |1
hyderabad           |1
meerut              |1
survey              |1
Ghaziabad, UP, India|1
Ghaziabazd          |1
Gurgaon, Haryana    |1
```

Hence, the OSM of New-Delhi has some cities data in it which does not belong there such as Bangalore, Hyderabad, meerut, etc. These errors can be removed to improve the data.

## Additional database exploration

### Top 10 most available amenities

```sql
sqlite> SELECT value, COUNT(*) as Number
        FROM nodes_tags
        WHERE key = 'amenity'
        GROUP BY value
        ORDER BY Number DESC
        LIMIT 10;
```

```
restaurant      |234
fuel            |217
atm             |207
place_of_worship|200
bank            |177
school          |161
fast_food       |134
parking         |90
hospital        |88
cafe            |80
```

### Number of localities

```sql
sqlite> SELECT COUNT(*)
        FROM nodes_tags
        WHERE value = 'locality';
```

889

###  Group using Postcode values

```sql
sqlite> SELECT tags.value, COUNT(*) as Number
        FROM (SELECT * FROM nodes_tags
        UNION ALL
        SELECT * FROM ways_tags) as tags
        WHERE tags.key='postcode'
        GROUP BY tags.value
        ORDER BY Number DESC
        LIMIT 10;
```
```
110087|509
122001|100
110092|65
100006|59
110075|55
201301|54
110085|37
110042|36
110088|29
110070|28
```

###   Top 10 types of roads near the highway

```sql
sqlite> SELECT value, count(*) as Number
        FROM ways_tags
        WHERE key = 'highway'
        GROUP BY value
        ORDER BY Number DESC
        LIMIT 10;
```
```
residential    |106041
service        |13177
living_street  |5958
tertiary       |4564
unclassified   |3110
secondary      |2084
footway        |1782
track          |918
construction   |810
path           |644
```

### Top 10 types of power generation

```sql
sqlite> SELECT value, count(*) as Number
        FROM nodes_tags
        WHERE key = 'power'
        GROUP BY value
        ORDER BY Number DESC
        LIMIT 10;
```
```
tower       |16269
pole        |180
transformer |21
generator   |14
substation  |6
```
So New_Delhi uses these 5 types of power generation and distribution systems.


### Top 10 leisure facilities available

```sql
sqlite> SELECT value, count(*) as Number
        FROM nodes_tags
        WHERE key = 'leisure'
        GROUP BY value
        ORDER BY Number DESC
        LIMIT 10;
```
```
park            |49
playground      |19
pitch           |18
sports_centre   |13
fitness_centre  |5
stadium         |4
swimming_pool   |3
water_park      |3
fishing         |2
fitness_station |1
```


### Top 10 types of shops found around New Delhi

```sql
sqlite> SELECT value, count(*) as Number
        FROM nodes_tags
        WHERE key = 'shop'
        GROUP BY value
        ORDER BY Number DESC
        LIMIT 10;
```
```
street_vendor |329
supermarket   |82
convenience   |55
bakery        |43
clothes       |43
alcohol       |22
jewelry       |20
electronics   |17
books         |16
hairdresser   |16
```


### Top 10 types of Cuisines available in New-Delhi

```sql
sqlite> SELECT nodes_tags.value, COUNT(*) as num
        FROM nodes_tags, (SELECT DISTINCT(id) FROM nodes_tags WHERE value='restaurant') as subq
        ON nodes_tags.id=subq.id
        WHERE nodes_tags.key='cuisine'
        GROUP BY nodes_tags.value
        ORDER BY num DESC
        LIMIT 10;
```

```
indian       |23
regional     |11
pizza        |8
North_Indian |5
chinese      |5
vegetarian   |5
burger       |4
korean       |2
sandwich     |2
thai         |2
```

## Conclusion

In this project, cleaning of the data in the OSM file of New-Delhi was done. During the process of cleaning
some errors in the data entry by the contributing users was also found and mentioned in this project report.
Also some ideas for improvement of data from  my part were also found and shown using the SQL queries. Also
i tried exploring the given database using SQL queries.

## Github Repo for this project
[Jeswin's Github repo for Wrangle-Openstreetmap-data](https://github.com/jeswingeorge/Wrangle-Openstreetmap-data)
