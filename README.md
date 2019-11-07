# Scrap_For_Sectors_Python using python
from googlesearch import search
import requests
from bs4 import BeautifulSoup
import pandas as pd
import sqlite3

df = pd.read_excel(r'C:\Users\Belema Grend\Desktop\clup.xlsx',"Sheet1") #reads data from excel file
#print(df['Made_By'])#prints out data from the excel file the ".head" property prints the first 5 rows
"""the .loc[:, ['col1', 'col2']] allows you read columns in the second square brackets """
conn = sqlite3.connect('test.db')
#conn.execute('''CREATE TABLE COMPANY
#         (ID INT PRIMARY KEY     NOT NULL,
#         NAME           TEXT    NOT NULL,
#         AGE            INT     NOT NULL,
#         ADDRESS        CHAR(50),
#         SALARY         REAL);''')
tvar=df.values
subset = df[['S/No', 'Title', 'Made_By','Addy','Wage']]  
tuples = [tuple(x) for x in subset.values]
for lin in tuples:
    print(lin,"\n")   
    conn.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (?,?,?,?,?)",lin);
conn.commit()
cursor = conn.execute("SELECT id,age, name, address, salary from COMPANY order by id")
query = "Belema Grend -twitter"
for url in search(query, stop=1):
    r = requests.get(url)
    soup = BeautifulSoup(r.text, "html5lib")
    datunim = soup.get_text()
    datunim = datunim.casefold()
    tlist = ['linkedion','Belema','Grend']
    if any(ele in datunim for ele in tlist):
        print("found it" + "\n" + "next section please")
    else: print("no luck today \n" + datunim)
conn.close()
