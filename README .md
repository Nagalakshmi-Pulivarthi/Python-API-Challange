
# WeatherPy
<!--In this Project, I created a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. To accomplish this, i usde a simple Python library, the OpenWeatherMap API and  a representative model of weather across world cities.-->


```python
#Dependencies
import numpy as np
import pandas as pd
import requests as req
import matplotlib.pyplot as plt
from citipy import citipy
import seaborn as sns
import json
import time
import datetime
from password import OPENWEATHER_API_KEY #import weather key from password.py file
```

# Generate Cities List


```python
#Generating the cities list by using citypy
cities =[]

for i in np.random.uniform(-90,90,50):
    for j in np.random.uniform(-180 ,180,50):
            city = citipy.nearest_city(i,j)
            if not  city.city_name in cities:
                cities.append(city.city_name)            
print(cities)
print(len(cities)) 
         
```

    ['isangel', 'namibe', 'marcona', 'acari', 'coxim', 'rikitea', 'tautira', 'puerto ayora', 'bowen', 'georgetown', 'bengkulu', 'karratha', 'neiafu', 'katherine', 'arani', 'pisco', 'camana', 'puerto quijarro', 'pangai', 'vila', 'buritizeiro', 'poum', 'huarmey', 'rundu', 'palabuhanratu', 'labuhan', 'caravelas', 'avarua', 'oxford', 'trapani', 'marystown', 'elko', 'jiuquan', 'shelburne', 'wajima', 'farap', 'oga', 'balkanabat', 'valencia', 'yerkoy', 'kashi', 'brigantine', 'fortuna', 'trenton', 'seydi', 'kapaa', 'glenwood springs', 'changli', 'gurgan', 'tortoli', 'dongsheng', 'vila franca do campo', 'price', 'shelbyville', 'qinhuangdao', 'yumen', 'grass valley', 'clearlake', 'dandong', 'point pleasant', 'haibowan', 'mahon', 'horta', 'kutahya', 'vrangel', 'yamada', 'camacha', 'kodiak', 'agarak', 'ushuaia', 'busselton', 'albany', 'hermanus', 'cape town', 'new norfolk', 'hobart', 'mataura', 'port elizabeth', 'bluff', 'punta arenas', 'vaini', 'bredasdorp', 'taolanaro', 'port alfred', 'mahebourg', 'neuquen', 'portland', 'westport', 'maldonado', 'ancud', 'jamestown', 'arraial do cabo', 'tsihombe', 'saint-philippe', 'port lincoln', 'umzimvubu', 'lebu', 'santa rosa', 'cidreira', 'knysna', 'souillac', 'laguna', 'mar del plata', 'margate', 'bahia blanca', 'vardo', 'yellowknife', 'mys shmidta', 'barrow', 'pevek', 'chokurdakh', 'tuktoyaktuk', 'illoqqortoormiut', 'dikson', 'leningradskiy', 'barentsburg', 'tiksi', 'nizhneyansk', 'qaanaaq', 'berlevag', 'ilulissat', 'saskylakh', 'khatanga', 'cherskiy', 'amderma', 'talnakh', 'hasaki', 'marv dasht', 'hilo', 'kailua', 'hamilton', 'ribeira grande', 'baghdad', 'santa cruz de la palma', 'warqla', 'lakeway', 'ismailia', 'sakakah', 'tunxi', 'nacozari', 'half moon bay', 'shenjiamen', 'wadi musa', 'ahumada', 'sentyabrskiy', 'lumberton', 'warrington', 'marsh harbour', 'karak', 'fredericksburg', 'kushima', 'joshimath', 'fukue', 'saint george', 'naze', 'nikolskoye', 'nichinan', 'los llanos de aridane', 'yaan', 'katsuura', 'kot addu', 'barkhan', 'nalut', 'sulphur', 'atuona', 'cutervo', 'kiunga', 'hithadhoo', 'mpanda', 'canutama', 'saleaula', 'lata', 'jacareacanga', 'padang', 'faanui', 'meulaboh', 'kalemie', 'tual', 'eirunepe', 'mbanza-ngungu', 'lolua', 'pekalongan', 'tari', 'jati', 'karema', 'mgandu', 'tshikapa', 'victoria', 'mirador', 'vaitupu', 'carauari', 'cabedelo', 'matadi', 'northam', 'carnarvon', 'narrabri', 'richards bay', 'calvinia', 'geraldton', 'luderitz', 'flinders', 'ahipara', 'port shepstone', 'port augusta', 'armidale', 'quthing', 'kaeo', 'durban', 'coquimbo', 'bambous virieux', 'santa fe', 'imbituba', 'aliwal north', 'dubbo', 'oni', 'sainte-maxime', 'karatau', 'lagoa', 'winnemucca', 'erenhot', 'zhanakorgan', 'lander', 'iwanai', 'muros', 'hovd', 'date', 'bethel', 'nemuro', 'storm lake', 'harvard', 'altamont', 'battle creek', 'jaca', 'tomakomai', 'sitka', 'riverton', 'torbay', 'hami', 'ochamchira', 'arys', 'olga', 'akdepe', 'yarmouth', 'mitchell', 'severo-kurilsk', 'port hardy', 'hamburg', 'shizunai', 'bontang', 'thinadhoo', 'bonthe', 'carutapera', 'kumi', 'rantauprapat', 'chepareria', 'namatanai', 'albania', 'bairiki', 'dujuma', 'lorengau', 'oyem', 'hobyo', 'matara', 'harper', 'pemangkat', 'aquiraz', 'amapa', 'sri aman', 'sibolga', 'samusu', 'mogadishu', 'barcelos', 'acarau', 'la macarena', 'bongandanga', 'hambantota', 'touros', 'coahuayana', 'codrington', 'bombay', 'pyay', 'jagdalpur', 'san vicente', 'teknaf', 'lompoc', 'west bay', 'bathsheba', 'butaritari', 'kihei', 'dicabisagan', 'najran', 'arlit', 'araouane', 'saint-francois', 'marawi', 'nouakchott', 'akyab', 'salalah', 'allapalli', 'miragoane', 'dong hoi', 'faya', 'savannah bight', 'port maria', 'airai', 'abha', 'esperance', 'ballina', 'vicuna', 'vila velha', 'avera', 'ambovombe', 'sao joao da barra', 'yulara', 'vao', 'prieska', 'dingle', 'petropavlovsk-kamchatskiy', 'erzin', 'provideniya', 'semey', 'poronaysk', 'white rock', 'killarney', 'markivka', 'krasnyy chikoy', 'bechyne', 'raymond', 'turzovka', 'svatove', 'gravelbourg', 'kyra', 'alihe', 'poyarkovo', 'milove', 'chikoy', 'ketchikan', 'nanortalik', 'maple creek', 'karkaralinsk', 'sainte-anne-des-monts', 'moron', 'arkhara', 'burshtyn', 'blairmore', 'thunder bay', 'praia da vitoria', 'magole', 'kananga', 'mtambile', 'lubao', 'asau', 'novo aripuana', 'tayu', 'kieta', 'samalaeulu', 'mombaca', 'buala', 'ambilobe', 'amahai', 'san cristobal', 'sao geraldo do araguaia', 'singaraja', 'jutai', 'moyobamba', 'gamba', 'teluknaga', 'husavik', 'polyarnyy', 'tasiilaq', 'bud', 'bolungarvik', 'upernavik', 'yar-sale', 'aasiaat', 'pangnirtung', 'norman wells', 'lakselv', 'brae', 'ostrovnoy', 'thompson', 'batagay-alyta', 'severnyy', 'bilibino', 'srednekolymsk', 'clyde river', 'zhigansk', 'pangody', 'fairbanks', 'lyngseidet', 'belushya guba', 'tumannyy', 'narsaq', 'longyearbyen', 'mehamn', 'kiama', 'tres arroyos', 'saldanha', 'karamea', 'vilcun', 'colac', 'mount gambier', 'rocha', 'warrnambool', 'piopio', 'lakes entrance', 'kulhudhuffushi', 'buenos aires', 'eyl', 'ciudad bolivar', 'bac lieu', 'totness', 'sabang', 'banyo', 'san patricio', 'acapulco', 'cabo san lucas', 'tafo', 'gadung', 'ifo', 'otukpo', 'simbahan', 'upata', 'makakilo city', 'sao filipe', 'kavieng', 'ratnapura', 'bandarbeyla', 'obo', 'kalmunai', 'mabaruma', 'kloulklubed', 'farias brito', 'ipixuna', 'alagoa nova', 'floriano', 'maneromango', 'atambua', 'picos', 'inyonga', 'humaita', 'cibeureum', 'manicore', 'dibaya', 'kimbe', 'vikindu', 'soyo', 'pringsewu', 'maumere', 'sao felix do xingu', 'campos sales', 'east london', 'iqaluit', 'chagda', 'kichmengskiy gorodok', 'uvat', 'hofn', 'fort nelson', 'syamzha', 'salym', 'lokosovo', 'grindavik', 'lensk', 'hay river', 'tigil', 'dukat', 'lerwick', 'hanko', 'grand centre', 'nome', 'uptar', 'strezhevoy', 'haines junction', 'olafsvik', 'la ronge', 'aseri', 'juneau', 'begunitsy', 'attawapiskat', 'togur', 'lesnoy', 'talaya', 'gornopravdinsk', 'qaqortoq', 'ust-maya', 'tommot', 'amot', 'belyy yar', 'wanning', 'jejuri', 'ponta do sol', 'higuey', 'nicolas bravo', 'bilma', 'warangal', 'sur', 'tawkar', 'letpadan', 'escarcega', 'the valley', 'kutum', 'payo', 'pa sang', 'nishihara', 'shirwal', 'jumla', 'yafran', 'daxian', 'tezu', 'shahr-e kord', 'san angelo', 'ankang', 'rosarito', 'semirom', 'sonoita', 'turayf', 'surt', 'brownwood', 'port hueneme', 'lasa', 'natchez', 'ponta delgada', 'mehran', 'bani walid', 'tanabe', 'chaman', 'bolshaya atnya', 'krasnoyarsk-66', 'arman', 'sarana', 'khani', 'kazachinskoye', 'vestbygda', 'tinskoy', 'auce', 'saint anthony', 'birzai', 'fort saint john', 'vestmannaeyjar', 'chapais', 'shestakovo', 'zolotinka', 'port-cartier', 'gavrilov posad', 'vigrestad', 'liseleje', 'rzhev', 'mochalishche', 'sukhoverkovo', 'kansk', 'kozlovo', 'novosokolniki', 'chara', 'palmer', 'ayan', 'kataysk', 'kolosovka', 'vidim', 'flin flon', 'yaya', 'bonnyville', 'kaitangata', 'chuy', 'goryachegorsk', 'ronne', 'okha', 'moletai', 'chernenko', 'tuma', 'saint-augustin', 'malino', 'tynda', 'meadow lake', 'satka', 'sioux lookout', 'harlingen', 'mnogovershinnyy', 'takhtamygda', 'chumikan', 'sobolevo', 'wladyslawowo', 'zhigalovo', 'mackenzie', 'berdyaush', 'tatarsk', 'sept-iles', 'fort saint james', 'iznoski', 'deputatskiy', 'mezen', 'college', 'aykhal', 'qasigiannguit', 'rognan', 'nyurba', 'skagastrond', 'leshukonskoye', 'udachnyy', 'egvekinot', 'baykit', 'igarka', 'tura', 'sisimiut', 'lavrentiya', 'inuvik', 'naryan-mar', 'dalvik', 'verkhoyansk', 'kuusamo', 'aklavik', 'oksfjord', 'tromso', 'karaul', 'sovetskiy', 'sorland', 'andenes', 'klaksvik', 'kangaatsiaq', 'kamenka', 'skjervoy', 'gravdal', 'sistranda', 'belaya gora', 'kharp', 'guantanamo', 'lahaina', 'guerrero negro', 'dwarka', 'frontera', 'chiang rai', 'santa maria', 'kodinar', 'nouadhibou', 'tomatlan', 'ibra', 'tateyama', 'guisa', 'constitucion', 'mundo nuevo', 'san quintin', 'atar', 'tessalit', 'cozumel', 'seybaplaya', 'luang prabang', 'loikaw', 'phayao', 'bodden town', 'rayagada', 'samana', 'gonaives', 'mae hong son', 'tepalcatepec', 'lingao', 'benito juarez', 'nara', 'arecibo', 'mangrol', 'ustye', 'maniitsoq', 'whitehorse', 'megion', 'teguldet', 'tilichiki', 'tarnogskiy gorodok', 'oparino', 'atka', 'okhotsk', 'vanavara', 'sozimskiy', 'solsvik', 'nizhniy kuranakh', 'uppsala', 'paamiut', 'homer', 'solnechnyy', 'luban', 'malinovoye ozero', 'katyuzhanka', 'vostok', 'charyshskoye', 'glushkovo', 'rosetown', 'bonavista', 'talovaya', 'talakan', 'kungurtug', 'zachagansk', 'sudzha', 'dauphin', 'katangli', 'olesnica', 'hearst', 'bayangol', 'kyzyl-mazhalyk', 'neepawa', 'kalga', 'pinawa', 'samoylovka', 'ashford', 'longlac', 'bethanien', 'bongaree', 'santiago del estero', 'broken hill', 'saint-joseph', 'presidencia roque saenz pena', 'inhambane', 'orkney', 'ampanihy', 'grand river south east', 'santa cecilia', 'gold coast', 'bokspits', 'russell', 'roma', 'tucuman', 'uri', 'pacific grove', 'huilong', 'abu kamal', 'lake havasu city', 'nador', 'havelock', 'birin', 'linxia', 'tral', 'misratah', 'lardos', 'jubayl', 'hope', 'wilmington', 'semnan', 'portales', 'roswell', 'asfi', 'juegang', 'pombia', 'darnah', 'sun city west', 'tawzar', 'harsin', 'virginia beach', 'anchorage', 'severnyy-kospashskiy', 'turtas', 'palana', 'pavda', 'beringovskiy', 'high level', 'kargasok', 'sandwick', 'kropotkin', 'yurla', 'yayva', 'kardla', 'kologriv', 'nuuk', 'olgina', 'volosovo', 'vokhma', 'kolpashevo', 'boksitogorsk', 'paramonga', 'tunduru', 'kirakira', 'xapuri', 'morehead', 'benguela', 'gizo', 'maragogi', 'barra', 'cazaje', 'camacupa', 'madimba', 'soe', 'kanigoro', 'pangoa', 'chicama', 'mutsamudu', 'honiara', 'formoso do araguaia', 'olinda', 'luau', 'merauke', 'gondanglegi', 'tamandare', 'black river', 'pathein', 'dakar', 'vieques', 'zacualpan', 'ribeira brava', 'ginda', 'guntur', 'jamiltepec', 'ixtapa', 'maghama', 'ewa beach', 'maunabo', 'mao', 'labutta', 'charlestown', 'benemerito de las americas', 'santa isabel', 'sal rei', 'meyungs', 'santa cruz', 'kafanchan', 'vung tau', 'kavaratti', 'phan rang', 'raga', 'wa', 'carora', 'batticaloa', 'maturin', 'ko samui', 'garowe', 'mullaitivu', 'goundi', 'bosconia', 'kagoro', 'cayenne', 'virapandi', 'mana', 'ciudad ojeda', 'port blair', 'sholavandan', 'ranong', 'sorong', 'port-gentil', 'axim', 'tabiauea', 'nueva loja', 'manaus', 'prainha', 'quevedo', 'manokwari', 'pillaro', 'barreirinhas', 'boende', 'mwingi', 'tidore', 'muana', 'barawe', 'tena', 'fougamou', 'trairi', 'mimongo', 'komsomolskiy', 'roald', 'honningsvag', 'skalistyy', 'rypefjord', 'kovdor', 'kautokeino', 'karasjok', 'snezhnogorsk', 'beisfjord', 'walvis bay', 'poya', 'henties bay', 'sakaraha', 'farafangana', 'alofi', 'rockhampton', 'rehoboth', 'amambai', 'saint-leu', 'moerai', 'manakara', 'louis trichardt', 'taubate', 'xinyang', 'birjand', 'tianpeng', 'ahuimanu', 'yol', 'xihe', 'jadu', 'shingu', 'muroto', 'andrews', 'tubruq', 'canico', 'dehloran', 'progreso', 'minggang', 'kahului', 'mrirt', 'viedma', 'te anau', 'san carlos de bariloche', 'necochea', 'burnie', 'launceston', 'castro', 'tuatapere', 'rawson', 'plettenberg bay', 'kruisfontein', 'coihaique', 'aksarka', 'havoysund', 'batsfjord', 'dudinka']
    907
    

## Perform API Calls


```python
#Taking api ,url  assigining the cities into the Url
app_key=OPENWEATHER_API_KEY  #use the api key from password file
url="http://api.openweathermap.org/data/2.5/weather?"
units='Imperial'
final_url=url+"&appid="+app_key+"units="+units

city_number=0
city_data =[]
setsize=50

print("Begining Data Retrieval")
print(".......................")
for city in cities:
    final_url=url+"appid="+app_key+"&q="+city+"&units="+units
    
    print(" Processing Record  " +str(city_number % setsize) + " of" + " set "+str(int(city_number/setsize)) +" | " +city )   
    print(final_url)
    weather_response=req.get(final_url)
    status_code =  weather_response.status_code
    
    if(status_code == 200):
        weather_response = weather_response.json()
        if (weather_response["name"].lower() == city.lower()):
            city_data.append(weather_response)
        else:
            print("Skipping as City not found.....")
    else:
         print("Skipping as City not found.....")
    
    city_number +=1 
    if (city_number % setsize ==0):
                time.sleep(10)

    #time.sleep(3)
    
print("......................")
print("Data Retieval Complete.")
print("......................")
```

    Begining Data Retrieval
    .......................
     Processing Record  0 of set 0 | isangel
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=isangel&units=Imperial
     Processing Record  1 of set 0 | namibe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=namibe&units=Imperial
     Processing Record  2 of set 0 | marcona
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=marcona&units=Imperial
    Skipping as City not found.....
     Processing Record  3 of set 0 | acari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=acari&units=Imperial
     Processing Record  4 of set 0 | coxim
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=coxim&units=Imperial
     Processing Record  5 of set 0 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rikitea&units=Imperial
     Processing Record  6 of set 0 | tautira
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tautira&units=Imperial
     Processing Record  7 of set 0 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=puerto ayora&units=Imperial
     Processing Record  8 of set 0 | bowen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bowen&units=Imperial
     Processing Record  9 of set 0 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=georgetown&units=Imperial
     Processing Record  10 of set 0 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bengkulu&units=Imperial
     Processing Record  11 of set 0 | karratha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karratha&units=Imperial
     Processing Record  12 of set 0 | neiafu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=neiafu&units=Imperial
     Processing Record  13 of set 0 | katherine
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=katherine&units=Imperial
     Processing Record  14 of set 0 | arani
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arani&units=Imperial
     Processing Record  15 of set 0 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pisco&units=Imperial
     Processing Record  16 of set 0 | camana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=camana&units=Imperial
     Processing Record  17 of set 0 | puerto quijarro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=puerto quijarro&units=Imperial
     Processing Record  18 of set 0 | pangai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pangai&units=Imperial
     Processing Record  19 of set 0 | vila
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vila&units=Imperial
     Processing Record  20 of set 0 | buritizeiro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=buritizeiro&units=Imperial
     Processing Record  21 of set 0 | poum
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=poum&units=Imperial
     Processing Record  22 of set 0 | huarmey
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=huarmey&units=Imperial
     Processing Record  23 of set 0 | rundu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rundu&units=Imperial
     Processing Record  24 of set 0 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=palabuhanratu&units=Imperial
    Skipping as City not found.....
     Processing Record  25 of set 0 | labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=labuhan&units=Imperial
     Processing Record  26 of set 0 | caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=caravelas&units=Imperial
     Processing Record  27 of set 0 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=avarua&units=Imperial
     Processing Record  28 of set 0 | oxford
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oxford&units=Imperial
     Processing Record  29 of set 0 | trapani
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=trapani&units=Imperial
     Processing Record  30 of set 0 | marystown
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=marystown&units=Imperial
     Processing Record  31 of set 0 | elko
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=elko&units=Imperial
     Processing Record  32 of set 0 | jiuquan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jiuquan&units=Imperial
    Skipping as City not found.....
     Processing Record  33 of set 0 | shelburne
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shelburne&units=Imperial
     Processing Record  34 of set 0 | wajima
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wajima&units=Imperial
     Processing Record  35 of set 0 | farap
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=farap&units=Imperial
     Processing Record  36 of set 0 | oga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oga&units=Imperial
    Skipping as City not found.....
     Processing Record  37 of set 0 | balkanabat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=balkanabat&units=Imperial
     Processing Record  38 of set 0 | valencia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=valencia&units=Imperial
     Processing Record  39 of set 0 | yerkoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yerkoy&units=Imperial
     Processing Record  40 of set 0 | kashi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kashi&units=Imperial
     Processing Record  41 of set 0 | brigantine
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=brigantine&units=Imperial
     Processing Record  42 of set 0 | fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fortuna&units=Imperial
     Processing Record  43 of set 0 | trenton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=trenton&units=Imperial
     Processing Record  44 of set 0 | seydi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=seydi&units=Imperial
     Processing Record  45 of set 0 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kapaa&units=Imperial
     Processing Record  46 of set 0 | glenwood springs
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=glenwood springs&units=Imperial
     Processing Record  47 of set 0 | changli
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=changli&units=Imperial
     Processing Record  48 of set 0 | gurgan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gurgan&units=Imperial
    Skipping as City not found.....
     Processing Record  49 of set 0 | tortoli
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tortoli&units=Imperial
     Processing Record  0 of set 1 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dongsheng&units=Imperial
     Processing Record  1 of set 1 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vila franca do campo&units=Imperial
     Processing Record  2 of set 1 | price
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=price&units=Imperial
     Processing Record  3 of set 1 | shelbyville
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shelbyville&units=Imperial
     Processing Record  4 of set 1 | qinhuangdao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=qinhuangdao&units=Imperial
     Processing Record  5 of set 1 | yumen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yumen&units=Imperial
     Processing Record  6 of set 1 | grass valley
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=grass valley&units=Imperial
     Processing Record  7 of set 1 | clearlake
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=clearlake&units=Imperial
     Processing Record  8 of set 1 | dandong
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dandong&units=Imperial
     Processing Record  9 of set 1 | point pleasant
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=point pleasant&units=Imperial
     Processing Record  10 of set 1 | haibowan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=haibowan&units=Imperial
    Skipping as City not found.....
     Processing Record  11 of set 1 | mahon
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mahon&units=Imperial
    Skipping as City not found.....
     Processing Record  12 of set 1 | horta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=horta&units=Imperial
     Processing Record  13 of set 1 | kutahya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kutahya&units=Imperial
     Processing Record  14 of set 1 | vrangel
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vrangel&units=Imperial
     Processing Record  15 of set 1 | yamada
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yamada&units=Imperial
     Processing Record  16 of set 1 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=camacha&units=Imperial
     Processing Record  17 of set 1 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kodiak&units=Imperial
     Processing Record  18 of set 1 | agarak
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=agarak&units=Imperial
     Processing Record  19 of set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ushuaia&units=Imperial
     Processing Record  20 of set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=busselton&units=Imperial
     Processing Record  21 of set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=albany&units=Imperial
     Processing Record  22 of set 1 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hermanus&units=Imperial
     Processing Record  23 of set 1 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cape town&units=Imperial
     Processing Record  24 of set 1 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=new norfolk&units=Imperial
     Processing Record  25 of set 1 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hobart&units=Imperial
     Processing Record  26 of set 1 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mataura&units=Imperial
    Skipping as City not found.....
     Processing Record  27 of set 1 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port elizabeth&units=Imperial
     Processing Record  28 of set 1 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bluff&units=Imperial
     Processing Record  29 of set 1 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=punta arenas&units=Imperial
     Processing Record  30 of set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vaini&units=Imperial
     Processing Record  31 of set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bredasdorp&units=Imperial
     Processing Record  32 of set 1 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=taolanaro&units=Imperial
    Skipping as City not found.....
     Processing Record  33 of set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port alfred&units=Imperial
     Processing Record  34 of set 1 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mahebourg&units=Imperial
     Processing Record  35 of set 1 | neuquen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=neuquen&units=Imperial
     Processing Record  36 of set 1 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=portland&units=Imperial
     Processing Record  37 of set 1 | westport
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=westport&units=Imperial
     Processing Record  38 of set 1 | maldonado
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maldonado&units=Imperial
     Processing Record  39 of set 1 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ancud&units=Imperial
     Processing Record  40 of set 1 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jamestown&units=Imperial
     Processing Record  41 of set 1 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arraial do cabo&units=Imperial
     Processing Record  42 of set 1 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tsihombe&units=Imperial
    Skipping as City not found.....
     Processing Record  43 of set 1 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint-philippe&units=Imperial
     Processing Record  44 of set 1 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port lincoln&units=Imperial
     Processing Record  45 of set 1 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=umzimvubu&units=Imperial
    Skipping as City not found.....
     Processing Record  46 of set 1 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lebu&units=Imperial
     Processing Record  47 of set 1 | santa rosa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa rosa&units=Imperial
     Processing Record  48 of set 1 | cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cidreira&units=Imperial
     Processing Record  49 of set 1 | knysna
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=knysna&units=Imperial
     Processing Record  0 of set 2 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=souillac&units=Imperial
     Processing Record  1 of set 2 | laguna
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=laguna&units=Imperial
     Processing Record  2 of set 2 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mar del plata&units=Imperial
     Processing Record  3 of set 2 | margate
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=margate&units=Imperial
     Processing Record  4 of set 2 | bahia blanca
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bahia blanca&units=Imperial
     Processing Record  5 of set 2 | vardo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vardo&units=Imperial
     Processing Record  6 of set 2 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yellowknife&units=Imperial
     Processing Record  7 of set 2 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mys shmidta&units=Imperial
    Skipping as City not found.....
     Processing Record  8 of set 2 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barrow&units=Imperial
     Processing Record  9 of set 2 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pevek&units=Imperial
     Processing Record  10 of set 2 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chokurdakh&units=Imperial
     Processing Record  11 of set 2 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tuktoyaktuk&units=Imperial
     Processing Record  12 of set 2 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=illoqqortoormiut&units=Imperial
    Skipping as City not found.....
     Processing Record  13 of set 2 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dikson&units=Imperial
     Processing Record  14 of set 2 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=leningradskiy&units=Imperial
     Processing Record  15 of set 2 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barentsburg&units=Imperial
    Skipping as City not found.....
     Processing Record  16 of set 2 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tiksi&units=Imperial
     Processing Record  17 of set 2 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nizhneyansk&units=Imperial
    Skipping as City not found.....
     Processing Record  18 of set 2 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=qaanaaq&units=Imperial
     Processing Record  19 of set 2 | berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=berlevag&units=Imperial
     Processing Record  20 of set 2 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ilulissat&units=Imperial
     Processing Record  21 of set 2 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saskylakh&units=Imperial
     Processing Record  22 of set 2 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=khatanga&units=Imperial
     Processing Record  23 of set 2 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cherskiy&units=Imperial
     Processing Record  24 of set 2 | amderma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=amderma&units=Imperial
    Skipping as City not found.....
     Processing Record  25 of set 2 | talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=talnakh&units=Imperial
     Processing Record  26 of set 2 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hasaki&units=Imperial
     Processing Record  27 of set 2 | marv dasht
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=marv dasht&units=Imperial
    Skipping as City not found.....
     Processing Record  28 of set 2 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hilo&units=Imperial
     Processing Record  29 of set 2 | kailua
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kailua&units=Imperial
     Processing Record  30 of set 2 | hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hamilton&units=Imperial
     Processing Record  31 of set 2 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ribeira grande&units=Imperial
     Processing Record  32 of set 2 | baghdad
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=baghdad&units=Imperial
     Processing Record  33 of set 2 | santa cruz de la palma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa cruz de la palma&units=Imperial
     Processing Record  34 of set 2 | warqla
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=warqla&units=Imperial
    Skipping as City not found.....
     Processing Record  35 of set 2 | lakeway
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lakeway&units=Imperial
     Processing Record  36 of set 2 | ismailia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ismailia&units=Imperial
     Processing Record  37 of set 2 | sakakah
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sakakah&units=Imperial
    Skipping as City not found.....
     Processing Record  38 of set 2 | tunxi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tunxi&units=Imperial
    Skipping as City not found.....
     Processing Record  39 of set 2 | nacozari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nacozari&units=Imperial
    Skipping as City not found.....
     Processing Record  40 of set 2 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=half moon bay&units=Imperial
     Processing Record  41 of set 2 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shenjiamen&units=Imperial
     Processing Record  42 of set 2 | wadi musa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wadi musa&units=Imperial
    Skipping as City not found.....
     Processing Record  43 of set 2 | ahumada
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ahumada&units=Imperial
    Skipping as City not found.....
     Processing Record  44 of set 2 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sentyabrskiy&units=Imperial
    Skipping as City not found.....
     Processing Record  45 of set 2 | lumberton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lumberton&units=Imperial
     Processing Record  46 of set 2 | warrington
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=warrington&units=Imperial
     Processing Record  47 of set 2 | marsh harbour
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=marsh harbour&units=Imperial
     Processing Record  48 of set 2 | karak
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karak&units=Imperial
     Processing Record  49 of set 2 | fredericksburg
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fredericksburg&units=Imperial
     Processing Record  0 of set 3 | kushima
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kushima&units=Imperial
     Processing Record  1 of set 3 | joshimath
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=joshimath&units=Imperial
     Processing Record  2 of set 3 | fukue
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fukue&units=Imperial
     Processing Record  3 of set 3 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint george&units=Imperial
     Processing Record  4 of set 3 | naze
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=naze&units=Imperial
     Processing Record  5 of set 3 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nikolskoye&units=Imperial
    Skipping as City not found.....
     Processing Record  6 of set 3 | nichinan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nichinan&units=Imperial
     Processing Record  7 of set 3 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=los llanos de aridane&units=Imperial
     Processing Record  8 of set 3 | yaan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yaan&units=Imperial
    Skipping as City not found.....
     Processing Record  9 of set 3 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=katsuura&units=Imperial
     Processing Record  10 of set 3 | kot addu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kot addu&units=Imperial
     Processing Record  11 of set 3 | barkhan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barkhan&units=Imperial
     Processing Record  12 of set 3 | nalut
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nalut&units=Imperial
     Processing Record  13 of set 3 | sulphur
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sulphur&units=Imperial
     Processing Record  14 of set 3 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=atuona&units=Imperial
     Processing Record  15 of set 3 | cutervo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cutervo&units=Imperial
    Skipping as City not found.....
     Processing Record  16 of set 3 | kiunga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kiunga&units=Imperial
     Processing Record  17 of set 3 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hithadhoo&units=Imperial
     Processing Record  18 of set 3 | mpanda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mpanda&units=Imperial
     Processing Record  19 of set 3 | canutama
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=canutama&units=Imperial
     Processing Record  20 of set 3 | saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saleaula&units=Imperial
    Skipping as City not found.....
     Processing Record  21 of set 3 | lata
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lata&units=Imperial
     Processing Record  22 of set 3 | jacareacanga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jacareacanga&units=Imperial
     Processing Record  23 of set 3 | padang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=padang&units=Imperial
     Processing Record  24 of set 3 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=faanui&units=Imperial
     Processing Record  25 of set 3 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=meulaboh&units=Imperial
     Processing Record  26 of set 3 | kalemie
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kalemie&units=Imperial
     Processing Record  27 of set 3 | tual
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tual&units=Imperial
     Processing Record  28 of set 3 | eirunepe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=eirunepe&units=Imperial
     Processing Record  29 of set 3 | mbanza-ngungu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mbanza-ngungu&units=Imperial
     Processing Record  30 of set 3 | lolua
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lolua&units=Imperial
    Skipping as City not found.....
     Processing Record  31 of set 3 | pekalongan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pekalongan&units=Imperial
     Processing Record  32 of set 3 | tari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tari&units=Imperial
    Skipping as City not found.....
     Processing Record  33 of set 3 | jati
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jati&units=Imperial
     Processing Record  34 of set 3 | karema
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karema&units=Imperial
     Processing Record  35 of set 3 | mgandu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mgandu&units=Imperial
     Processing Record  36 of set 3 | tshikapa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tshikapa&units=Imperial
     Processing Record  37 of set 3 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=victoria&units=Imperial
     Processing Record  38 of set 3 | mirador
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mirador&units=Imperial
     Processing Record  39 of set 3 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vaitupu&units=Imperial
    Skipping as City not found.....
     Processing Record  40 of set 3 | carauari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=carauari&units=Imperial
     Processing Record  41 of set 3 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cabedelo&units=Imperial
     Processing Record  42 of set 3 | matadi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=matadi&units=Imperial
     Processing Record  43 of set 3 | northam
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=northam&units=Imperial
     Processing Record  44 of set 3 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=carnarvon&units=Imperial
     Processing Record  45 of set 3 | narrabri
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=narrabri&units=Imperial
     Processing Record  46 of set 3 | richards bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=richards bay&units=Imperial
     Processing Record  47 of set 3 | calvinia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=calvinia&units=Imperial
     Processing Record  48 of set 3 | geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=geraldton&units=Imperial
     Processing Record  49 of set 3 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=luderitz&units=Imperial
     Processing Record  0 of set 4 | flinders
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=flinders&units=Imperial
     Processing Record  1 of set 4 | ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ahipara&units=Imperial
     Processing Record  2 of set 4 | port shepstone
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port shepstone&units=Imperial
     Processing Record  3 of set 4 | port augusta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port augusta&units=Imperial
     Processing Record  4 of set 4 | armidale
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=armidale&units=Imperial
     Processing Record  5 of set 4 | quthing
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=quthing&units=Imperial
     Processing Record  6 of set 4 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kaeo&units=Imperial
     Processing Record  7 of set 4 | durban
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=durban&units=Imperial
     Processing Record  8 of set 4 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=coquimbo&units=Imperial
     Processing Record  9 of set 4 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bambous virieux&units=Imperial
     Processing Record  10 of set 4 | santa fe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa fe&units=Imperial
     Processing Record  11 of set 4 | imbituba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=imbituba&units=Imperial
     Processing Record  12 of set 4 | aliwal north
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aliwal north&units=Imperial
     Processing Record  13 of set 4 | dubbo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dubbo&units=Imperial
     Processing Record  14 of set 4 | oni
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oni&units=Imperial
     Processing Record  15 of set 4 | sainte-maxime
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sainte-maxime&units=Imperial
     Processing Record  16 of set 4 | karatau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karatau&units=Imperial
     Processing Record  17 of set 4 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lagoa&units=Imperial
     Processing Record  18 of set 4 | winnemucca
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=winnemucca&units=Imperial
     Processing Record  19 of set 4 | erenhot
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=erenhot&units=Imperial
    Skipping as City not found.....
     Processing Record  20 of set 4 | zhanakorgan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zhanakorgan&units=Imperial
    Skipping as City not found.....
     Processing Record  21 of set 4 | lander
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lander&units=Imperial
     Processing Record  22 of set 4 | iwanai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=iwanai&units=Imperial
     Processing Record  23 of set 4 | muros
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=muros&units=Imperial
     Processing Record  24 of set 4 | hovd
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hovd&units=Imperial
     Processing Record  25 of set 4 | date
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=date&units=Imperial
     Processing Record  26 of set 4 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bethel&units=Imperial
     Processing Record  27 of set 4 | nemuro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nemuro&units=Imperial
     Processing Record  28 of set 4 | storm lake
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=storm lake&units=Imperial
     Processing Record  29 of set 4 | harvard
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=harvard&units=Imperial
     Processing Record  30 of set 4 | altamont
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=altamont&units=Imperial
     Processing Record  31 of set 4 | battle creek
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=battle creek&units=Imperial
     Processing Record  32 of set 4 | jaca
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jaca&units=Imperial
     Processing Record  33 of set 4 | tomakomai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tomakomai&units=Imperial
     Processing Record  34 of set 4 | sitka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sitka&units=Imperial
     Processing Record  35 of set 4 | riverton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=riverton&units=Imperial
     Processing Record  36 of set 4 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=torbay&units=Imperial
     Processing Record  37 of set 4 | hami
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hami&units=Imperial
     Processing Record  38 of set 4 | ochamchira
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ochamchira&units=Imperial
    Skipping as City not found.....
     Processing Record  39 of set 4 | arys
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arys&units=Imperial
     Processing Record  40 of set 4 | olga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=olga&units=Imperial
     Processing Record  41 of set 4 | akdepe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=akdepe&units=Imperial
     Processing Record  42 of set 4 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yarmouth&units=Imperial
     Processing Record  43 of set 4 | mitchell
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mitchell&units=Imperial
     Processing Record  44 of set 4 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=severo-kurilsk&units=Imperial
     Processing Record  45 of set 4 | port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port hardy&units=Imperial
     Processing Record  46 of set 4 | hamburg
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hamburg&units=Imperial
     Processing Record  47 of set 4 | shizunai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shizunai&units=Imperial
     Processing Record  48 of set 4 | bontang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bontang&units=Imperial
     Processing Record  49 of set 4 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=thinadhoo&units=Imperial
     Processing Record  0 of set 5 | bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bonthe&units=Imperial
     Processing Record  1 of set 5 | carutapera
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=carutapera&units=Imperial
     Processing Record  2 of set 5 | kumi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kumi&units=Imperial
     Processing Record  3 of set 5 | rantauprapat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rantauprapat&units=Imperial
     Processing Record  4 of set 5 | chepareria
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chepareria&units=Imperial
     Processing Record  5 of set 5 | namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=namatanai&units=Imperial
     Processing Record  6 of set 5 | albania
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=albania&units=Imperial
     Processing Record  7 of set 5 | bairiki
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bairiki&units=Imperial
    Skipping as City not found.....
     Processing Record  8 of set 5 | dujuma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dujuma&units=Imperial
    Skipping as City not found.....
     Processing Record  9 of set 5 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lorengau&units=Imperial
     Processing Record  10 of set 5 | oyem
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oyem&units=Imperial
     Processing Record  11 of set 5 | hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hobyo&units=Imperial
     Processing Record  12 of set 5 | matara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=matara&units=Imperial
     Processing Record  13 of set 5 | harper
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=harper&units=Imperial
     Processing Record  14 of set 5 | pemangkat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pemangkat&units=Imperial
     Processing Record  15 of set 5 | aquiraz
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aquiraz&units=Imperial
     Processing Record  16 of set 5 | amapa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=amapa&units=Imperial
    Skipping as City not found.....
     Processing Record  17 of set 5 | sri aman
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sri aman&units=Imperial
    Skipping as City not found.....
     Processing Record  18 of set 5 | sibolga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sibolga&units=Imperial
     Processing Record  19 of set 5 | samusu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=samusu&units=Imperial
    Skipping as City not found.....
     Processing Record  20 of set 5 | mogadishu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mogadishu&units=Imperial
     Processing Record  21 of set 5 | barcelos
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barcelos&units=Imperial
     Processing Record  22 of set 5 | acarau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=acarau&units=Imperial
     Processing Record  23 of set 5 | la macarena
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=la macarena&units=Imperial
     Processing Record  24 of set 5 | bongandanga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bongandanga&units=Imperial
     Processing Record  25 of set 5 | hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hambantota&units=Imperial
     Processing Record  26 of set 5 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=touros&units=Imperial
     Processing Record  27 of set 5 | coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=coahuayana&units=Imperial
     Processing Record  28 of set 5 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=codrington&units=Imperial
     Processing Record  29 of set 5 | bombay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bombay&units=Imperial
     Processing Record  30 of set 5 | pyay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pyay&units=Imperial
     Processing Record  31 of set 5 | jagdalpur
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jagdalpur&units=Imperial
     Processing Record  32 of set 5 | san vicente
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san vicente&units=Imperial
     Processing Record  33 of set 5 | teknaf
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=teknaf&units=Imperial
     Processing Record  34 of set 5 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lompoc&units=Imperial
     Processing Record  35 of set 5 | west bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=west bay&units=Imperial
     Processing Record  36 of set 5 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bathsheba&units=Imperial
     Processing Record  37 of set 5 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=butaritari&units=Imperial
     Processing Record  38 of set 5 | kihei
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kihei&units=Imperial
     Processing Record  39 of set 5 | dicabisagan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dicabisagan&units=Imperial
     Processing Record  40 of set 5 | najran
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=najran&units=Imperial
     Processing Record  41 of set 5 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arlit&units=Imperial
     Processing Record  42 of set 5 | araouane
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=araouane&units=Imperial
     Processing Record  43 of set 5 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint-francois&units=Imperial
     Processing Record  44 of set 5 | marawi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=marawi&units=Imperial
     Processing Record  45 of set 5 | nouakchott
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nouakchott&units=Imperial
     Processing Record  46 of set 5 | akyab
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=akyab&units=Imperial
    Skipping as City not found.....
     Processing Record  47 of set 5 | salalah
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=salalah&units=Imperial
     Processing Record  48 of set 5 | allapalli
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=allapalli&units=Imperial
     Processing Record  49 of set 5 | miragoane
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=miragoane&units=Imperial
     Processing Record  0 of set 6 | dong hoi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dong hoi&units=Imperial
     Processing Record  1 of set 6 | faya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=faya&units=Imperial
    Skipping as City not found.....
     Processing Record  2 of set 6 | savannah bight
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=savannah bight&units=Imperial
     Processing Record  3 of set 6 | port maria
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port maria&units=Imperial
     Processing Record  4 of set 6 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=airai&units=Imperial
    Skipping as City not found.....
     Processing Record  5 of set 6 | abha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=abha&units=Imperial
     Processing Record  6 of set 6 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=esperance&units=Imperial
     Processing Record  7 of set 6 | ballina
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ballina&units=Imperial
     Processing Record  8 of set 6 | vicuna
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vicuna&units=Imperial
     Processing Record  9 of set 6 | vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vila velha&units=Imperial
     Processing Record  10 of set 6 | avera
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=avera&units=Imperial
    Skipping as City not found.....
     Processing Record  11 of set 6 | ambovombe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ambovombe&units=Imperial
     Processing Record  12 of set 6 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sao joao da barra&units=Imperial
     Processing Record  13 of set 6 | yulara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yulara&units=Imperial
     Processing Record  14 of set 6 | vao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vao&units=Imperial
     Processing Record  15 of set 6 | prieska
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=prieska&units=Imperial
     Processing Record  16 of set 6 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dingle&units=Imperial
     Processing Record  17 of set 6 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=petropavlovsk-kamchatskiy&units=Imperial
     Processing Record  18 of set 6 | erzin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=erzin&units=Imperial
     Processing Record  19 of set 6 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=provideniya&units=Imperial
     Processing Record  20 of set 6 | semey
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=semey&units=Imperial
     Processing Record  21 of set 6 | poronaysk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=poronaysk&units=Imperial
     Processing Record  22 of set 6 | white rock
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=white rock&units=Imperial
     Processing Record  23 of set 6 | killarney
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=killarney&units=Imperial
     Processing Record  24 of set 6 | markivka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=markivka&units=Imperial
     Processing Record  25 of set 6 | krasnyy chikoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=krasnyy chikoy&units=Imperial
     Processing Record  26 of set 6 | bechyne
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bechyne&units=Imperial
     Processing Record  27 of set 6 | raymond
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=raymond&units=Imperial
     Processing Record  28 of set 6 | turzovka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=turzovka&units=Imperial
     Processing Record  29 of set 6 | svatove
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=svatove&units=Imperial
     Processing Record  30 of set 6 | gravelbourg
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gravelbourg&units=Imperial
     Processing Record  31 of set 6 | kyra
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kyra&units=Imperial
     Processing Record  32 of set 6 | alihe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=alihe&units=Imperial
     Processing Record  33 of set 6 | poyarkovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=poyarkovo&units=Imperial
     Processing Record  34 of set 6 | milove
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=milove&units=Imperial
     Processing Record  35 of set 6 | chikoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chikoy&units=Imperial
    Skipping as City not found.....
     Processing Record  36 of set 6 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ketchikan&units=Imperial
     Processing Record  37 of set 6 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nanortalik&units=Imperial
     Processing Record  38 of set 6 | maple creek
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maple creek&units=Imperial
     Processing Record  39 of set 6 | karkaralinsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karkaralinsk&units=Imperial
    Skipping as City not found.....
     Processing Record  40 of set 6 | sainte-anne-des-monts
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sainte-anne-des-monts&units=Imperial
     Processing Record  41 of set 6 | moron
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=moron&units=Imperial
     Processing Record  42 of set 6 | arkhara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arkhara&units=Imperial
     Processing Record  43 of set 6 | burshtyn
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=burshtyn&units=Imperial
     Processing Record  44 of set 6 | blairmore
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=blairmore&units=Imperial
     Processing Record  45 of set 6 | thunder bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=thunder bay&units=Imperial
     Processing Record  46 of set 6 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=praia da vitoria&units=Imperial
     Processing Record  47 of set 6 | magole
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=magole&units=Imperial
     Processing Record  48 of set 6 | kananga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kananga&units=Imperial
     Processing Record  49 of set 6 | mtambile
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mtambile&units=Imperial
     Processing Record  0 of set 7 | lubao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lubao&units=Imperial
     Processing Record  1 of set 7 | asau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=asau&units=Imperial
     Processing Record  2 of set 7 | novo aripuana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=novo aripuana&units=Imperial
     Processing Record  3 of set 7 | tayu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tayu&units=Imperial
     Processing Record  4 of set 7 | kieta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kieta&units=Imperial
     Processing Record  5 of set 7 | samalaeulu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=samalaeulu&units=Imperial
    Skipping as City not found.....
     Processing Record  6 of set 7 | mombaca
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mombaca&units=Imperial
     Processing Record  7 of set 7 | buala
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=buala&units=Imperial
     Processing Record  8 of set 7 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ambilobe&units=Imperial
     Processing Record  9 of set 7 | amahai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=amahai&units=Imperial
     Processing Record  10 of set 7 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san cristobal&units=Imperial
     Processing Record  11 of set 7 | sao geraldo do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sao geraldo do araguaia&units=Imperial
     Processing Record  12 of set 7 | singaraja
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=singaraja&units=Imperial
     Processing Record  13 of set 7 | jutai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jutai&units=Imperial
     Processing Record  14 of set 7 | moyobamba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=moyobamba&units=Imperial
     Processing Record  15 of set 7 | gamba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gamba&units=Imperial
     Processing Record  16 of set 7 | teluknaga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=teluknaga&units=Imperial
     Processing Record  17 of set 7 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=husavik&units=Imperial
     Processing Record  18 of set 7 | polyarnyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=polyarnyy&units=Imperial
     Processing Record  19 of set 7 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tasiilaq&units=Imperial
     Processing Record  20 of set 7 | bud
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bud&units=Imperial
     Processing Record  21 of set 7 | bolungarvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bolungarvik&units=Imperial
    Skipping as City not found.....
     Processing Record  22 of set 7 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=upernavik&units=Imperial
     Processing Record  23 of set 7 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yar-sale&units=Imperial
     Processing Record  24 of set 7 | aasiaat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aasiaat&units=Imperial
     Processing Record  25 of set 7 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pangnirtung&units=Imperial
     Processing Record  26 of set 7 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=norman wells&units=Imperial
     Processing Record  27 of set 7 | lakselv
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lakselv&units=Imperial
     Processing Record  28 of set 7 | brae
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=brae&units=Imperial
     Processing Record  29 of set 7 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ostrovnoy&units=Imperial
     Processing Record  30 of set 7 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=thompson&units=Imperial
     Processing Record  31 of set 7 | batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=batagay-alyta&units=Imperial
     Processing Record  32 of set 7 | severnyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=severnyy&units=Imperial
     Processing Record  33 of set 7 | bilibino
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bilibino&units=Imperial
     Processing Record  34 of set 7 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=srednekolymsk&units=Imperial
     Processing Record  35 of set 7 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=clyde river&units=Imperial
     Processing Record  36 of set 7 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zhigansk&units=Imperial
     Processing Record  37 of set 7 | pangody
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pangody&units=Imperial
     Processing Record  38 of set 7 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fairbanks&units=Imperial
     Processing Record  39 of set 7 | lyngseidet
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lyngseidet&units=Imperial
     Processing Record  40 of set 7 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=belushya guba&units=Imperial
    Skipping as City not found.....
     Processing Record  41 of set 7 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tumannyy&units=Imperial
    Skipping as City not found.....
     Processing Record  42 of set 7 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=narsaq&units=Imperial
     Processing Record  43 of set 7 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=longyearbyen&units=Imperial
     Processing Record  44 of set 7 | mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mehamn&units=Imperial
     Processing Record  45 of set 7 | kiama
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kiama&units=Imperial
     Processing Record  46 of set 7 | tres arroyos
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tres arroyos&units=Imperial
     Processing Record  47 of set 7 | saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saldanha&units=Imperial
     Processing Record  48 of set 7 | karamea
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karamea&units=Imperial
    Skipping as City not found.....
     Processing Record  49 of set 7 | vilcun
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vilcun&units=Imperial
     Processing Record  0 of set 8 | colac
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=colac&units=Imperial
     Processing Record  1 of set 8 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mount gambier&units=Imperial
     Processing Record  2 of set 8 | rocha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rocha&units=Imperial
     Processing Record  3 of set 8 | warrnambool
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=warrnambool&units=Imperial
     Processing Record  4 of set 8 | piopio
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=piopio&units=Imperial
     Processing Record  5 of set 8 | lakes entrance
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lakes entrance&units=Imperial
     Processing Record  6 of set 8 | kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kulhudhuffushi&units=Imperial
     Processing Record  7 of set 8 | buenos aires
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=buenos aires&units=Imperial
     Processing Record  8 of set 8 | eyl
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=eyl&units=Imperial
     Processing Record  9 of set 8 | ciudad bolivar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ciudad bolivar&units=Imperial
     Processing Record  10 of set 8 | bac lieu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bac lieu&units=Imperial
    Skipping as City not found.....
     Processing Record  11 of set 8 | totness
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=totness&units=Imperial
     Processing Record  12 of set 8 | sabang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sabang&units=Imperial
     Processing Record  13 of set 8 | banyo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=banyo&units=Imperial
     Processing Record  14 of set 8 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san patricio&units=Imperial
     Processing Record  15 of set 8 | acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=acapulco&units=Imperial
    Skipping as City not found.....
     Processing Record  16 of set 8 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cabo san lucas&units=Imperial
     Processing Record  17 of set 8 | tafo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tafo&units=Imperial
     Processing Record  18 of set 8 | gadung
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gadung&units=Imperial
     Processing Record  19 of set 8 | ifo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ifo&units=Imperial
     Processing Record  20 of set 8 | otukpo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=otukpo&units=Imperial
    Skipping as City not found.....
     Processing Record  21 of set 8 | simbahan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=simbahan&units=Imperial
     Processing Record  22 of set 8 | upata
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=upata&units=Imperial
     Processing Record  23 of set 8 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=makakilo city&units=Imperial
     Processing Record  24 of set 8 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sao filipe&units=Imperial
     Processing Record  25 of set 8 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kavieng&units=Imperial
     Processing Record  26 of set 8 | ratnapura
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ratnapura&units=Imperial
     Processing Record  27 of set 8 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bandarbeyla&units=Imperial
     Processing Record  28 of set 8 | obo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=obo&units=Imperial
     Processing Record  29 of set 8 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kalmunai&units=Imperial
     Processing Record  30 of set 8 | mabaruma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mabaruma&units=Imperial
     Processing Record  31 of set 8 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kloulklubed&units=Imperial
     Processing Record  32 of set 8 | farias brito
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=farias brito&units=Imperial
     Processing Record  33 of set 8 | ipixuna
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ipixuna&units=Imperial
    Skipping as City not found.....
     Processing Record  34 of set 8 | alagoa nova
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=alagoa nova&units=Imperial
     Processing Record  35 of set 8 | floriano
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=floriano&units=Imperial
     Processing Record  36 of set 8 | maneromango
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maneromango&units=Imperial
     Processing Record  37 of set 8 | atambua
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=atambua&units=Imperial
     Processing Record  38 of set 8 | picos
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=picos&units=Imperial
     Processing Record  39 of set 8 | inyonga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=inyonga&units=Imperial
     Processing Record  40 of set 8 | humaita
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=humaita&units=Imperial
     Processing Record  41 of set 8 | cibeureum
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cibeureum&units=Imperial
     Processing Record  42 of set 8 | manicore
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=manicore&units=Imperial
     Processing Record  43 of set 8 | dibaya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dibaya&units=Imperial
    Skipping as City not found.....
     Processing Record  44 of set 8 | kimbe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kimbe&units=Imperial
     Processing Record  45 of set 8 | vikindu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vikindu&units=Imperial
     Processing Record  46 of set 8 | soyo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=soyo&units=Imperial
    Skipping as City not found.....
     Processing Record  47 of set 8 | pringsewu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pringsewu&units=Imperial
     Processing Record  48 of set 8 | maumere
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maumere&units=Imperial
     Processing Record  49 of set 8 | sao felix do xingu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sao felix do xingu&units=Imperial
     Processing Record  0 of set 9 | campos sales
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=campos sales&units=Imperial
     Processing Record  1 of set 9 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=east london&units=Imperial
     Processing Record  2 of set 9 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=iqaluit&units=Imperial
     Processing Record  3 of set 9 | chagda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chagda&units=Imperial
    Skipping as City not found.....
     Processing Record  4 of set 9 | kichmengskiy gorodok
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kichmengskiy gorodok&units=Imperial
     Processing Record  5 of set 9 | uvat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=uvat&units=Imperial
     Processing Record  6 of set 9 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hofn&units=Imperial
     Processing Record  7 of set 9 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fort nelson&units=Imperial
     Processing Record  8 of set 9 | syamzha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=syamzha&units=Imperial
     Processing Record  9 of set 9 | salym
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=salym&units=Imperial
     Processing Record  10 of set 9 | lokosovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lokosovo&units=Imperial
     Processing Record  11 of set 9 | grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=grindavik&units=Imperial
     Processing Record  12 of set 9 | lensk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lensk&units=Imperial
     Processing Record  13 of set 9 | hay river
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hay river&units=Imperial
     Processing Record  14 of set 9 | tigil
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tigil&units=Imperial
     Processing Record  15 of set 9 | dukat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dukat&units=Imperial
     Processing Record  16 of set 9 | lerwick
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lerwick&units=Imperial
     Processing Record  17 of set 9 | hanko
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hanko&units=Imperial
     Processing Record  18 of set 9 | grand centre
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=grand centre&units=Imperial
    Skipping as City not found.....
     Processing Record  19 of set 9 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nome&units=Imperial
     Processing Record  20 of set 9 | uptar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=uptar&units=Imperial
     Processing Record  21 of set 9 | strezhevoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=strezhevoy&units=Imperial
     Processing Record  22 of set 9 | haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=haines junction&units=Imperial
     Processing Record  23 of set 9 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=olafsvik&units=Imperial
    Skipping as City not found.....
     Processing Record  24 of set 9 | la ronge
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=la ronge&units=Imperial
     Processing Record  25 of set 9 | aseri
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aseri&units=Imperial
     Processing Record  26 of set 9 | juneau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=juneau&units=Imperial
     Processing Record  27 of set 9 | begunitsy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=begunitsy&units=Imperial
     Processing Record  28 of set 9 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=attawapiskat&units=Imperial
    Skipping as City not found.....
     Processing Record  29 of set 9 | togur
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=togur&units=Imperial
     Processing Record  30 of set 9 | lesnoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lesnoy&units=Imperial
     Processing Record  31 of set 9 | talaya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=talaya&units=Imperial
     Processing Record  32 of set 9 | gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gornopravdinsk&units=Imperial
     Processing Record  33 of set 9 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=qaqortoq&units=Imperial
     Processing Record  34 of set 9 | ust-maya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ust-maya&units=Imperial
     Processing Record  35 of set 9 | tommot
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tommot&units=Imperial
     Processing Record  36 of set 9 | amot
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=amot&units=Imperial
     Processing Record  37 of set 9 | belyy yar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=belyy yar&units=Imperial
     Processing Record  38 of set 9 | wanning
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wanning&units=Imperial
     Processing Record  39 of set 9 | jejuri
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jejuri&units=Imperial
     Processing Record  40 of set 9 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ponta do sol&units=Imperial
     Processing Record  41 of set 9 | higuey
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=higuey&units=Imperial
    Skipping as City not found.....
     Processing Record  42 of set 9 | nicolas bravo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nicolas bravo&units=Imperial
     Processing Record  43 of set 9 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bilma&units=Imperial
     Processing Record  44 of set 9 | warangal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=warangal&units=Imperial
     Processing Record  45 of set 9 | sur
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sur&units=Imperial
     Processing Record  46 of set 9 | tawkar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tawkar&units=Imperial
    Skipping as City not found.....
     Processing Record  47 of set 9 | letpadan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=letpadan&units=Imperial
    Skipping as City not found.....
     Processing Record  48 of set 9 | escarcega
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=escarcega&units=Imperial
     Processing Record  49 of set 9 | the valley
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=the valley&units=Imperial
     Processing Record  0 of set 10 | kutum
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kutum&units=Imperial
     Processing Record  1 of set 10 | payo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=payo&units=Imperial
    Skipping as City not found.....
     Processing Record  2 of set 10 | pa sang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pa sang&units=Imperial
     Processing Record  3 of set 10 | nishihara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nishihara&units=Imperial
     Processing Record  4 of set 10 | shirwal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shirwal&units=Imperial
     Processing Record  5 of set 10 | jumla
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jumla&units=Imperial
     Processing Record  6 of set 10 | yafran
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yafran&units=Imperial
     Processing Record  7 of set 10 | daxian
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=daxian&units=Imperial
    Skipping as City not found.....
     Processing Record  8 of set 10 | tezu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tezu&units=Imperial
     Processing Record  9 of set 10 | shahr-e kord
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shahr-e kord&units=Imperial
     Processing Record  10 of set 10 | san angelo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san angelo&units=Imperial
     Processing Record  11 of set 10 | ankang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ankang&units=Imperial
     Processing Record  12 of set 10 | rosarito
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rosarito&units=Imperial
     Processing Record  13 of set 10 | semirom
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=semirom&units=Imperial
     Processing Record  14 of set 10 | sonoita
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sonoita&units=Imperial
    Skipping as City not found.....
     Processing Record  15 of set 10 | turayf
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=turayf&units=Imperial
     Processing Record  16 of set 10 | surt
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=surt&units=Imperial
     Processing Record  17 of set 10 | brownwood
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=brownwood&units=Imperial
     Processing Record  18 of set 10 | port hueneme
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port hueneme&units=Imperial
     Processing Record  19 of set 10 | lasa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lasa&units=Imperial
     Processing Record  20 of set 10 | natchez
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=natchez&units=Imperial
     Processing Record  21 of set 10 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ponta delgada&units=Imperial
     Processing Record  22 of set 10 | mehran
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mehran&units=Imperial
     Processing Record  23 of set 10 | bani walid
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bani walid&units=Imperial
     Processing Record  24 of set 10 | tanabe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tanabe&units=Imperial
     Processing Record  25 of set 10 | chaman
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chaman&units=Imperial
     Processing Record  26 of set 10 | bolshaya atnya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bolshaya atnya&units=Imperial
    Skipping as City not found.....
     Processing Record  27 of set 10 | krasnoyarsk-66
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=krasnoyarsk-66&units=Imperial
    Skipping as City not found.....
     Processing Record  28 of set 10 | arman
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arman&units=Imperial
     Processing Record  29 of set 10 | sarana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sarana&units=Imperial
     Processing Record  30 of set 10 | khani
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=khani&units=Imperial
    Skipping as City not found.....
     Processing Record  31 of set 10 | kazachinskoye
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kazachinskoye&units=Imperial
     Processing Record  32 of set 10 | vestbygda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vestbygda&units=Imperial
    Skipping as City not found.....
     Processing Record  33 of set 10 | tinskoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tinskoy&units=Imperial
     Processing Record  34 of set 10 | auce
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=auce&units=Imperial
     Processing Record  35 of set 10 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint anthony&units=Imperial
     Processing Record  36 of set 10 | birzai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=birzai&units=Imperial
     Processing Record  37 of set 10 | fort saint john
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fort saint john&units=Imperial
    Skipping as City not found.....
     Processing Record  38 of set 10 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vestmannaeyjar&units=Imperial
     Processing Record  39 of set 10 | chapais
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chapais&units=Imperial
     Processing Record  40 of set 10 | shestakovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shestakovo&units=Imperial
     Processing Record  41 of set 10 | zolotinka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zolotinka&units=Imperial
    Skipping as City not found.....
     Processing Record  42 of set 10 | port-cartier
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port-cartier&units=Imperial
     Processing Record  43 of set 10 | gavrilov posad
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gavrilov posad&units=Imperial
     Processing Record  44 of set 10 | vigrestad
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vigrestad&units=Imperial
     Processing Record  45 of set 10 | liseleje
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=liseleje&units=Imperial
     Processing Record  46 of set 10 | rzhev
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rzhev&units=Imperial
     Processing Record  47 of set 10 | mochalishche
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mochalishche&units=Imperial
     Processing Record  48 of set 10 | sukhoverkovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sukhoverkovo&units=Imperial
    Skipping as City not found.....
     Processing Record  49 of set 10 | kansk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kansk&units=Imperial
     Processing Record  0 of set 11 | kozlovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kozlovo&units=Imperial
     Processing Record  1 of set 11 | novosokolniki
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=novosokolniki&units=Imperial
     Processing Record  2 of set 11 | chara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chara&units=Imperial
     Processing Record  3 of set 11 | palmer
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=palmer&units=Imperial
     Processing Record  4 of set 11 | ayan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ayan&units=Imperial
     Processing Record  5 of set 11 | kataysk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kataysk&units=Imperial
     Processing Record  6 of set 11 | kolosovka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kolosovka&units=Imperial
     Processing Record  7 of set 11 | vidim
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vidim&units=Imperial
     Processing Record  8 of set 11 | flin flon
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=flin flon&units=Imperial
     Processing Record  9 of set 11 | yaya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yaya&units=Imperial
     Processing Record  10 of set 11 | bonnyville
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bonnyville&units=Imperial
     Processing Record  11 of set 11 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kaitangata&units=Imperial
    Skipping as City not found.....
     Processing Record  12 of set 11 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chuy&units=Imperial
     Processing Record  13 of set 11 | goryachegorsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=goryachegorsk&units=Imperial
    Skipping as City not found.....
     Processing Record  14 of set 11 | ronne
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ronne&units=Imperial
     Processing Record  15 of set 11 | okha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=okha&units=Imperial
     Processing Record  16 of set 11 | moletai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=moletai&units=Imperial
     Processing Record  17 of set 11 | chernenko
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chernenko&units=Imperial
     Processing Record  18 of set 11 | tuma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tuma&units=Imperial
     Processing Record  19 of set 11 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint-augustin&units=Imperial
     Processing Record  20 of set 11 | malino
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=malino&units=Imperial
     Processing Record  21 of set 11 | tynda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tynda&units=Imperial
     Processing Record  22 of set 11 | meadow lake
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=meadow lake&units=Imperial
     Processing Record  23 of set 11 | satka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=satka&units=Imperial
     Processing Record  24 of set 11 | sioux lookout
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sioux lookout&units=Imperial
     Processing Record  25 of set 11 | harlingen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=harlingen&units=Imperial
     Processing Record  26 of set 11 | mnogovershinnyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mnogovershinnyy&units=Imperial
     Processing Record  27 of set 11 | takhtamygda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=takhtamygda&units=Imperial
     Processing Record  28 of set 11 | chumikan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chumikan&units=Imperial
     Processing Record  29 of set 11 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sobolevo&units=Imperial
    Skipping as City not found.....
     Processing Record  30 of set 11 | wladyslawowo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wladyslawowo&units=Imperial
     Processing Record  31 of set 11 | zhigalovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zhigalovo&units=Imperial
     Processing Record  32 of set 11 | mackenzie
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mackenzie&units=Imperial
     Processing Record  33 of set 11 | berdyaush
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=berdyaush&units=Imperial
     Processing Record  34 of set 11 | tatarsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tatarsk&units=Imperial
     Processing Record  35 of set 11 | sept-iles
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sept-iles&units=Imperial
     Processing Record  36 of set 11 | fort saint james
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fort saint james&units=Imperial
    Skipping as City not found.....
     Processing Record  37 of set 11 | iznoski
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=iznoski&units=Imperial
     Processing Record  38 of set 11 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=deputatskiy&units=Imperial
     Processing Record  39 of set 11 | mezen
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mezen&units=Imperial
     Processing Record  40 of set 11 | college
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=college&units=Imperial
     Processing Record  41 of set 11 | aykhal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aykhal&units=Imperial
     Processing Record  42 of set 11 | qasigiannguit
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=qasigiannguit&units=Imperial
     Processing Record  43 of set 11 | rognan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rognan&units=Imperial
     Processing Record  44 of set 11 | nyurba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nyurba&units=Imperial
     Processing Record  45 of set 11 | skagastrond
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=skagastrond&units=Imperial
    Skipping as City not found.....
     Processing Record  46 of set 11 | leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=leshukonskoye&units=Imperial
     Processing Record  47 of set 11 | udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=udachnyy&units=Imperial
     Processing Record  48 of set 11 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=egvekinot&units=Imperial
     Processing Record  49 of set 11 | baykit
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=baykit&units=Imperial
     Processing Record  0 of set 12 | igarka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=igarka&units=Imperial
     Processing Record  1 of set 12 | tura
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tura&units=Imperial
     Processing Record  2 of set 12 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sisimiut&units=Imperial
     Processing Record  3 of set 12 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lavrentiya&units=Imperial
     Processing Record  4 of set 12 | inuvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=inuvik&units=Imperial
     Processing Record  5 of set 12 | naryan-mar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=naryan-mar&units=Imperial
     Processing Record  6 of set 12 | dalvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dalvik&units=Imperial
     Processing Record  7 of set 12 | verkhoyansk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=verkhoyansk&units=Imperial
     Processing Record  8 of set 12 | kuusamo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kuusamo&units=Imperial
     Processing Record  9 of set 12 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aklavik&units=Imperial
     Processing Record  10 of set 12 | oksfjord
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oksfjord&units=Imperial
     Processing Record  11 of set 12 | tromso
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tromso&units=Imperial
     Processing Record  12 of set 12 | karaul
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karaul&units=Imperial
    Skipping as City not found.....
     Processing Record  13 of set 12 | sovetskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sovetskiy&units=Imperial
     Processing Record  14 of set 12 | sorland
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sorland&units=Imperial
     Processing Record  15 of set 12 | andenes
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=andenes&units=Imperial
     Processing Record  16 of set 12 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=klaksvik&units=Imperial
     Processing Record  17 of set 12 | kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kangaatsiaq&units=Imperial
     Processing Record  18 of set 12 | kamenka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kamenka&units=Imperial
     Processing Record  19 of set 12 | skjervoy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=skjervoy&units=Imperial
     Processing Record  20 of set 12 | gravdal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gravdal&units=Imperial
     Processing Record  21 of set 12 | sistranda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sistranda&units=Imperial
     Processing Record  22 of set 12 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=belaya gora&units=Imperial
     Processing Record  23 of set 12 | kharp
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kharp&units=Imperial
     Processing Record  24 of set 12 | guantanamo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=guantanamo&units=Imperial
     Processing Record  25 of set 12 | lahaina
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lahaina&units=Imperial
     Processing Record  26 of set 12 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=guerrero negro&units=Imperial
     Processing Record  27 of set 12 | dwarka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dwarka&units=Imperial
     Processing Record  28 of set 12 | frontera
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=frontera&units=Imperial
     Processing Record  29 of set 12 | chiang rai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chiang rai&units=Imperial
     Processing Record  30 of set 12 | santa maria
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa maria&units=Imperial
     Processing Record  31 of set 12 | kodinar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kodinar&units=Imperial
     Processing Record  32 of set 12 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nouadhibou&units=Imperial
     Processing Record  33 of set 12 | tomatlan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tomatlan&units=Imperial
     Processing Record  34 of set 12 | ibra
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ibra&units=Imperial
     Processing Record  35 of set 12 | tateyama
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tateyama&units=Imperial
     Processing Record  36 of set 12 | guisa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=guisa&units=Imperial
     Processing Record  37 of set 12 | constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=constitucion&units=Imperial
     Processing Record  38 of set 12 | mundo nuevo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mundo nuevo&units=Imperial
     Processing Record  39 of set 12 | san quintin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san quintin&units=Imperial
     Processing Record  40 of set 12 | atar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=atar&units=Imperial
     Processing Record  41 of set 12 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tessalit&units=Imperial
     Processing Record  42 of set 12 | cozumel
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cozumel&units=Imperial
    Skipping as City not found.....
     Processing Record  43 of set 12 | seybaplaya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=seybaplaya&units=Imperial
     Processing Record  44 of set 12 | luang prabang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=luang prabang&units=Imperial
    Skipping as City not found.....
     Processing Record  45 of set 12 | loikaw
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=loikaw&units=Imperial
     Processing Record  46 of set 12 | phayao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=phayao&units=Imperial
     Processing Record  47 of set 12 | bodden town
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bodden town&units=Imperial
     Processing Record  48 of set 12 | rayagada
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rayagada&units=Imperial
    Skipping as City not found.....
     Processing Record  49 of set 12 | samana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=samana&units=Imperial
    Skipping as City not found.....
     Processing Record  0 of set 13 | gonaives
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gonaives&units=Imperial
     Processing Record  1 of set 13 | mae hong son
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mae hong son&units=Imperial
     Processing Record  2 of set 13 | tepalcatepec
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tepalcatepec&units=Imperial
     Processing Record  3 of set 13 | lingao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lingao&units=Imperial
    Skipping as City not found.....
     Processing Record  4 of set 13 | benito juarez
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=benito juarez&units=Imperial
     Processing Record  5 of set 13 | nara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nara&units=Imperial
    Skipping as City not found.....
     Processing Record  6 of set 13 | arecibo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=arecibo&units=Imperial
     Processing Record  7 of set 13 | mangrol
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mangrol&units=Imperial
     Processing Record  8 of set 13 | ustye
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ustye&units=Imperial
     Processing Record  9 of set 13 | maniitsoq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maniitsoq&units=Imperial
     Processing Record  10 of set 13 | whitehorse
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=whitehorse&units=Imperial
     Processing Record  11 of set 13 | megion
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=megion&units=Imperial
     Processing Record  12 of set 13 | teguldet
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=teguldet&units=Imperial
     Processing Record  13 of set 13 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tilichiki&units=Imperial
     Processing Record  14 of set 13 | tarnogskiy gorodok
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tarnogskiy gorodok&units=Imperial
     Processing Record  15 of set 13 | oparino
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=oparino&units=Imperial
     Processing Record  16 of set 13 | atka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=atka&units=Imperial
    Skipping as City not found.....
     Processing Record  17 of set 13 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=okhotsk&units=Imperial
     Processing Record  18 of set 13 | vanavara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vanavara&units=Imperial
     Processing Record  19 of set 13 | sozimskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sozimskiy&units=Imperial
     Processing Record  20 of set 13 | solsvik
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=solsvik&units=Imperial
    Skipping as City not found.....
     Processing Record  21 of set 13 | nizhniy kuranakh
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nizhniy kuranakh&units=Imperial
     Processing Record  22 of set 13 | uppsala
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=uppsala&units=Imperial
     Processing Record  23 of set 13 | paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=paamiut&units=Imperial
     Processing Record  24 of set 13 | homer
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=homer&units=Imperial
     Processing Record  25 of set 13 | solnechnyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=solnechnyy&units=Imperial
     Processing Record  26 of set 13 | luban
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=luban&units=Imperial
     Processing Record  27 of set 13 | malinovoye ozero
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=malinovoye ozero&units=Imperial
     Processing Record  28 of set 13 | katyuzhanka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=katyuzhanka&units=Imperial
     Processing Record  29 of set 13 | vostok
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vostok&units=Imperial
     Processing Record  30 of set 13 | charyshskoye
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=charyshskoye&units=Imperial
     Processing Record  31 of set 13 | glushkovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=glushkovo&units=Imperial
     Processing Record  32 of set 13 | rosetown
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rosetown&units=Imperial
     Processing Record  33 of set 13 | bonavista
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bonavista&units=Imperial
     Processing Record  34 of set 13 | talovaya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=talovaya&units=Imperial
     Processing Record  35 of set 13 | talakan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=talakan&units=Imperial
     Processing Record  36 of set 13 | kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kungurtug&units=Imperial
     Processing Record  37 of set 13 | zachagansk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zachagansk&units=Imperial
    Skipping as City not found.....
     Processing Record  38 of set 13 | sudzha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sudzha&units=Imperial
     Processing Record  39 of set 13 | dauphin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dauphin&units=Imperial
     Processing Record  40 of set 13 | katangli
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=katangli&units=Imperial
    Skipping as City not found.....
     Processing Record  41 of set 13 | olesnica
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=olesnica&units=Imperial
     Processing Record  42 of set 13 | hearst
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hearst&units=Imperial
     Processing Record  43 of set 13 | bayangol
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bayangol&units=Imperial
     Processing Record  44 of set 13 | kyzyl-mazhalyk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kyzyl-mazhalyk&units=Imperial
     Processing Record  45 of set 13 | neepawa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=neepawa&units=Imperial
     Processing Record  46 of set 13 | kalga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kalga&units=Imperial
     Processing Record  47 of set 13 | pinawa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pinawa&units=Imperial
     Processing Record  48 of set 13 | samoylovka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=samoylovka&units=Imperial
     Processing Record  49 of set 13 | ashford
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ashford&units=Imperial
     Processing Record  0 of set 14 | longlac
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=longlac&units=Imperial
    Skipping as City not found.....
     Processing Record  1 of set 14 | bethanien
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bethanien&units=Imperial
    Skipping as City not found.....
     Processing Record  2 of set 14 | bongaree
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bongaree&units=Imperial
     Processing Record  3 of set 14 | santiago del estero
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santiago del estero&units=Imperial
     Processing Record  4 of set 14 | broken hill
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=broken hill&units=Imperial
     Processing Record  5 of set 14 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint-joseph&units=Imperial
     Processing Record  6 of set 14 | presidencia roque saenz pena
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=presidencia roque saenz pena&units=Imperial
     Processing Record  7 of set 14 | inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=inhambane&units=Imperial
     Processing Record  8 of set 14 | orkney
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=orkney&units=Imperial
     Processing Record  9 of set 14 | ampanihy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ampanihy&units=Imperial
     Processing Record  10 of set 14 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=grand river south east&units=Imperial
    Skipping as City not found.....
     Processing Record  11 of set 14 | santa cecilia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa cecilia&units=Imperial
     Processing Record  12 of set 14 | gold coast
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gold coast&units=Imperial
     Processing Record  13 of set 14 | bokspits
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bokspits&units=Imperial
    Skipping as City not found.....
     Processing Record  14 of set 14 | russell
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=russell&units=Imperial
     Processing Record  15 of set 14 | roma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=roma&units=Imperial
     Processing Record  16 of set 14 | tucuman
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tucuman&units=Imperial
    Skipping as City not found.....
     Processing Record  17 of set 14 | uri
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=uri&units=Imperial
     Processing Record  18 of set 14 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pacific grove&units=Imperial
     Processing Record  19 of set 14 | huilong
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=huilong&units=Imperial
     Processing Record  20 of set 14 | abu kamal
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=abu kamal&units=Imperial
    Skipping as City not found.....
     Processing Record  21 of set 14 | lake havasu city
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lake havasu city&units=Imperial
     Processing Record  22 of set 14 | nador
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nador&units=Imperial
     Processing Record  23 of set 14 | havelock
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=havelock&units=Imperial
     Processing Record  24 of set 14 | birin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=birin&units=Imperial
    Skipping as City not found.....
     Processing Record  25 of set 14 | linxia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=linxia&units=Imperial
     Processing Record  26 of set 14 | tral
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tral&units=Imperial
     Processing Record  27 of set 14 | misratah
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=misratah&units=Imperial
     Processing Record  28 of set 14 | lardos
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=lardos&units=Imperial
     Processing Record  29 of set 14 | jubayl
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jubayl&units=Imperial
    Skipping as City not found.....
     Processing Record  30 of set 14 | hope
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=hope&units=Imperial
     Processing Record  31 of set 14 | wilmington
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wilmington&units=Imperial
     Processing Record  32 of set 14 | semnan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=semnan&units=Imperial
     Processing Record  33 of set 14 | portales
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=portales&units=Imperial
     Processing Record  34 of set 14 | roswell
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=roswell&units=Imperial
     Processing Record  35 of set 14 | asfi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=asfi&units=Imperial
    Skipping as City not found.....
     Processing Record  36 of set 14 | juegang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=juegang&units=Imperial
     Processing Record  37 of set 14 | pombia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pombia&units=Imperial
     Processing Record  38 of set 14 | darnah
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=darnah&units=Imperial
     Processing Record  39 of set 14 | sun city west
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sun city west&units=Imperial
     Processing Record  40 of set 14 | tawzar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tawzar&units=Imperial
    Skipping as City not found.....
     Processing Record  41 of set 14 | harsin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=harsin&units=Imperial
     Processing Record  42 of set 14 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=virginia beach&units=Imperial
     Processing Record  43 of set 14 | anchorage
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=anchorage&units=Imperial
     Processing Record  44 of set 14 | severnyy-kospashskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=severnyy-kospashskiy&units=Imperial
     Processing Record  45 of set 14 | turtas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=turtas&units=Imperial
     Processing Record  46 of set 14 | palana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=palana&units=Imperial
     Processing Record  47 of set 14 | pavda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pavda&units=Imperial
    Skipping as City not found.....
     Processing Record  48 of set 14 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=beringovskiy&units=Imperial
     Processing Record  49 of set 14 | high level
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=high level&units=Imperial
     Processing Record  0 of set 15 | kargasok
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kargasok&units=Imperial
     Processing Record  1 of set 15 | sandwick
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sandwick&units=Imperial
     Processing Record  2 of set 15 | kropotkin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kropotkin&units=Imperial
     Processing Record  3 of set 15 | yurla
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yurla&units=Imperial
     Processing Record  4 of set 15 | yayva
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yayva&units=Imperial
     Processing Record  5 of set 15 | kardla
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kardla&units=Imperial
     Processing Record  6 of set 15 | kologriv
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kologriv&units=Imperial
     Processing Record  7 of set 15 | nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nuuk&units=Imperial
     Processing Record  8 of set 15 | olgina
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=olgina&units=Imperial
     Processing Record  9 of set 15 | volosovo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=volosovo&units=Imperial
     Processing Record  10 of set 15 | vokhma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vokhma&units=Imperial
     Processing Record  11 of set 15 | kolpashevo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kolpashevo&units=Imperial
     Processing Record  12 of set 15 | boksitogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=boksitogorsk&units=Imperial
     Processing Record  13 of set 15 | paramonga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=paramonga&units=Imperial
     Processing Record  14 of set 15 | tunduru
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tunduru&units=Imperial
    Skipping as City not found.....
     Processing Record  15 of set 15 | kirakira
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kirakira&units=Imperial
     Processing Record  16 of set 15 | xapuri
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=xapuri&units=Imperial
     Processing Record  17 of set 15 | morehead
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=morehead&units=Imperial
     Processing Record  18 of set 15 | benguela
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=benguela&units=Imperial
     Processing Record  19 of set 15 | gizo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gizo&units=Imperial
     Processing Record  20 of set 15 | maragogi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maragogi&units=Imperial
     Processing Record  21 of set 15 | barra
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barra&units=Imperial
     Processing Record  22 of set 15 | cazaje
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cazaje&units=Imperial
    Skipping as City not found.....
     Processing Record  23 of set 15 | camacupa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=camacupa&units=Imperial
     Processing Record  24 of set 15 | madimba
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=madimba&units=Imperial
     Processing Record  25 of set 15 | soe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=soe&units=Imperial
     Processing Record  26 of set 15 | kanigoro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kanigoro&units=Imperial
     Processing Record  27 of set 15 | pangoa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pangoa&units=Imperial
     Processing Record  28 of set 15 | chicama
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=chicama&units=Imperial
     Processing Record  29 of set 15 | mutsamudu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mutsamudu&units=Imperial
    Skipping as City not found.....
     Processing Record  30 of set 15 | honiara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=honiara&units=Imperial
     Processing Record  31 of set 15 | formoso do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=formoso do araguaia&units=Imperial
    Skipping as City not found.....
     Processing Record  32 of set 15 | olinda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=olinda&units=Imperial
     Processing Record  33 of set 15 | luau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=luau&units=Imperial
     Processing Record  34 of set 15 | merauke
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=merauke&units=Imperial
     Processing Record  35 of set 15 | gondanglegi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=gondanglegi&units=Imperial
    Skipping as City not found.....
     Processing Record  36 of set 15 | tamandare
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tamandare&units=Imperial
     Processing Record  37 of set 15 | black river
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=black river&units=Imperial
     Processing Record  38 of set 15 | pathein
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pathein&units=Imperial
     Processing Record  39 of set 15 | dakar
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dakar&units=Imperial
     Processing Record  40 of set 15 | vieques
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vieques&units=Imperial
     Processing Record  41 of set 15 | zacualpan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=zacualpan&units=Imperial
     Processing Record  42 of set 15 | ribeira brava
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ribeira brava&units=Imperial
     Processing Record  43 of set 15 | ginda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ginda&units=Imperial
    Skipping as City not found.....
     Processing Record  44 of set 15 | guntur
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=guntur&units=Imperial
     Processing Record  45 of set 15 | jamiltepec
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jamiltepec&units=Imperial
    Skipping as City not found.....
     Processing Record  46 of set 15 | ixtapa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ixtapa&units=Imperial
     Processing Record  47 of set 15 | maghama
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maghama&units=Imperial
    Skipping as City not found.....
     Processing Record  48 of set 15 | ewa beach
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ewa beach&units=Imperial
     Processing Record  49 of set 15 | maunabo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maunabo&units=Imperial
     Processing Record  0 of set 16 | mao
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mao&units=Imperial
     Processing Record  1 of set 16 | labutta
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=labutta&units=Imperial
    Skipping as City not found.....
     Processing Record  2 of set 16 | charlestown
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=charlestown&units=Imperial
     Processing Record  3 of set 16 | benemerito de las americas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=benemerito de las americas&units=Imperial
     Processing Record  4 of set 16 | santa isabel
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa isabel&units=Imperial
     Processing Record  5 of set 16 | sal rei
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sal rei&units=Imperial
     Processing Record  6 of set 16 | meyungs
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=meyungs&units=Imperial
    Skipping as City not found.....
     Processing Record  7 of set 16 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=santa cruz&units=Imperial
     Processing Record  8 of set 16 | kafanchan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kafanchan&units=Imperial
    Skipping as City not found.....
     Processing Record  9 of set 16 | vung tau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=vung tau&units=Imperial
     Processing Record  10 of set 16 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kavaratti&units=Imperial
     Processing Record  11 of set 16 | phan rang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=phan rang&units=Imperial
    Skipping as City not found.....
     Processing Record  12 of set 16 | raga
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=raga&units=Imperial
     Processing Record  13 of set 16 | wa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=wa&units=Imperial
    Skipping as City not found.....
     Processing Record  14 of set 16 | carora
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=carora&units=Imperial
     Processing Record  15 of set 16 | batticaloa
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=batticaloa&units=Imperial
     Processing Record  16 of set 16 | maturin
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=maturin&units=Imperial
     Processing Record  17 of set 16 | ko samui
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ko samui&units=Imperial
     Processing Record  18 of set 16 | garowe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=garowe&units=Imperial
    Skipping as City not found.....
     Processing Record  19 of set 16 | mullaitivu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mullaitivu&units=Imperial
    Skipping as City not found.....
     Processing Record  20 of set 16 | goundi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=goundi&units=Imperial
     Processing Record  21 of set 16 | bosconia
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=bosconia&units=Imperial
     Processing Record  22 of set 16 | kagoro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kagoro&units=Imperial
     Processing Record  23 of set 16 | cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=cayenne&units=Imperial
     Processing Record  24 of set 16 | virapandi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=virapandi&units=Imperial
    Skipping as City not found.....
     Processing Record  25 of set 16 | mana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mana&units=Imperial
     Processing Record  26 of set 16 | ciudad ojeda
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ciudad ojeda&units=Imperial
     Processing Record  27 of set 16 | port blair
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port blair&units=Imperial
     Processing Record  28 of set 16 | sholavandan
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sholavandan&units=Imperial
    Skipping as City not found.....
     Processing Record  29 of set 16 | ranong
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ranong&units=Imperial
     Processing Record  30 of set 16 | sorong
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sorong&units=Imperial
     Processing Record  31 of set 16 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=port-gentil&units=Imperial
     Processing Record  32 of set 16 | axim
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=axim&units=Imperial
     Processing Record  33 of set 16 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tabiauea&units=Imperial
    Skipping as City not found.....
     Processing Record  34 of set 16 | nueva loja
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=nueva loja&units=Imperial
     Processing Record  35 of set 16 | manaus
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=manaus&units=Imperial
     Processing Record  36 of set 16 | prainha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=prainha&units=Imperial
     Processing Record  37 of set 16 | quevedo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=quevedo&units=Imperial
     Processing Record  38 of set 16 | manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=manokwari&units=Imperial
     Processing Record  39 of set 16 | pillaro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=pillaro&units=Imperial
     Processing Record  40 of set 16 | barreirinhas
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barreirinhas&units=Imperial
     Processing Record  41 of set 16 | boende
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=boende&units=Imperial
     Processing Record  42 of set 16 | mwingi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mwingi&units=Imperial
     Processing Record  43 of set 16 | tidore
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tidore&units=Imperial
    Skipping as City not found.....
     Processing Record  44 of set 16 | muana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=muana&units=Imperial
     Processing Record  45 of set 16 | barawe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=barawe&units=Imperial
    Skipping as City not found.....
     Processing Record  46 of set 16 | tena
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tena&units=Imperial
     Processing Record  47 of set 16 | fougamou
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=fougamou&units=Imperial
     Processing Record  48 of set 16 | trairi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=trairi&units=Imperial
     Processing Record  49 of set 16 | mimongo
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mimongo&units=Imperial
     Processing Record  0 of set 17 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=komsomolskiy&units=Imperial
     Processing Record  1 of set 17 | roald
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=roald&units=Imperial
     Processing Record  2 of set 17 | honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=honningsvag&units=Imperial
     Processing Record  3 of set 17 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=skalistyy&units=Imperial
    Skipping as City not found.....
     Processing Record  4 of set 17 | rypefjord
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rypefjord&units=Imperial
     Processing Record  5 of set 17 | kovdor
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kovdor&units=Imperial
     Processing Record  6 of set 17 | kautokeino
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kautokeino&units=Imperial
     Processing Record  7 of set 17 | karasjok
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=karasjok&units=Imperial
     Processing Record  8 of set 17 | snezhnogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=snezhnogorsk&units=Imperial
     Processing Record  9 of set 17 | beisfjord
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=beisfjord&units=Imperial
    Skipping as City not found.....
     Processing Record  10 of set 17 | walvis bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=walvis bay&units=Imperial
     Processing Record  11 of set 17 | poya
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=poya&units=Imperial
     Processing Record  12 of set 17 | henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=henties bay&units=Imperial
    Skipping as City not found.....
     Processing Record  13 of set 17 | sakaraha
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=sakaraha&units=Imperial
     Processing Record  14 of set 17 | farafangana
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=farafangana&units=Imperial
     Processing Record  15 of set 17 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=alofi&units=Imperial
     Processing Record  16 of set 17 | rockhampton
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rockhampton&units=Imperial
     Processing Record  17 of set 17 | rehoboth
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rehoboth&units=Imperial
     Processing Record  18 of set 17 | amambai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=amambai&units=Imperial
     Processing Record  19 of set 17 | saint-leu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=saint-leu&units=Imperial
     Processing Record  20 of set 17 | moerai
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=moerai&units=Imperial
     Processing Record  21 of set 17 | manakara
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=manakara&units=Imperial
     Processing Record  22 of set 17 | louis trichardt
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=louis trichardt&units=Imperial
     Processing Record  23 of set 17 | taubate
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=taubate&units=Imperial
     Processing Record  24 of set 17 | xinyang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=xinyang&units=Imperial
     Processing Record  25 of set 17 | birjand
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=birjand&units=Imperial
     Processing Record  26 of set 17 | tianpeng
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tianpeng&units=Imperial
     Processing Record  27 of set 17 | ahuimanu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=ahuimanu&units=Imperial
     Processing Record  28 of set 17 | yol
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=yol&units=Imperial
     Processing Record  29 of set 17 | xihe
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=xihe&units=Imperial
     Processing Record  30 of set 17 | jadu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=jadu&units=Imperial
    Skipping as City not found.....
     Processing Record  31 of set 17 | shingu
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=shingu&units=Imperial
     Processing Record  32 of set 17 | muroto
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=muroto&units=Imperial
     Processing Record  33 of set 17 | andrews
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=andrews&units=Imperial
     Processing Record  34 of set 17 | tubruq
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tubruq&units=Imperial
    Skipping as City not found.....
     Processing Record  35 of set 17 | canico
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=canico&units=Imperial
     Processing Record  36 of set 17 | dehloran
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dehloran&units=Imperial
     Processing Record  37 of set 17 | progreso
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=progreso&units=Imperial
     Processing Record  38 of set 17 | minggang
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=minggang&units=Imperial
     Processing Record  39 of set 17 | kahului
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kahului&units=Imperial
     Processing Record  40 of set 17 | mrirt
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=mrirt&units=Imperial
    Skipping as City not found.....
     Processing Record  41 of set 17 | viedma
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=viedma&units=Imperial
     Processing Record  42 of set 17 | te anau
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=te anau&units=Imperial
     Processing Record  43 of set 17 | san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=san carlos de bariloche&units=Imperial
     Processing Record  44 of set 17 | necochea
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=necochea&units=Imperial
     Processing Record  45 of set 17 | burnie
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=burnie&units=Imperial
     Processing Record  46 of set 17 | launceston
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=launceston&units=Imperial
     Processing Record  47 of set 17 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=castro&units=Imperial
     Processing Record  48 of set 17 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=tuatapere&units=Imperial
     Processing Record  49 of set 17 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=rawson&units=Imperial
     Processing Record  0 of set 18 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=plettenberg bay&units=Imperial
     Processing Record  1 of set 18 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=kruisfontein&units=Imperial
     Processing Record  2 of set 18 | coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=coihaique&units=Imperial
     Processing Record  3 of set 18 | aksarka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=aksarka&units=Imperial
     Processing Record  4 of set 18 | havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=havoysund&units=Imperial
     Processing Record  5 of set 18 | batsfjord
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=batsfjord&units=Imperial
     Processing Record  6 of set 18 | dudinka
    http://api.openweathermap.org/data/2.5/weather?appid=630b81a0e82edb148c41f8f48f46cd29&q=dudinka&units=Imperial
    ......................
    Data Retieval Complete.
    ......................
    


```python
#Creating the dataframe by using city_dat list values
city_dict =[]
for cd in city_data:
    city_dict.append({
                        "City":cd["name"],
                        "Country":cd["sys"]["country"],
                        "Humidity":cd["main"]["humidity"],
                        "Cloudiness":cd["clouds"]["all"],
                        "lon":cd["coord"]["lon"],
                        "lat":cd["coord"]["lat"],
                        "Wind Speed":cd["wind"]["speed"],
                        "MaxTemp":cd["main"]["temp_max"],
        
                        "Date": cd["dt"]

    })
city_df = pd.DataFrame(city_dict);
city_df.count()
```




    City          781
    Cloudiness    781
    Country       781
    Date          781
    Humidity      781
    MaxTemp       781
    Wind Speed    781
    lat           781
    lon           781
    dtype: int64




```python
#Display the City DataFrame
city_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>MaxTemp</th>
      <th>Wind Speed</th>
      <th>lat</th>
      <th>lon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Isangel</td>
      <td>0</td>
      <td>VU</td>
      <td>1505396920</td>
      <td>100</td>
      <td>73.79</td>
      <td>11.12</td>
      <td>-19.55</td>
      <td>169.27</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Namibe</td>
      <td>0</td>
      <td>AO</td>
      <td>1505396921</td>
      <td>100</td>
      <td>67.13</td>
      <td>10.22</td>
      <td>-15.20</td>
      <td>12.15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Acari</td>
      <td>8</td>
      <td>PE</td>
      <td>1505396921</td>
      <td>67</td>
      <td>64.25</td>
      <td>2.84</td>
      <td>-15.43</td>
      <td>-74.62</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Coxim</td>
      <td>0</td>
      <td>BR</td>
      <td>1505396921</td>
      <td>32</td>
      <td>93.77</td>
      <td>13.47</td>
      <td>-18.51</td>
      <td>-54.76</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rikitea</td>
      <td>80</td>
      <td>PF</td>
      <td>1505396922</td>
      <td>100</td>
      <td>69.56</td>
      <td>9.89</td>
      <td>-23.12</td>
      <td>-134.97</td>
    </tr>
  </tbody>
</table>
</div>




```python
date = datetime.datetime.fromtimestamp(city_df["Date"].max()).strftime('%m/%d/%Y')
```

## Lattitude vs. Temperature Plot


```python
plt.scatter(city_df["lat"], 
            city_df["MaxTemp"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8, label="lat")
plt.title("City Lattitude Vs MAx Temperature("+date+")",{'fontsize':16} )
plt.xlabel("Lattitude",{'fontsize':14})
plt.ylabel("MaxTemperature(F)",{'fontsize':14})
plt.grid(True)
sns.set()
plt.xticks(np.arange(-80,101,20))
plt.yticks(np.arange(-100,151,50))
plt.xlim(-80,100)
plt.ylim(-100,150)
plt.show()
```


![png](output_10_0.png)


## Lattitude vs. Humidity Plot


```python
plt.scatter(city_df["lat"], 
            city_df["Humidity"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)
plt.title("City Lattitude Vs Humidity("+date+")",{'fontsize':16})
plt.xlabel("Lattitude",{'fontsize':14})
plt.ylabel("Humidity(%)",{'fontsize':14})
plt.grid(True)
sns.set()
plt.xticks(np.arange(-80,100+1,20))
plt.yticks(np.arange(-20,120+1,20))
plt.xlim(-80,100)
plt.ylim(-20,120)
plt.show()
```


![png](output_12_0.png)


## Lattitude vs. Cloudiness Plot


```python
plt.scatter(city_df["lat"], 
            city_df["Cloudiness"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)
plt.title("City Lattitude Vs MAx Cloudiness("+date+")",{'fontsize':16})
plt.xlabel("Lattitude",{'fontsize':14})
plt.ylabel("Cloudiness(%)",{'fontsize':14})
plt.grid(True)
sns.set()
plt.xticks(np.arange(-80,100+1,20))
plt.yticks(np.arange(-20,120+1,20))
plt.xlim(-80,100)
plt.ylim(-20,120)
plt.show()
```


![png](output_14_0.png)


## Lattitude vs. WindSpeed Plot


```python
plt.scatter(city_df["lat"], 
            city_df["Wind Speed"],
            edgecolor="blue", linewidths=1, marker="o", 
            alpha=0.8)
plt.title("City Lattitude Vs Wind Speed("+date+"01/05/2017)",{'fontsize':16})
plt.xlabel("Lattitude",{'fontsize':14})
plt.ylabel("Wind Speed(mph)",{'fontsize':14})
plt.grid(True)
sns.set()
plt.xticks(np.arange(-80,100+1,20))
plt.yticks(np.arange(-5,40+1,5))
plt.xlim(-80,100)
plt.ylim(-5,40)
plt.show()
```


![png](output_16_0.png)



```python

```
