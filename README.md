# gramoday-task
import requests
import urllib.request
import time
from bs4 import BeautifulSoup

# as the data for 2020 was checked on website , it came in multiple pages but the links does changes , it can be done by running a loop over list of links of each date
# this can be done as 
# URL =[] ,
# for url in range(0,365):

url = 'https://agmarknet.gov.in/SearchCmmMkt.aspx?Tx_Commodity=24&Tx_State=UP&Tx_District=1&Tx_Market=0&DateFrom=01-Jan-2020&DateTo=31-Jan-2020&Fr_Date=01-Jan%022020&To_Date=31-Jan%022020&Tx_Trend=0&Tx_CommodityHead=Potato&Tx_StateHead=Uttar+Pradesh&Tx_DistrictHead=Agra&Tx_MarketHead=--Select--'
response = requests.get(url)
soup = BeautifulSoup(response.content,'html.parser')
sp=soup.findAll('table')
sp=sp[0]
ls=sp.findAll('tr')
table_data = []
i=0

for tr in ls: # find all tr's from table's tbody
    t_row = []

    for td in tr.find_all("td"): 
        t_row.append(td.text.replace('\n', '').strip())
    table_data.append(t_row)
table_data=table_data[1:]
import csv

for i in table_data:
    with open("Agr.csv", 'a') as out_file:
        writer = csv.writer(out_file, delimiter=',', quoting=csv.QUOTE_MINIMAL)
    
        writer.writerow(i)
