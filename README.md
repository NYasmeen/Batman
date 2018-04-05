# Batman

import urllib2
from bs4 import BeautifulSoup

import json

contents = urllib2.urlopen("https://www.bt.com/consumerFaultTracking/public/faults/tracking.do?pageId=31").read()

status = BeautifulSoup(contents,"html.parser")

xyz = status.find('ul', id ='service-summaries-new')#, class_="not-ie mobile-accordian ma-styled")


A = []
B = []
C = []


Yay = xyz.find('li', id = "broadband-summaries")
final_value = Yay.find('iframe')
device_status = urllib2.urlopen(final_value['src'])

connection_status = BeautifulSoup(device_status,"html.parser")

table = connection_status.find('table', class_ = 'service_status_table_consumer')
if table is not None:
        tr_cells = table.find_all('tr', style = 'cursor: pointer;')
        for tr_cell in tr_cells:
            cells = tr_cell.find_all('td')
            A.append(cells[0].find(text=True))
            B.append(cells[1].find(text=True))
            C.append(cells[2].find(text=True))
            





myDict = { "Description": C[0] , "Date": A[0],"Status": B[0]}

myJson = json.dumps(myDict).encode("utf-8")


print myJson
