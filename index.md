---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
---
🍲 Γαστρονομική Πολιτιστική Κληρονομιά
"Η γεύση αποτελεί έναν από τους πιο ζωντανούς φορείς συλλογικής μνήμης και πολιτισμού."

Καλώς ήρθατε στον ψηφιακό ιστοχώρο προβολής της ελληνικής γαστρονομικής παραδόσεως. Ο παρών ιστότοπος δημιουργήθηκε με σκοπό την καταγραφή, την ανάδειξη και τη διατήρηση της άυλης πολιτιστικής κληρονομιάς που σχετίζεται με τη διατροφή, τα τοπικά προϊόντα και τις παραδοσιακές παρασκευές ανά την Ελλάδα.

Η ελληνική κουζίνα δεν είναι απλώς ένα σύνολο συνταγών, αλλά ένας ζωντανός πολιτισμικός χάρτης. Διαμορφώνεται από τη γεωγραφία, το κλίμα, την ιστορία και τις παραδόσεις κάθε περιοχής, αποτυπώνοντας την ταυτότητα και τη σοφία των τοπικών κοινωνιών στο πέρασμα του χρόνου.

<div id="map" style="height: 450px; width: 100%; border-radius: 8px; margin-top: 20px;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function() {
    // 1. Αρχικοποίηση χάρτη (συντεταγμένες π.χ. για Κέρκυρα)
    var map = L.map('map').setView([39.6243, 19.9217], 12);
    
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 19,
        attribution: '&copy; OpenStreetMap'
    }).addTo(map);

    {% for poi in site.pois %}
      {% if poi.latitude and poi.longitude %}
        L.marker([{{ poi.latitude }}, {{ poi.longitude }}])
         .addTo(map)
         .bindPopup("<b><a href='{{ poi.url | relative_url }}'>{{ poi.title }}</a></b>");
      {% endif %}
    {% endfor %}
});
</script>
