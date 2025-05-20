# Integracja SLAM w Projekcie Wózka 

## Wprowadzenie
W ramach projektu autonomicznego wózka mobilnego, system SLAM (Simultaneous Localization and Mapping) będzie wykorzystywany do jednoczesnego tworzenia mapy otoczenia oraz określania pozycji wózka w tej mapie w czasie rzeczywistym. Jest to kluczowy element umożliwiający autonomiczną nawigację, unikanie przeszkód i podejmowanie decyzji ruchowych.

---

## Wejście (Input) do SLAM
System SLAM wymaga danych z kilku różnych czujników:

- **Obraz RGB** z kamery (np. USB lub RealSense) — do ekstrakcji punktów charakterystycznych (feature points).
- **Dane głębokości (depth)** z czujnika ToF lub RGB-D — używane do budowania chmury punktów 3D.
- **(Opcjonalnie) Dane z IMU** — przyspieszenie i orientacja pomagają w estymacji ruchu.
- **Odometria (jeśli dostępna)** — dane z enkoderów kół lub innego systemu odometrii.

---

## Wyjście (Output) z SLAM
Po przetworzeniu danych, SLAM generuje:

- **Mapę otoczenia**:
  - 2D: siatka `nav_msgs/OccupancyGrid` do planowania trasy.
  - 3D: `sensor_msgs/PointCloud2` do wizualizacji i analizy przestrzennej.
- **Pozycję i orientację robota**:
  - Współrzędne `geometry_msgs/PoseStamped` w układzie mapy.
- **Transformacje tf**:
  - Relacje między układami współrzędnych: `base_link -> odom -> map`.

---

## Integracja z ROS2
- SLAM działa jako osobny node ROS2 (np. `rtabmap_ros`, `orb_slam2_ros`).
- Komunikuje się przez standardowe topiki:
  - `/camera/color/image_raw`, `/camera/depth/image_raw`
  - `/tf`, `/odom`
- Output z SLAM udostępniany jest innym komponentom systemu, np. planowaniu trasy (`nav2`).

## Wizualizacja i debugowanie
- Do wizualizacji danych i mapy wykorzystywany jest **RViz2**:
  - Widok mapy 2D lub chmury punktów 3D.
  - Aktualna pozycja robota, transformacje tf, sensory.

---

## Wymagania techniczne
- **ROS2 (np. Humble)** z obsługą `colcon`, `rviz2`, `tf2`.
- **Kamera RGB oraz depth (ToF lub RGB-D)**.
- **Kompatybilny system SLAM** (np. `rtabmap_ros`).
- **Komputer z akceleracją GPU** (zalecane dla przetwarzania w czasie rzeczywistym).
- **Poprawna synchronizacja i kalibracja sensorów**.