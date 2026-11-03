---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
---
🍲 Γαστρονομική Πολιτιστική Κληρονομιά
"Η γεύση αποτελεί έναν από τους πιο ζωντανούς φορείς συλλογικής μνήμης και πολιτισμού."

Καλώς ήρθατε στον ψηφιακό ιστοχώρο προβολής της ελληνικής γαστρονομικής παραδόσεως. Ο παρών ιστότοπος δημιουργήθηκε με σκοπό την καταγραφή, την ανάδειξη και τη διατήρηση της άυλης πολιτιστικής κληρονομιάς που σχετίζεται με τη διατροφή, τα τοπικά προϊόντα και τις παραδοσιακές παρασκευές ανά την Ελλάδα.

Η ελληνική κουζίνα δεν είναι απλώς ένα σύνολο συνταγών, αλλά ένας ζωντανός πολιτισμικός χάρτης. Διαμορφώνεται από τη γεωγραφία, το κλίμα, την ιστορία και τις παραδόσεις κάθε περιοχής, αποτυπώνοντας την ταυτότητα και τη σοφία των τοπικών κοινωνιών στο πέρασμα του χρόνου.

<div id="map" style="height: 550px; width: 100%; border-radius: 8px; margin-bottom: 20px;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
var map = L.map('map').setView([38.5, 20.7], 6);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
maxZoom: 19,
attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

var poiUrls = {
        {% for poi in site.pois %}
            {% if poi.wikidatum %}
"{{ poi.wikidatum }}": "{{ poi.url | relative_url }}",
            {% endif %}
        {% endfor %}
    };

var wikidataIds = Object.keys(poiUrls).map(id => "wd:" + id);

if (wikidataIds.length === 0) return;

var idsString = wikidataIds.join(" ");
var sparqlQuery = `
        SELECT ?item ?itemLabel ?coord WHERE {
            VALUES ?item { ${idsString} }
            ?item wdt:P625 ?coord .
            SERVICE wikibase:label { bd:serviceParam wikibase:language "el,en". }
        }
    `;

var endpointUrl = 'https://query.wikidata.org/sparql?query=' + encodeURIComponent(sparqlQuery);

fetch(endpointUrl, { headers: { 'Accept': 'application/sparql-results+json' } })
        .then(response => response.json())
        .then(data => {
var bounds = [];

            data.results.bindings.forEach(function(row) {
var point = row.coord.value.replace("Point(", "").replace(")", "").split(" ");
var lon = parseFloat(point[0]);
var lat = parseFloat(point[1]);
var title = row.itemLabel.value;

var qid = row.item.value.split('/').pop();
var pageUrl = poiUrls[qid];

                bounds.push([lat, lon]);

var marker = L.marker([lat, lon]).addTo(map);

if (pageUrl) {
                    marker.bindPopup("<b><a href='" + pageUrl + "'>" + title + "</a></b>");

                    marker.on('click', function() {
                        window.location.href = pageUrl;
                    });
                } else {
                    marker.bindPopup("<b>" + title + "</b>");
                }
            });

if (bounds.length > 0) {
                map.fitBounds(bounds, { padding: [30, 30] });
            }
        })
        .catch(error => console.error('Σφάλμα:', error));
});
</script>