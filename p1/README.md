SUPSI 2026  
Corso d’interaction design, CV429.01  
Docenti: A. Gysin, G. Profeta  

Progetto 1: La conquista dello spazio

# Hubble space telescope
Autore: Laura Pantani \
[hubble space telescope](https://laura-pantani.github.io/NASA-P1/p1/)


## Introduzione e tema
Il mio progetto intende rendere omaggio al satellite Hubble. Dal 1985 fino a oggi, grazie al suo contributo, abbiamo ottenuto le prime immagini dettagliate dello spazio, fondamentali per lo studio e la comprensione dell’universo.


## Riferimenti progettuali
Al momento non sono stati individuati riferimenti progettuali specifici.

 


## Design dell’interfaccia e modalità di interazione
Il progetto si sviluppa come una piattaforma documentativa dedicata alla storia di Hubble. In una prima fase, l’utente può esplorare contenuti informativi sul satellite e sulla sua evoluzione nel tempo. Successivamente, viene presentata una sezione di archivio che raccoglie una selezione di immagini dello spazio catturate da Hubble, consultabili in modo semplice e intuitivo.




## Tecnologia usata
Il progetto è stato realizzato con **HTML**, **CSS** e **JavaScript**, lavorando in Visual Studio Code con il supporto di Gemini Code Assist. La pagina principale è costruita come racconto verticale: testi, immagini, animazioni leggere e navigazione a capitoli accompagnano l'utente nella storia di Hubble.

Per la parte visiva ho usato CSS personalizzato, font locali e variabili cromatiche, in modo da mantenere un'identità coerente e scura, vicina all'immaginario dello spazio.

```css
@font-face {
	font-family: "Kantumruy Pro";
  src: url("./fonts/KantumruyPro-VariableFont_wght.ttf") format("truetype");
  font-weight: 100 900;
}

:root {
	--bg: #020202;
  --text: #f2efe8;
  --muted: rgba(242, 239, 232, 0.72);
  --mono: "Komunal Mono", "Courier New", Courier, monospace;
  --sans: "Kantumruy Pro", Arial, sans-serif;
}
```

Nella pagina principale è presente anche un piccolo sistema di stelle disegnato con **Canvas**, che reagisce allo scroll e crea un movimento di profondità.

```javascript
const canvas = document.querySelector(".starfield");
const context = canvas.getContext("2d");
const stars = [];

function drawStars() {
	context.clearRect(0, 0, canvas.width, canvas.height);
  stars.forEach((star) => {
	  const offsetY = (window.scrollY * star.speed) % (window.innerHeight + 120);
    const y = (star.y + offsetY) % (window.innerHeight + 120) - 60;
    context.globalAlpha = star.alpha;
    context.fillRect(star.x, y, star.size, star.size);
  });
}
```

La sezione archivio utilizza invece **Three.js**, una libreria JavaScript per creare scene 3D nel browser. Le immagini di Hubble vengono caricate come texture e posizionate nello spazio in base a coordinate astronomiche semplificate, così l'utente può ruotare la scena e navigare l'archivio in modo interattivo.

```javascript
const scene = new THREE.Scene();
const universe = new THREE.Group();
scene.add(universe);

const camera = new THREE.PerspectiveCamera(
	75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
document.getElementById("scene-root").appendChild(renderer.domElement);
```

Le immagini dell'archivio sono organizzate in un database locale JavaScript, con titolo, descrizione, file immagine, coordinate e tag. Questo permette di filtrare i contenuti per categorie come nebulose, galassie, pianeti, stelle e ammassi.

```javascript
const HUBBLE_DB = {
	timeline: [
		{
			year: 1,
      title: "Great Storm on Saturn",
      desc: "Hubble observes a rare and massive white dust storm...",
      url: "img/year-1-major-storm-on-saturn_54398802956_o.jpg",
      ra: 18.5,
      dec: -22.5,
      tags: ["pianeti"]
    }
  ]
};
```

Questo frammento mostra il filtro per categoria e la creazione delle immagini 3D nell'archivio:

```javascript
const allItems = HUBBLE_DB.timeline;
const items = tag === "tutte"
  ? allItems
  : allItems.filter(item => item.tags && item.tags.includes(tag));

items.forEach((item) => {
	textureLoader.load(item.url, (texture) => {
		const aspect = texture.image.width / texture.image.height;
    const mesh = new THREE.Mesh(
		new THREE.PlaneGeometry(12 * aspect, 12),
      new THREE.MeshBasicMaterial({ map: texture, side: THREE.DoubleSide })
    );

    const radius = 60;
    const raRad = (item.ra / 24) * Math.PI * 2;
    const decRad = (item.dec * Math.PI) / 180;

    mesh.position.set(
		radius * Math.cos(decRad) * Math.sin(raRad),
      radius * Math.sin(decRad),
      -radius * Math.cos(decRad) * Math.cos(raRad)
    );

    mesh.lookAt(0, 0, 0);
    universe.add(mesh);
  });
});
		[<img src="doc/cards.gif" width="500" alt="Magic trick">]()
		
		## Video del progetto
		Registrazione dello schermo del sito durante la navigazione del sito:
		
		<video src="../Registrazione_schermo.mp4" controls width="700"></video>
		
		[Guarda la registrazione](../Registrazione_schermo.mp4)
```

## Target e contesto d’uso
Il target del mio progetto sono i ragazzi e il possibile contesto d'uso é educativo e scolastico.

[<img src="doc/munari.jpg" width="300" alt="Supplemento al dizionario italiano">]()
