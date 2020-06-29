# weather-module

# Approche

## user stories

- En tant qu'utilisateur, je veux pouvoir chercher une ville pour avoir la météo courante.


## Design

### Vue :

- Moteur de recherche avec autocomplete
- Header graphique avec le nom de la ville, description rapide avec picto + température avec swtich de °C à °F en cliquant sur le symbol (info en rollover)
- List des données diverses dans une table

![ex1](mdcontent/module-ex1.jpg)

### Style :

Clean, outline, popin-flottant, animations basiques.

![ex1](mdcontent/design-ex1.jpg)![ex2](mdcontent/design-ex2.png)![ex2](mdcontent/design-ex3.gif)![ex2](mdcontent/material_design_weather.gif)

## Code

### Datas :

- Les datas sont chargées depuis une api REST qui renvoie un json des données en fonction du nom de ville

ex : [openweathermap.org](https://samples.openweathermap.org/data/2.5/weather?q=London,uk&appid=439d4b804bc8187953eb36d2a8c26a02)

### Composants :

- Popin responsive au centre de l'écran.
 - Form avec api call
 - Composant header
    - titre(string)
    - Description rapide : Picto(png) {mot clé}(string) : {description}(string)
    - temperature(number) + symbole (°C ou °F)
 - Composant datas
    - Table deux colonnes :
  |intitulé|valeur|
  |-|-|
  |Heure lcoale|10:15, 29 Jun 2020|
  |Pression|1012hp|
  |Humidité|80%|
  |Température minimum|279.15°F|
  |Température maximum|281.15°F|
  |Vent|4.1ms à 80deg|
  |Nuages|90%|



## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
