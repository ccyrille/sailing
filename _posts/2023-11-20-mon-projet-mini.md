---
layout: post
title:  "Episode 0 - Le projet"
author: cyrille
tags: [ Aventure Mini ]
image: assets/images/classe-mini-650-1.jpg
comments: false
custom_js:
- https://api.mapbox.com/mapbox-gl-js/v3.0.0/mapbox-gl.js
custom_styles:
- https://api.mapbox.com/mapbox-gl-js/v3.0.0/mapbox-gl.css

---

Juin 2023, je viens de décider de m'engager dans un projet Mini ⛵

**La Classe Mini c'est quoi ?** Ce sont les plus petits bateaux de courses au large. Malgré leurs 6.50m de long, ils sont capables de s'engager sur des courses océaniques. Les courses se font le plus souvent en solitaire, mais certaines se font aussi en double.

![]({{ site.baseurl }}/assets/images/mini-transat-1.jpg)

**Et la Mini Transat ?** Une course transatlantique à la voile sur Mini, en solitaire et sans assistance. Organisée chaque deux ans depuis 1977, Elle a vu naître les plus grands skippers tels que Michel Desjoyeaux, Ellen MacArthur et Yannick Bestaven. <i>« C'est la Mini Transat qui m'a donné le goût du large. Je ne l'oublierai jamais »</i> disait Ellen MacArthur.

![]({{ site.baseurl }}/assets/images/mini-transat-2.jpg)

**Pourquoi tu te lances là-dedans ?** Les 10 ans de <a href="https://www.youtube.com/watch?v=RnD7iG3fY1g" target="_blank">cette aventure exceptionnelle qu'a été Klaxit</a> m'ont fait mettre de côté une passion que j'ai depuis toujours, la voile. Dès l'âge de 5 ans, mes parents m'ont mis sur un bateau. Toute ma jeunesse j'ai eu la chance de naviguer sur la côte Atlantique et en Méditérranée. Puis je me suis engagé sur des régates en J80, notamment le Grand Prix du Crouesty et le Spi Ouest. Je veux désormais rattraper le temps perdu et quoi de mieux pour ça qu'un projet hyper intensif dans le domaine ?

![]({{ site.baseurl }}/assets/images/cyrille-petit-1.jpg)

**Par quel bout le prendre ?** Franchement je n'y connais rien de rien. Alors je contacte ceux qui savent. Une dizaine de skippers en tout. Le temps qui m'est accordé par ces skippers me fait tout de suite sentir que je serai bien dans cet environnement. Ces gens sont passionnés, engagés et prêts à donner de leur temps pour partager leur expérience. 

> Je commence à comprendre que la route à venir est longue mais qu'elle est bien pour moi ! 

Car avant de pouvoir atteindre le « graal », c'est un vrai parcours du combattant :
* **Se former** aux nombreux savoir-faires indispensables pour une telle aventure.
* **S'entraîner** pour acquérir le physique et les automatismes qui resteront même après plusieurs jours de mer, à peu dormir et manger sur le pouce.
* **Participer aux courses, et les finir dans les temps,** soit au moins 3 courses qui prennent entre 7j et 1 mois, chaque année pendant 2 ans !
* **Faire sa "qualif"**, un tour de 1000MN (1800km+) sans escale et en totale autonomie pendant 6-12 jours, entre l'Île de Ré et le Sud de l'Irlande 👇 
<div id="map" style="height: 400px"></div>
<div class="legend">Parcours de qualification AKA « la qualif »</div>

Sans oublier la structuration et la communication autour du projet !

**Et tu fais ça tout seul ?** Non, j'ai intégré le <a href="https://laturballecourseaularge.com/" target="_blank">pôle de Course au Large de la Turballe</a>. Nous sommes 25 skippers au pôle à nous entraîner et nous entraider... une vraie équipe ! En plus de l'indispensable soutien de ma chérie Valentina Grampa ❤️

En lecteur attentif, vous n'aurez pas manqué de noter l'écart entre cette publication et l'avancement du projet alors...

La suite très vite 😉

<script>
mapboxgl.accessToken = 'pk.eyJ1IjoiY3lyaWxsZWMiLCJhIjoiY2xwbWlwMjAwMDlmdzJsbXM0aGZ4eTlpdSJ9.0aNLOUJ5iSmRB2i10PtWDQ';

const map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/cyrillec/clpmiq6ul00yp01pg1rbb3n4c',
    center: [-4.6, 48.7],
    zoom: 4.5
});

// Generated using https://labs.mapbox.com/bezier-curves/
// Coninberg Buoy (52.0533,-6.64278)
// Ile-de-Ré bridge (46.1727, -1.2405)
// Plateau de Rochebonne (46.18880, -2.45498)

function uuidv4() {
  return "10000000-1000-4000-8000-100000000000".replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
  );
}

function  addMarker(marker, map){
    const id = "marker-" + uuidv4();
    map.loadImage(
        marker[3],
        (error, image) => {
            if (error) throw error;

            console.log(id);

            const imgId = 'custom-marker-' + uuidv4();
            map.addImage(imgId, image);
            
            // Add a GeoJSON source with 2 points
            console.log(marker[2]);

            map.addSource(id, {
                'type': 'geojson',
                'data': {
                'type': 'FeatureCollection',
                'features': [
                    {
                        'type': 'Feature',
                        'geometry': {
                            'type': 'Point',
                            'coordinates': [
                                marker[2], marker[1]
                            ]
                        },
                        'properties': {
                            'title': marker[0]
                        }
                    }
                ]
            }});
                
            // Add a symbol layer
            map.addLayer({
                'id': id,
                'type': 'symbol',
                'source': id,
                'layout': {
                    'icon-image': imgId,
                    // get the title name from the source's "title" property
                    'text-field': ['get', 'title'],
                    'text-font': [
                        'Open Sans Semibold',
                        'Arial Unicode MS Bold'
                    ],
                    'text-size': 12,
                    'text-offset': [0, 1.25],
                    'text-anchor': 'top',
                    'icon-size': 0.25,
                }
            });
        }
    );
}

map.on('load', () => {
    const nav = new mapboxgl.NavigationControl({
        visualizePitch: true
    });
    map.addControl(nav, 'bottom-right');

    map.addSource('qualif-route', {
        type: 'geojson',
        data: '{{ site.baseurl }}/assets/routes/qualif-route.geojson'
    });

    map.addLayer({
        'id': 'qualif-route',
        'type': 'line',
        'source': 'qualif-route',
        'layout': {
            'line-join': 'round',
            'line-cap': 'round'
        },
        'paint': {
            'line-color': '#aa2e88',
            'line-width': 3
        }
    });

    const markers = [
        ['Cardinale de Coninberg',52.0533,-6.64278, '{{ site.baseurl }}/assets/images/marker-target.png'],
        ['Ile-de-Ré',46.17360, -1.3386, '{{ site.baseurl }}/assets/images/marker-target.png'],
        ['Plateau de Rochebonne',46.18880,-2.45498, '{{ site.baseurl }}/assets/images/marker-target.png']
    ]

    markers.forEach(function(marker, index) {
        addMarker(marker, map);
    });
});
</script>
