FROM ubuntu:16.04

ADD . /osm-files


RUN apt-get update && apt-get install -y ca-certificates build-essential git cmake pkg-config libbz2-dev libstxxl-dev libstxxl1v5 libxml2-dev libzip-dev libboost-all-dev lua5.2 liblua5.2-dev libtbb-dev wget

RUN git clone https://github.com/Project-OSRM/osrm-backend

RUN cd osrm-backend && mkdir -p build && cd build && cmake .. && cmake --build . &&  cmake --build . --target install

RUN cd osm-files && wget https://s3.amazonaws.com/metro-extracts.mapzen.com/dc-baltimore_maryland.osm.pbf

RUN cd osm-files && osrm-extract dc-baltimore_maryland.osm.pbf -p ../osrm-backend/profiles/bicycle.lua

RUN cd osm-files && osrm-contract dc-baltimore_maryland.osrm

CMD osrm-routed --max-trip-size 500 osm-files/dc-baltimore_maryland.osrm
