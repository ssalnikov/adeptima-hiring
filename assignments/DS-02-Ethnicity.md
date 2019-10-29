## Ethnicity Detection by Name API
Create microservice for detecting nationality by first and last name. Gender, Origin featurea are optional.

* NamSor software classifies personal names accurately by gender, country of origin, or ethnicity http://www.namsor.com/
* Name Ethnicity Classifier http://www.textmap.com/ethnicity/
* NamePrism : a name-based nationality classifier http://www.name-prism.com/about

```json
{
    "George Washington": [
        { "scores":
            [ {"score": "0.07", "ethnicity": "Asian"},
              {"score": "0.00", "ethnicity": "GreaterAfrican"},
              {"score": "0.93", "ethnicity": "GreaterEuropean"}],
          "best":"GreaterEuropean" },
        { "scores":
            [ {"score": "1.00", "ethnicity": "British"},
              {"score": "0.00", "ethnicity": "Jewish"}, 
              {"score": "0.00", "ethnicity": "WestEuropean"}, 
              {"score": "0.00", "ethnicity": "EastEuropean"}],
          "best":"British" }
    ],
    "John Smith": [
        { "scores":
            [ {"score": "0.00", "ethnicity": "Asian"},
              {"score": "0.00", "ethnicity": "GreaterAfrican"},
              {"score": "1.00", "ethnicity": "GreaterEuropean"}],
          "best":"GreaterEuropean" },
        { "scores":
            [ {"score": "1.00", "ethnicity": "British"},
              {"score": "0.00", "ethnicity": "Jewish"},
              {"score": "0.00", "ethnicity": "WestEuropean"},
              {"score": "0.00", "ethnicity": "EastEuropean"}],
          "best": "British" }
    ]
}
	
```

### Repos
* https://github.com/reactima/reactima-demo-ethnic-api