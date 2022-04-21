# SWE EVAG API (inoffiziell)

API-Schnittstelle, verwendet von der SWE-EVAG App für den Erfurter Nahverkehr.

Inoffizielle Dokumentation, die Schnittstelle selbst ist nicht öffentlich.

### Inhaltsverzeichnis

1. [Aktuelle Nachrichten abrufen](#item1)
2. [Bestimmte Nachricht abrufen](#item2)
3. [Alle Stationen abrufen](#item3)
4. [Stationen suchen (Autocomplete)](#item4)
5. [Nächste Abfahrten](#item5)
6. [Haltestellen auf einer Linie / Journey](#item6)
7. [Verbindung zwischen 2 Stationen / Haltestellen](#item7)



## 1. Aktuelle Nachrichten abrufen <a name="item1"></a>

Ruft aktuelle Nachrichten der SWE-EVAG ab

**URL** : `https://evag-live.wla-backend.de/cms?cat=3&json=1`

**Methode** : `GET`

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b></summary>

```json
{
    "status": "ok",
    "count": 6,
    "pages": 1,
    "category": {
        "id": 3,
        "slug": "news",
        "title": "News",
        "description": "",
        "parent": 0,
        "post_count": 6
    },
    "posts": [
        {
            "id": -1,
            "type": "post",
            "slug": "",
            "url": "",
            "status": "",
            "title": "",
            "title_plain": "",
            "content": "",
            "excerpt": "",
            "date": "",
            "modified": "",
            "categories": [
                {
                    "id": 3,
                    "slug": "news",
                    "title": "News",
                    "description": "",
                    "parent": 0,
                    "post_count": 6
                },
                {
                    "id": 4,
                    "slug": "push",
                    "title": "Push",
                    "description": "",
                    "parent": 0,
                    "post_count": 1
                }
            ],
            "tags": [],
            "author": {
                "id": 1,
                "slug": "admin",
                "name": "admin",
                "first_name": "",
                "last_name": "",
                "nickname": "admin",
                "url": "",
                "description": ""
            },
            "comments": [],
            "attachments": [],
            "comment_count": 0,
            "comment_status": "open",
            "thumbnail": null,
            "custom_fields": {},
            "thumbnail_size": "thumbnail",
            "thumbnail_images": {
                "full": {
                    "url": "",
                    "width": 1100,
                    "height": 1100
                },
                "twentyseventeen-featured-image": {
                    "url": "",
                    "width": 1100,
                    "height": 1100
                }
            }
        }
    ]
}
```
</details>


**Sonstiges**: Über den Parameter `cat` kann der Kanal geändert werden, wobei neben Kanal 3 noch der Kanal 4 relevant ist, welcher für die Push-Benachrichtigungen verwendet wird.

## 2. Bestimmte Nachricht abrufen <a name="item2"></a>

Ruft eine bestimmte Nachricht mithilfe der ID ab

**URL** : `https://evag-live.wla-backend.de/cms?json=1&p=418`

**Methode** : `GET`

**Query-Parameter** : 

`p` (Post-ID)

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b></summary>

```json
{
    "status": "ok",
    "post": {
        "id": 418,
        "type": "post",
        "slug": "",
        "url": "",
        "status": "",
        "title": "",
        "title_plain": "",
        "content": "",
        "excerpt": "",
        "date": "",
        "modified": "",
        "categories": [
            {
                "id": 3,
                "slug": "news",
                "title": "News",
                "description": "",
                "parent": 0,
                "post_count": 6
            },
            {
                "id": 4,
                "slug": "push",
                "title": "Push",
                "description": "",
                "parent": 0,
                "post_count": 1
            }
        ],
        "tags": [],
        "author": {
            "id": 1,
            "slug": "admin",
            "name": "admin",
            "first_name": "",
            "last_name": "",
            "nickname": "admin",
            "url": "",
            "description": ""
        },
        "comments": [],
        "attachments": [],
        "comment_count": 0,
        "comment_status": "open",
        "thumbnail": null,
        "custom_fields": {},
        "thumbnail_size": "thumbnail",
        "thumbnail_images": {
            "full": {
                "url": "",
                "width": 1100,
                "height": 1100
            },
            "twentyseventeen-featured-image": {
                "url": "",
                "width": 1100,
                "height": 1100
            }
        }
    },
    "previous_url": "https://evag-live.wla-backend.de/cms/?p=423"
}
```
</details>
<details>
<summary><b>Fehlerhafte Antwort (Status Code 404), Beispiel</b></summary>

```json
{
    "status": "error",
    "error": "Not found"
}
```
    
</details>


## 3. Alle Stationen abrufen <a name="item3"></a>

Ruft alle Stationen und Haltestellen aus Thüringen und teilweise Sachsen (bspw. Leipzig) ab.

**URL** : `https://evag-live.wla-backend.de/stations/latest.json`

**Methode** : `GET`

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b></summary>

```json
[
    {
        "y": 50.976262,
        "x": 11.034115,
        "name": "Ersatzhaltestelle Anger",
        "parent": "Erfurt",
        "id": "5033"
    },
    {
        "y": 50.402688,
        "x": 11.686957,
        "name": "Absang",
        "parent": "Absang",
        "id": "2865361"
    }
]
```
</details>


**Sonstiges**: Nach aktuellem Stand kann diese Abfrage nicht gefiltert werden und daher werden alle Stationen abgerufen, wodurch ein großer Payload entsteht.

## 4. Stationen suchen (Autocomplete) <a name="item4"></a>

Gibt Stationen und Haltestellen aus, die dem Suchparameter `q` entsprechen.

**URL** : `https://evag-live.wla-backend.de/node/v2/autocomplete`

**Methode** : `POST`

**Body-Parameter** (als raw-json) : 

```json
{
    "q": "[SUCHPARAMETER]"
}
```

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b> (q = Europaplatz)</summary>

```json
[
    {
        "name": "Erfurt, Europaplatz",
        "extId": "151063",
        "id": "A=1@O=Erfurt, Europaplatz@X=10991866@Y=51012983@U=80@L=151063@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 51.012983,
        "lon": 10.991866,
        "weight": 6946
    },
    {
        "name": "Erfurt, Europaplatz",
        "id": "A=2@O=Erfurt, Europaplatz@X=10989924@Y=51010979@U=99@b=980348296@B=1@p=1462862076@",
        "type": "ADR",
        "lat": 51.010979,
        "lon": 10.989924
    },
    {
        "name": "Berlin Hbf (Europaplatz)",
        "extId": "8070952",
        "id": "A=1@O=Berlin Hbf (Europaplatz)@X=13369180@Y=52526210@U=80@L=8070952@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 52.52621,
        "lon": 13.36918
    },
    {
        "name": "Erfurt, Domplatz Süd",
        "extId": "151748",
        "id": "A=1@O=Erfurt, Domplatz Süd@X=11024928@Y=50977045@U=80@L=151748@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.977045,
        "lon": 11.024928,
        "weight": 6774
    },
    {
        "name": "Erfurt, Domplatz Nord",
        "extId": "151192",
        "id": "A=1@O=Erfurt, Domplatz Nord@X=11024479@Y=50977863@U=80@L=151192@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.977863,
        "lon": 11.024479,
        "weight": 6946
    },
    {
        "name": "Erfurt, Gothaer Platz",
        "extId": "151267",
        "id": "A=1@O=Erfurt, Gothaer Platz@X=11012262@Y=50971220@U=80@L=151267@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.97122,
        "lon": 11.012262,
        "weight": 6774
    },
    {
        "name": "Erfurt, P+R-Platz Messe",
        "extId": "151276",
        "id": "A=1@O=Erfurt, P+R-Platz Messe@X=10984306@Y=50958850@U=80@L=151276@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.95885,
        "lon": 10.984306,
        "weight": 6946
    },
    {
        "name": "Erlau, Sportplatz",
        "extId": "2510392",
        "id": "A=1@O=Erlau, Sportplatz@X=10754038@Y=50537679@U=80@L=2510392@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.537679,
        "lon": 10.754038,
        "weight": 171
    },
    {
        "name": "Erfurt, Leipziger Platz",
        "extId": "151213",
        "id": "A=1@O=Erfurt, Leipziger Platz@X=11041100@Y=50981350@U=80@L=151213@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.98135,
        "lon": 11.0411,
        "weight": 4061
    },
    {
        "name": "Erfurt, Steinplatz",
        "extId": "151218",
        "id": "A=1@O=Erfurt, Steinplatz@X=11037935@Y=50984299@U=80@L=151218@B=1@p=1630073948@",
        "type": "STOP",
        "lat": 50.984299,
        "lon": 11.037935,
        "weight": 171
    }
]
```
</details>


**Sonstiges**: Bei manchen Anfragen, bspw: `Euro` ist die Antwort leer, der Grund dafür ist unbekannt.

## 5. Nächste Abfahrten <a name="item5"></a>

Zeigt die nächsten Abfahrten für Bus, Bahn und mehr an, die an einer Station, Haltestelle abfahren.

**URL** : `https://evag-live.wla-backend.de/node/v1/departures/[Station-ID]`

**Methode** : `GET`

**URL-Parameter** : 

`Station-ID` (ID der Station, entnommen aus bspw. Autocomplete (A=1@O=Erfurt, Europaplatz@X=10991866@Y=51012983@U=80@L=151063@B=1@p=1630073948@ -> 151063))

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b> (Station-ID = Europaplatz)</summary>

```json
{
    "departures": [
        {
            "category": "Strab",
            "line": "3",
            "lineOperator": "Erfurter Verkehrsbetriebe AG",
            "targetLocation": "Erfurt, Urbicher Kreuz",
            "type": "Strab",
            "timestamp": 1630255200,
            "timestamp_rt": 1630255200,
            "journey_id": "1|33252|32|80|29082021",
            "cancelled": false
        },
        {
            "category": "Strab",
            "line": "3",
            "lineOperator": "Erfurter Verkehrsbetriebe AG",
            "targetLocation": "Erfurt, Urbicher Kreuz",
            "type": "Strab",
            "timestamp": 1630256100,
            "timestamp_rt": 1630256100,
            "journey_id": "1|33252|33|80|29082021",
            "cancelled": false
        },
        {
            "category": "Bus",
            "line": "10",
            "lineOperator": "Erfurter Verkehrsbetriebe AG",
            "targetLocation": "Erfurt, Tiefthal",
            "type": "Bus",
            "timestamp": 1630256880,
            "timestamp_rt": 1630256880,
            "journey_id": "1|34831|5|80|29082021",
            "cancelled": false,
            "messages": [
                {
                    "category": 2,
                    "priority": 0,
                    "title": "Bauarbeiten in Gispersleben! Linie 10 verkehrt im Umleitungsverkehr. Die Hst. Kamenzer Str. und Akazienallee können vorübergehend nicht bedient werden.***",
                    "text": "Bauarbeiten in Gispersleben! Linie 10 verkehrt im Umleitungsverkehr. Die Hst. Kamenzer Str. und Akazienallee können vorübergehend nicht bedient werden.***",
                    "start_timestamp": 1629082800,
                    "end_timestamp": 1634936400
                }
            ]
        }
    ]
}
```
</details>


**Sonstiges**: 

Bei manchen Elementen wird noch `messages` angehängt, welche Nachrichten, wie bspw. ausfallende Haltestellen und mehr anzeigen kann. 

Sollte eine `Station-ID` ungültig sein, so ist das Element `departures` leer. Der Status-Code ist weiterhin `200`.

## 6. Haltestellen auf einer Linie / Journey <a name="item6"></a>

Zeigt die Stationen und Haltestellen einer Journey an.

**URL** : `https://evag-live.wla-backend.de/node/v1/nextStops/[Journey-ID]`

**Methode** : `GET`

**URL-Parameter** : 

`Journey-ID` (ID der Journey, entnommen aus bspw. Autocomplete über den Parameter `journey_id` (1|33252|34|80|29082021))

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b> (Journey-ID = 1|33252|34|80|29082021)</summary>

```json
{
    "result": {
        "stops": [
            {
                "stationName": "Europaplatz",
                "departure_timestamp": 1630257000,
                "departure_timestamp_rt": 1630257000,
                "departure_delay": 0,
                "cancelled": false,
                "id": "A=1@O=Erfurt, Europaplatz@X=10991893@Y=51013199@U=80@L=15106304@",
                "vmtId": "15106304",
                "coordinates": {
                    "lat": 51.013199,
                    "lon": 10.991893
                }
            },
            {
                "stationName": "Thüringen-Park",
                "arrival_timestamp": 1630257060,
                "arrival_timestamp_rt": 1630257060,
                "arrival_delay": 0,
                "departure_timestamp": 1630257060,
                "departure_timestamp_rt": 1630257060,
                "departure_delay": 0,
                "cancelled": false,
                "id": "A=1@O=Erfurt, Thüringen-Park@X=10994545@Y=51009055@U=80@L=15125600@",
                "vmtId": "15125600",
                "coordinates": {
                    "lat": 51.009055,
                    "lon": 10.994545
                }
            }
        ],
        "polyline": [
            {
                "lat": 51.013199,
                "lon": 10.991911
            },
            {
                "lat": 51.01248,
                "lon": 10.991668
            }
        ],
        "messages": []
    }
}
```
</details>


**Sonstiges**: 

Die Abfrage macht eine eigene Anfrage an die Schnittstelle `https://vmt.hafas.de/openapi/proxy/1.5/journeyDetail`

## 7. Verbindung zwischen 2 Stationen / Haltestellen <a name="item7"></a>

Zeigt die möglichen Verbindungen zwischen 2 Stationen an einem beliebigen Zeitpunkt an.

**URL** : `https://evag-live.wla-backend.de/node/v2/connections`

**Methode** : `POST`

**Content-Type** : `application/json`

**Body-Parameter** : 

`startId` (ID der Start-Station, anders als bei departures die `extId`, bspw: `151063` für `Europaplatz`)

`destId` (ID der Ziel-Station, anders als bei departures die `extId`, bspw: `151063` für `Europaplatz`)

`date` (Datum der Abfahrt / Ankunft nach Schema `YYYY-MM-DD`, bspw: `2021-08-29`)

`time` (Uhrzeit der Abfahrt / Ankunft nach Schema `HH:MM:SS`, bspw: `18:42:00`)

`arrival` (Boolean, wenn Datum und Uhrzeit Abfahrt entsprechen `false`, wenn nicht `true`)

<details>
<summary><b>Erfolgreiche Antwort (Status Code 200), Beispiel</b> (Europaplatz (151063) nach Hauptbahnhof (151196), am 29. August 2021 um 18:44 Uhr)</summary>

```json
{
    "connections": [
        {
            "tariff_result": [
                {
                    "name": "Einzelfahrt",
                    "items": [
                        {
                            "name": "Einzelfahrt",
                            "price": 220,
                            "cur": "EUR",
                            "shpCtx": "{\"VMT\":\"PKMTARIFF\",\"produkt\":\"5\",\"produkttext\":\"Einzelfahrt\",\"dbzielhaltestelle_name\":\"Erfurt, Hauptbahnhof\",\"zielzonetext\":\"Erfurt\",\"dbstarthaltestelle_id\":\"151063\",\"gebietsparameter_nr\":\"11\",\"dbzwischenzonen\":\"10\",\"anzeigetextvia\":\"---\",\"remark\":\"City Erfurt\",\"dbviatext\":\"---\",\"efmprodukt_nr\":\"11\",\"viatext.0\":\"---\",\"dbzielhaltestelle_id\":\"151196\",\"startzone\":\"10\",\"outputchannel\":\"I,M,W,H,D\",\"dbstarthaltestelle_name\":\"Erfurt, Europaplatz\",\"startzonetext\":\"Erfurt\",\"via.0\":\"1\",\"tarifflevel\":\"1\",\"zielzone\":\"10\"}",
                            "via0": "1",
                            "startzone": "10",
                            "zielzone": "10",
                            "product_id": "5"
                        }
                    ]
                }
            ],
            "cancelled": false,
            "rides": [
                {
                    "departure": "2021-08-29T18:55:00",
                    "arrival": "2021-08-29T19:14:00",
                    "departure_rt": "2021-08-29T18:55:00",
                    "arrival_rt": "2021-08-29T19:14:00",
                    "start": "Erfurt, Europaplatz",
                    "startId": "A=1@O=Erfurt, Europaplatz@X=10991866@Y=51012983@U=80@L=151063@",
                    "startExtId": "151063",
                    "startCoordinates": {
                        "latitude": 51.013199,
                        "longitude": 10.991893
                    },
                    "startType": "ST",
                    "destination": "Erfurt, Hauptbahnhof",
                    "destinationId": "A=1@O=Erfurt, Hauptbahnhof@X=11037306@Y=50972056@U=80@L=151196@",
                    "destinationExtId": "151196",
                    "destinationCoordinates": {
                        "latitude": 50.972109,
                        "longitude": 11.037261
                    },
                    "destinationType": "ST",
                    "targetLocation": "Erfurt, Urbicher Kreuz",
                    "stops": [
                        {
                            "Notes": {
                                "Note": [
                                    {
                                        "value": "",
                                        "key": "BN",
                                        "type": "A"
                                    }
                                ]
                            },
                            "name": "Erfurt, Europaplatz",
                            "id": "A=1@O=Erfurt, Europaplatz@X=10991893@Y=51013199@U=80@L=15106304@",
                            "extId": "15106304",
                            "routeIdx": 0,
                            "lon": 10.991893,
                            "lat": 51.013199,
                            "depPrognosisType": "PROGNOSED",
                            "depTime": "18:55",
                            "depDate": "2021-08-29",
                            "rtDepTime": "18:55:00",
                            "rtDepDate": "2021-08-29",
                            "hasMainMast": true,
                            "mainMastId": "A=1@O=Erfurt, Europaplatz@X=10991866@Y=51012983@U=80@L=151063@",
                            "mainMastExtId": "151063"
                        },
                        {
                            "Notes": {
                                "Note": [
                                    {
                                        "value": "",
                                        "key": "BN",
                                        "type": "A"
                                    }
                                ]
                            },
                            "name": "Erfurt, Anger",
                            "id": "A=1@O=Erfurt, Anger@X=11034142@Y=50976253@U=80@L=15118803@",
                            "extId": "15118803",
                            "routeIdx": 12,
                            "lon": 11.034142,
                            "lat": 50.976253,
                            "arrPrognosisType": "PROGNOSED",
                            "depPrognosisType": "PROGNOSED",
                            "depTime": "19:12",
                            "depDate": "2021-08-29",
                            "arrTime": "19:12",
                            "arrDate": "2021-08-29",
                            "rtDepTime": "19:12:00",
                            "rtDepDate": "2021-08-29",
                            "rtArrTime": "19:12:00",
                            "rtArrDate": "2021-08-29",
                            "hasMainMast": true,
                            "mainMastId": "A=1@O=Erfurt, Anger@X=11034277@Y=50976173@U=80@L=151188@",
                            "mainMastExtId": "151188"
                        },
                        {
                            "Notes": {
                                "Note": [
                                    {
                                        "value": "",
                                        "key": "BN",
                                        "type": "A"
                                    }
                                ]
                            },
                            "name": "Erfurt, Hauptbahnhof",
                            "id": "A=1@O=Erfurt, Hauptbahnhof@X=11037261@Y=50972109@U=80@L=15119601@",
                            "extId": "15119601",
                            "routeIdx": 13,
                            "lon": 11.037261,
                            "lat": 50.972109,
                            "arrPrognosisType": "PROGNOSED",
                            "arrTime": "19:14",
                            "arrDate": "2021-08-29",
                            "rtArrTime": "19:14:00",
                            "rtArrDate": "2021-08-29",
                            "hasMainMast": true,
                            "mainMastId": "A=1@O=Erfurt, Hauptbahnhof@X=11037306@Y=50972056@U=80@L=151196@",
                            "mainMastExtId": "151196"
                        }
                    ],
                    "line": "3",
                    "type": "tram",
                    "start_cancelled": false,
                    "destination_cancelled": false
                }
            ]
        }
    ],
    "start": "151063",
    "target": "151196",
    "scrB": "2|OB|MTµ11µ88975µ88975µ88994µ88994µ0µ0µ5µ88963µ1µ-2147483646µ0µ1µ2|PDHµ74eb0910e128b1c8e26db4b8f68fb212|RDµ29082021|RTµ184300|USµ0",
    "scrF": "2|OF|MTµ11µ89036µ89036µ89057µ89057µ0µ0µ5µ89019µ5µ-2147483646µ0µ1µ2|PDHµ74eb0910e128b1c8e26db4b8f68fb212|RDµ29082021|RTµ184300|USµ0"
}
```
</details>


**Sonstiges**: 

Die Abfrage macht eine eigene Anfrage an die Schnittstelle `https://vmt.hafas.de/openapi/proxy/1.5/trip`, wobei `startId` zu `originId` und `arrival` zu `searchForArrival` werden und eine acessId und weitere Parameter hinzugefügt werden. 
