wget https://download.geofabrik.de/asia/india/western-zone-latest.osm.pbf

"docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/western-zone-latest.osm.pbf"

"docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-partition /data/western-zone-latest.osrm"
"docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-customize /data/western-zone-latest.osrm"

"docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/western-zone-latest.osrm"

# url:port /route/v1/driving/72.8777,19.0760;72.5714,23.0225?steps=false


<!-- optional-not work -->
docker run -p 9966:9966 osrm/osrm-frontend




<!-- for marging -->

docker run -it -v "${PWD}:/data" debian:bullseye bash -c "apt-get update && apt-get install -y osmium-tool && osmium merge /data/northern-zone-latest.osm.pbf /data/western-zone-latest.osm.pbf -o /data/northwest.osm.pbf"

docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/northwest.osm.pbf
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-partition /data/northwest.osrm
docker run -t -v "${PWD}:/data" osrm/osrm-backend osrm-customize /data/northwest.osrm
docker run -t -i -p 5000:5000 -v "${PWD}:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/northwest.osrm
















<!-- Options -->

In addition to the general options the following options are supported for this service:

Option	Values	Description
sources	{index};{index}[;{index} ...] or all (default)	Use location with given index as source.
destinations	{index};{index}[;{index} ...] or all (default)	Use location with given index as destination.
annotations	duration (default), distance, or duration,distance	Return the requested table or tables in response.
fallback_speed	double > 0	If no route found between a source/destination pair, calculate the as-the-crow-flies distance, then use this speed to estimate duration.
fallback_coordinate	input (default), or snapped	When using a fallback_speed, use the user-supplied coordinate (input), or the snapped location (snapped) for calculating distances.
scale_factor	double > 0	Use in conjunction with annotations=durations. Scales the table duration values by this number.
Unlike other array encoded options, the length of sources and destinations can be smaller or equal to number of input locations;

With skip_waypoints set to true, both sources and destinations arrays will be skipped.

Example:

sources=0;5;7&destinations=5;1;4;2;3;6
Element	Values
index	0 <= integer < #locations
Example Request
# Returns a 3x3 duration matrix:
curl 'http://router.project-osrm.org/table/v1/driving/13.388860,52.517037;13.397634,52.529407;13.428555,52.523219'

# Returns a 1x3 duration matrix
curl 'http://router.project-osrm.org/table/v1/driving/13.388860,52.517037;13.397634,52.529407;13.428555,52.523219?sources=0'

# Returns a asymmetric 3x2 duration matrix with from the polyline encoded locations `qikdcB}~dpXkkHz`:
curl 'http://router.project-osrm.org/table/v1/driving/polyline(egs_Iq_aqAppHzbHulFzeMe`EuvKpnCglA)?sources=0;1;3&destinations=2;4'

# Returns a 3x3 duration matrix:
curl 'http://router.project-osrm.org/table/v1/driving/13.388860,52.517037;13.397634,52.529407;13.428555,52.523219?annotations=duration'

# Returns a 3x3 distance matrix for CH:
curl 'http://router.project-osrm.org/table/v1/driving/13.388860,52.517037;13.397634,52.529407;13.428555,52.523219?annotations=distance'

# Returns a 3x3 duration matrix and a 3x3 distance matrix for CH:
curl 'http://router.project-osrm.org/table/v1/driving/13.388860,52.517037;13.397634,52.529407;13.428555,52.523219?annotations=distance,duration'
