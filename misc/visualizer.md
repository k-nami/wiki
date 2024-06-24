
# time-serires visuzalizer in geodetic coordinates

## csv+Leaflet.js w/ gpt-4o

* シンプルな実装
* csvがメモリに乗る程度なら比較的軽量
* Laptop実行時（RAM16GB）
  * N<100, T<60, 軽量
  * N=1k, T=600, loadに1min程度
  * N=1k, T=6k, loadで落ちる

trajecotry.csv
```
id	time	lat	lng
0	0	35.681236	139.767125
0	1	35.681336	139.767225
...
```

index.html
```
<!DOCTY html>
<html>
<head>
    <title>Leaflet Animation Example</title>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <style>
        #map {
            height: 600px;
        }
        #controls {
            position: absolute;
            top: 10px;
            left: 10px;
            background: white;
            padding: 10px;
            z-index: 1000;
        }
        #controls button, #controls input {
            margin: 5px;
        }
    </style>
</head>
<body>
    <div id="map"></div>
    <div id="controls">
        <button id="startButton">Start</button>
        <button id="stopButton">Stop</button>
        <input type="range" id="timeSlider" min="0" max="60" value="0">
    </div>
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.0/papaparse.min.js"></script>
    <script>
        // マップの初期化
        var map = L.map('map').setView([35.681236, 139.767125], 13); // 東京駅の座標

        // OpenStreetMapのタイルレイヤーを追加
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 19,
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        // 空のマーカーの配列を作成
        var markers = {};
        var trajectories = {};

        // CSVファイルを読み込み、マーカーの座標を設定
        Papa.parse("trajectory.csv", {
            download: true,
            header: true,
            dynamicTyping: true,
            complete: function(results) {
                results.data.forEach(function(row) {
                    if (!trajectories[row.id]) {
                        trajectories[row.id] = [];
                    }
                    trajectories[row.id].push([row.time, row.lat, row.lng]);
                });
                initializeMarkers();
            }
        });

        // マーカーを初期化
        function initializeMarkers() {
            for (var id in trajectories) {
                var initialPosition = trajectories[id][0];
                markers[id] = L.circleMarker([initialPosition[1], initialPosition[2]], {
                    radius: 10,
                    color: 'red',
                    fillColor: '#f03',
                    fillOpacity: 0.5
                }).addTo(map);
            }
        }

        var animationId;
        var currentTime = 0;

        // アニメーション関数
        function animateMarkers() {
            for (var id in markers) {
                var position = trajectories[id].find(function(point) {
                    return point[0] === currentTime;
                });
                if (position) {
                    markers[id].setLatLng([position[1], position[2]]);
                }
            }
            currentTime++;
            if (currentTime > 60) currentTime = 0;
            animationId = requestAnimationFrame(animateMarkers);
        }

        // スタートボタンのイベントリスナー
        document.getElementById('startButton').addEventListener('click', function() {
            if (!animationId) {
                animateMarkers();
            }
        });

        // ストップボタンのイベントリスナー
        document.getElementById('stopButton').addEventListener('click', function() {
            if (animationId) {
                cancelAnimationFrame(animationId);
                animationId = null;
            }
        });

        // タイムスライダーのイベントリスナー
        document.getElementById('timeSlider').addEventListener('input', function() {
            currentTime = parseInt(this.value);
            for (var id in markers) {
                var position = trajectories[id].find(function(point) {
                    return point[0] === currentTime;
                });
                if (position) {
                    markers[id].setLatLng([position[1], position[2]]);
                }
            }
        });
    </script>
</body>
</html>

```