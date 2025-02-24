<!DOCTYPE html>
<html>
<head>
    <title>موقع تحديد الموقع الحالي</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
    <style>
        #map {
            height: 400px;
            width: 100%;
        }
        .container {
            padding: 20px;
        }
        .coordinates-box {
            margin-top: 10px;
            width: 100%;
            padding: 10px;
            text-align: center;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="map"></div>
        <input type="text" id="coordinates" class="coordinates-box" readonly>
    </div>

    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
    <script>
        let map;
        let marker;

        function initMap(latitude, longitude) {
            map = L.map('map').setView([latitude, longitude], 15);

            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);

            marker = L.marker([latitude, longitude]).addTo(map)
                .bindPopup('موقعك الحالي')
                .openPopup();

            // التعديل هنا: عرض الإحداثيات فقط بدون نصوص
            document.getElementById('coordinates').value = 
                `${latitude.toFixed(6)}, ${longitude.toFixed(6)}`;
        }

        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    const lat = position.coords.latitude;
                    const lng = position.coords.longitude;
                    initMap(lat, lng);
                },
                (error) => {
                    alert('الرجاء السماح بالوصول إلى الموقع الجغرافي');
                    console.error('Error:', error);
                }
            );
        } else {
            alert('المتصفح لا يدعم خدمة تحديد الموقع');
        }
    </script>
</body>
</html>
